powershell.exe -NoProfile -ExecutionPolicy Bypass -File "F:\MASTER.PS1" -IsolatedLabVM -CheckOnly
Get-NetRoute | Where-Object { $_.DestinationPrefix -in @("0.0.0.0/0","::/0") } | Format-Table InterfaceIndex,InterfaceAlias,DestinationPrefix,NextHop -AutoSize
