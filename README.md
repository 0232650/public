foreach ($p in (Get-AppxPackage -AllUsers -Name 'Microsoft.Winget.Source')) {
    Remove-AppxPackage -Package $p.PackageFullName -AllUsers -ErrorAction Stop
}

foreach ($p in (Get-AppxProvisionedPackage -Online | Where-Object DisplayName -eq 'Microsoft.Winget.Source')) {
    Remove-AppxProvisionedPackage -Online -PackageName $p.PackageName -ErrorAction Stop
}
