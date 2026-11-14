New-NetFirewallRule -DisplayName "LAB Juice Shop TCP 3000" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 3000 -RemoteAddress 10.60.0.0/24 -Profile Any




foreach ($p in (Get-AppxPackage -AllUsers -Name 'Microsoft.Winget.Source')) {
    Remove-AppxPackage -Package $p.PackageFullName -AllUsers -ErrorAction Stop
}

foreach ($p in (Get-AppxProvisionedPackage -Online | Where-Object DisplayName -eq 'Microsoft.Winget.Source')) {
    Remove-AppxProvisionedPackage -Online -PackageName $p.PackageName -ErrorAction Stop
}





https://github.com/juice-shop/juice-shop/releases/tag/v20.4.0?utm_source=chatgpt.com
