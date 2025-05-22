```bash
$dossier = "C:\Reseaux_entreprise\Direction"
$acl = Get-Acl $dossier


$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule("Dev","FullControl", "Deny")

$acl.AddAccessRule($accessRule)

Set-Acl $dossier $acl
```
