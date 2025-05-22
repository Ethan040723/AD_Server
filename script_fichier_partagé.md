Install-WindowsFeature FS-FileServer


if (-not (Test-Path "C:\Reseaux_entreprise")) {
    New-Item -Path "C:\Reseaux_entreprise" -ItemType Directory
}
else{
	Write-host "le dossier existe déjà"
}

$shareName = "Reseaux_entreprise"
if (-not (Get-SmbShare -Name $shareName -ErrorAction SilentlyContinue)) {
    New-SmbShare -Name $shareName -Path "C:\Reseaux_entreprise" -FullAccess "Direction", "Dev", "IT", "Comptabilité", "Administrateur"
}
else{
    Write-Host "le dossier partager"
}