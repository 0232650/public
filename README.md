$pakiet = 'Microsoft.Winget.Source'

Get-AppxPackage -AllUsers -Name $pakiet |
    ForEach-Object {
        Remove-AppxPackage -Package $_.PackageFullName -AllUsers -ErrorAction Stop
    }

Get-AppxProvisionedPackage -Online |
    Where-Object { $_.DisplayName -eq $pakiet } |
    ForEach-Object {
        Remove-AppxProvisionedPackage -Online -PackageName $_.PackageName -ErrorAction Stop
    }
