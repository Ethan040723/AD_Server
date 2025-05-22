#
# Script Windows PowerShell pour le déploiement d’AD DS
#
```bash
$domain = (Get-WmiObject -Class Win32_ComputerSystem).Domain
if ($domain -ne "WORKGROUP") {
    Write-Host "L'ordinateur est déjà membre d'un domaine. L'ajout d'AD sera arrêté."
    exit
}  
Import-Module ADDSDeployment
Install-ADDSDomainController `
-NoGlobalCatalog:$false `
-CreateDnsDelegation:$false `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainName "charlyne.local" `
-InstallDns:$true `
-LogPath "C:\Windows\NTDS" `
-NoRebootOnCompletion:$false `
-SiteName "Default-First-Site-Name" `
-SysvolPath "C:\Windows\SYSVOL" `
-Force:$true
```