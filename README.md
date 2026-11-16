# Adresy IPv4 Windowsa
Get-NetIPAddress -AddressFamily IPv4 |
    Select-Object InterfaceAlias, IPAddress

# Czy aplikacja nasluchuje na porcie 3000
Get-NetTCPConnection -LocalPort 3000 -State Listen -ErrorAction SilentlyContinue |
    Select-Object LocalAddress, LocalPort, OwningProcess

# Czy sklep odpowiada wewnatrz Windowsa
curl.exe -I --max-time 10 http://127.0.0.1:3000/

# Stan zadania
Get-ScheduledTask -TaskName "LabJuiceShop" |
    Select-Object TaskName, State

# Ostatnie komunikaty aplikacji
Get-Content "C:\Lab\juice-shop\startup.log" -Tail 20
