#
# Script Windows PowerShell pour le déploiement d’AD DS
#

$domain = (Get-WmiObject -Class Win32_ComputerSystem).Domain
if ($domain -ne "WORKGROUP") {
    Write-Host "L'ordinateur est déjà membre d'un domaine. L'ajout d'AD sera arrêté."
    exit
}  
Import-Module ADDSDeployment
Install-ADDSForest `
-CreateDnsDelegation:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainMode "WinThreshold" `
-DomainName "charlyne.local" `
-DomainNetbiosName "CHARLYNE" `
-ForestMode "WinThreshold" `
-InstallDns:$true `
-LogPath "C:\Windows\NTDS" `
-NoRebootOnCompletion:$false `
-SysvolPath "C:\Windows\SYSVOL" `
-Force:$true
