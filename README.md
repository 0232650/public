Get-ScheduledTask -TaskName "LabJuiceShop" | Select-Object TaskName,State


(Get-ScheduledTask -TaskName "LabJuiceShop").Actions | Format-List *
Get-ScheduledTaskInfo -TaskName "LabJuiceShop" | Format-List LastRunTime,LastTaskResult
Test-Path "C:\Lab\start-juice-shop-direct.cmd"
