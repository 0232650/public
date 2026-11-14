(Get-ScheduledTask -TaskName "LabJuiceShop").Actions | Format-List Execute, Arguments, WorkingDirectory
Get-Content "C:\Lab\start-juice-shop.cmd"
Test-Path "C:\Lab\juice-shop\package.json"
Test-Path "C:\Program Files\nodejs\npm.cmd"




$ErrorActionPreference = "Stop"

if (!(Test-Path "C:\Lab\juice-shop\package.json")) {
    throw "Nie znaleziono aplikacji w C:\Lab\juice-shop"
}
if (!(Test-Path "C:\Program Files\nodejs\npm.cmd")) {
    throw "Nie znaleziono npm w domyslnym katalogu Node.js"
}

@'
@echo off
cd /d C:\Lab\juice-shop
call "C:\Program Files\nodejs\npm.cmd" start >> "C:\Lab\juice-shop\startup.log" 2>&1
'@ | Set-Content "C:\Lab\start-juice-shop.cmd" -Encoding ASCII

icacls "C:\Lab\juice-shop" /grant '*S-1-5-19:(OI)(CI)M' /T /Q
if ($LASTEXITCODE -ne 0) { throw "Blad uprawnien folderu aplikacji" }

icacls "C:\Lab\start-juice-shop.cmd" /grant '*S-1-5-19:RX' /Q
if ($LASTEXITCODE -ne 0) { throw "Blad uprawnien pliku startowego" }

$action = New-ScheduledTaskAction -Execute "$env:SystemRoot\System32\cmd.exe" -Argument "/d /c C:\Lab\start-juice-shop.cmd" -WorkingDirectory "C:\Lab\juice-shop"
$trigger = New-ScheduledTaskTrigger -AtStartup
$principal = New-ScheduledTaskPrincipal -UserId "S-1-5-19" -LogonType ServiceAccount -RunLevel Limited
$settings = New-ScheduledTaskSettingsSet -ExecutionTimeLimit ([TimeSpan]::Zero) -MultipleInstances IgnoreNew -RestartCount 3 -RestartInterval (New-TimeSpan -Minutes 1)

Register-ScheduledTask -TaskName "LabJuiceShop" -Action $action -Trigger $trigger -Principal $principal -Settings $settings -Force
Start-ScheduledTask -TaskName "LabJuiceShop"






New-NetFirewallRule -DisplayName "LAB Juice Shop TCP 3000" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 3000 -RemoteAddress 10.60.0.0/24 -Profile Any




foreach ($p in (Get-AppxPackage -AllUsers -Name 'Microsoft.Winget.Source')) {
    Remove-AppxPackage -Package $p.PackageFullName -AllUsers -ErrorAction Stop
}

foreach ($p in (Get-AppxProvisionedPackage -Online | Where-Object DisplayName -eq 'Microsoft.Winget.Source')) {
    Remove-AppxProvisionedPackage -Online -PackageName $p.PackageName -ErrorAction Stop
}





https://github.com/juice-shop/juice-shop/releases/tag/v20.4.0?utm_source=chatgpt.com
