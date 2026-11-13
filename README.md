powershell.exe -NoProfile -ExecutionPolicy Bypass -File "F:\MASTER.PS1" -IsolatedLabVM -CheckOnly
Get-NetRoute | Where-Object { $_.DestinationPrefix -in @("0.0.0.0/0","::/0") } | Format-Table InterfaceIndex,InterfaceAlias,DestinationPrefix,NextHop -AutoSize




Remove-NetRoute -InterfaceIndex 6 -DestinationPrefix "0.0.0.0/0" -NextHop "10.60.0.1" -PolicyStore PersistentStore -Confirm:$false -ErrorAction SilentlyContinue

Remove-NetRoute -InterfaceIndex 6 -DestinationPrefix "0.0.0.0/0" -NextHop "10.60.0.1" -PolicyStore ActiveStore -Confirm:$false -ErrorAction SilentlyContinue



powershell.exe -NoProfile -ExecutionPolicy Bypass -File "F:\MASTER.PS1" -IsolatedLabVM -CheckOnly



Get-NetFirewallRule -Group "NovaLabStart" -ErrorAction SilentlyContinue |
    Where-Object {
        $_.Name -in @(
            "NovaLabStart-Allow",
            "NovaLabStart-BlockOthers",
            "NovaLabStart-MasterSeal"
        )
    } |
    Remove-NetFirewallRule
