#
# Script pour la preparation de la Vm
#
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.56.22 -PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 192.168.56.21 , 8.8.8.8

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

Rename-Computer -NewName AD2

Restart-Computer
