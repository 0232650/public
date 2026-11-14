foreach ($p in (Get-AppxPackage -AllUsers -Name 'Microsoft.Winget.Source')) {
    Remove-AppxPackage -Package $p.PackageFullName -AllUsers -ErrorAction Stop
}

foreach ($p in (Get-AppxProvisionedPackage -Online | Where-Object DisplayName -eq 'Microsoft.Winget.Source')) {
    Remove-AppxProvisionedPackage -Online -PackageName $p.PackageName -ErrorAction Stop
}





https://github.com/juice-shop/juice-shop/releases/tag/v20.4.0?utm_source=chatgpt.com
