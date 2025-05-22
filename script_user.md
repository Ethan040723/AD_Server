````bash 

if (-not (Get-ADOrganizationalUnit -Filter {Name -eq "Personnel"})) {
    New-ADOrganizationalUnit -Name "Personnel" -Path "DC=charlyne,DC=local"
}


$GroupNames = @("Dev", "Direction", "IT", "Comptabilité")
foreach ($GroupName in $GroupNames) {
    if (-not (Get-ADGroup -Filter {Name -eq $GroupName})) {
        New-ADGroup -Name $GroupName -Path "OU=Personnel,DC=charlyne,DC=local" -GroupScope Global
    }
}

function Create-UserIfNotExists {
    param (
        [string]$SamAccountName,
        [string]$UserPrincipalName,
        [string]$Name,
        [string]$GivenName,
        [string]$Surname,
        [string]$DisplayName,
        [string]$Password
    )

    if (-not (Get-ADUser -Filter {SamAccountName -eq $SamAccountName})) {
        New-ADUser -SamAccountName $SamAccountName `
                   -UserPrincipalName $UserPrincipalName `
                   -Name $Name `
                   -GivenName $GivenName `
                   -Surname $Surname `
                   -DisplayName $DisplayName `
                   -Path "OU=Personnel,DC=charlyne,DC=local" `
                   -AccountPassword (ConvertTo-SecureString $Password -AsPlainText -Force) `
                   -Enabled $true `
                   -Server "charlyne.local"
    }
}

$Users = @(
    @{ SamAccountName = "baptiste.D"; UserPrincipalName = "baptistedupuis@charlyne.local"; Name = "Baptiste DUPUIS"; GivenName = "Baptiste"; Surname = "DUPUIS"; DisplayName = "Baptiste DUPUIS"; Password = "Azerty14/04"; Group = "Dev" },
    @{ SamAccountName = "Johann.G"; UserPrincipalName = "johannguinberteau@charlyne.local"; Name = "Johann GUINBERTEAU"; GivenName = "Johann"; Surname = "GUINBRTEAU"; DisplayName = "Johann GUINBERTEAU"; Password = "Azerty14/04"; Group = "Dev" },
    @{ SamAccountName = "charlyne.G"; UserPrincipalName = "charlynegraincourtmerieau@charlyne.local"; Name = "Charlyne GRAINCOURT-MERIEAU"; GivenName = "Charlyne"; Surname = "GRAINCOURT-MERIEAU"; DisplayName = "Charlyne GRAINCOURT-MERIEAU"; Password = "Azerty14/04"; Group = "Direction" },
    @{ SamAccountName = "ethan.Q"; UserPrincipalName = "ethanquehelecozler@charlyne.local"; Name = "Ethan QUEHE-LE COZLER"; GivenName = "Ethan"; Surname = "QUEHE-LE COZLER"; DisplayName = "Ethan QUEHE-LE COZLER"; Password = "Azerty14/04"; Group = "Direction" },
    @{ SamAccountName = "meven.D"; UserPrincipalName = "mevendesbois@charlyne.local"; Name = "Meven DESBOIS"; GivenName = "Meven"; Surname = "DESBOIS"; DisplayName = "Meven DESBOIS"; Password = "Azerty14/04"; Group = "IT" },
    @{ SamAccountName = "landry.C"; UserPrincipalName = "landrycotillon@charlyne.local"; Name = "Landry COTILLON"; GivenName = "Landry"; Surname = "COTILLON"; DisplayName = "Landry COTILLON"; Password = "Azerty14/04"; Group = "IT" },
    @{ SamAccountName = "lilian.B"; UserPrincipalName = "lilianbrosset@charlyne.local"; Name = "Lilian BROSSET"; GivenName = "Lilian"; Surname = "BROSSET"; DisplayName = "Lilian BROSSET"; Password = "Azerty14/04"; Group = "Comptabilité" },
    @{ SamAccountName = "théotim.A"; UserPrincipalName = "théotimalberteau@charlyne.local"; Name = "Théotim ALBERTAU"; GivenName = "Théotim"; Surname = "ALBERTAU"; DisplayName = "Théotim ALBERTAU"; Password = "Azerty14/04"; Group = "Comptabilité" }
)

foreach ($user in $Users) {
    Create-UserIfNotExists -SamAccountName $user.SamAccountName `
                           -UserPrincipalName $user.UserPrincipalName `
                           -Name $user.Name `
                           -GivenName $user.GivenName `
                           -Surname $user.Surname `
                           -DisplayName $user.DisplayName `
                           -Password $user.Password

    if (-not (Get-ADGroupMember -Identity $user.Group | Where-Object { $_.SamAccountName -eq $user.SamAccountName })) {
        Add-ADGroupMember -Identity $user.Group -Members $user.SamAccountName
    }
}

```