#
# Script pour la preparation de la Vm
#
```bash 
$domain = (Get-WmiObject -Class Win32_ComputerSystem).Domain
if ($domain -ne "WORKGROUP") {
    Write-Host "L'ordinateur est déjà membre d'un domaine. L'ajout d'AD sera arrêté."
    exit
} 

Set-ExecutionPolicy Unrestricted -Scope CurrentUser

New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.1.24 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 192.168.1.1

Rename-Computer -NewName client_1

Add-Computer -Domain eval.server

Restart-Computer

```
