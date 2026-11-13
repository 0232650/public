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


$adresy = @(
    "0.0.0.0-10.60.0.10",
    "10.60.0.12-10.60.0.253",
    "10.60.0.255-255.255.255.255",
    "::/0",
    "0.0.0.1-10.60.0.10",
    "10.60.0.255-255.255.255.254"
)

foreach ($adres in $adresy) {
    try {
        $test = New-Object -ComObject HNetCfg.FWRule
        $test.RemoteAddresses = $adres
        Write-Host "OK: $adres"
    } catch {
        Write-Host "BLAD: $adres"
    }
}
try { $regulaDiag = New-Object -ComObject HNetCfg.FWRule; $regulaDiag.Protocol = 6; $regulaDiag.RemoteAddresses = "10.60.0.11"; Write-Host "OK" } catch { $_.Exception.ToString() }
