$ErrorActionPreference = "Stop"

$node = "C:\Program Files\nodejs\node.exe"
$app = "C:\Lab\juice-shop\build\app.js"
$launcher = "C:\Lab\start-juice-shop-direct.cmd"
$log = "C:\Lab\juice-shop\startup-direct.log"

# Sprawdzenie plikow przed zmiana
foreach ($path in @($node, $app)) {
    if (-not (Test-Path -LiteralPath $path -PathType Leaf)) {
        throw "Brak pliku: $path. Nie zmieniono zadania."
    }
}

# Kopia konfiguracji zadania
$backup = "C:\Lab\LabJuiceShop-$(Get-Date -Format yyyyMMdd-HHmmss).xml"
Export-ScheduledTask -TaskName "LabJuiceShop" |
    Set-Content -LiteralPath $backup -Encoding Unicode

# Nowy sposob startu i osobny log
$lines = @(
    "@echo off"
    'cd /d "C:\Lab\juice-shop"'
    "if errorlevel 1 exit /b 11"
    'echo [%date% %time%] START >> "C:\Lab\juice-shop\startup-direct.log"'
    '"C:\Program Files\nodejs\node.exe" "C:\Lab\juice-shop\build\app.js" >> "C:\Lab\juice-shop\startup-direct.log" 2>&1'
    "exit /b %errorlevel%"
)
$lines | Set-Content -LiteralPath $launcher -Encoding ASCII

Stop-ScheduledTask -TaskName "LabJuiceShop"

$action = New-ScheduledTaskAction `
    -Execute "$env:windir\System32\cmd.exe" `
    -Argument "/d /c C:\Lab\start-juice-shop-direct.cmd" `
    -WorkingDirectory "C:\Lab\juice-shop"

Set-ScheduledTask -TaskName "LabJuiceShop" -Action $action |
    Out-Null

Start-ScheduledTask -TaskName "LabJuiceShop"
Start-Sleep -Seconds 15

Get-ScheduledTask -TaskName "LabJuiceShop" |
    Select-Object TaskName, State

Get-Content -LiteralPath $log -Tail 20
curl.exe -I --max-time 10 http://127.0.0.1:3000/
