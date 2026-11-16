$t = Get-ScheduledTask -TaskName "LabJuiceShop"

$t | Select-Object TaskName, State
$t.Actions | Format-List Execute, Arguments, WorkingDirectory
$t.Principal | Format-List UserId, LogonType, RunLevel

$i = Get-ScheduledTaskInfo -TaskName "LabJuiceShop"
$i | Format-List LastRunTime, LastTaskResult
"Kod HEX: 0x{0:X8}" -f $i.LastTaskResult

"Plik startowy istnieje:"
Test-Path "C:\Lab\start-juice-shop-direct.cmd"

"Zawartosc pliku startowego:"
Get-Content "C:\Lab\start-juice-shop-direct.cmd" -ErrorAction Continue

"Plik aplikacji istnieje:"
Test-Path "C:\Lab\juice-shop\build\app.js"
