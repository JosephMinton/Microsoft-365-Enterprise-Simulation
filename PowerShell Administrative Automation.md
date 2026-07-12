# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Help Desk Scenarios 🛠️](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Helpdesk%20Scenarios.md)
<h1>Advanced Administrative Automation with PowerShell 📦</h1>


<h2>Description</h2>
The Microsoft 365 Admin Center is a great tool for routine tasks, but it was never designed for speed or scale. PowerShell with the Microsoft Graph SDK lets an administrator do in seconds what would take 
minutes through the portal — and more importantly, it lets you do it across hundreds of users at once. This section covers the full user lifecycle managed entirely through the command 
line: installing the tools, connecting securely, creating users one at a time and in bulk, managing licenses, enforcing security controls, and handling offboarding and recovery.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Install and verify the Microsoft Graph PowerShell SDK</li>
<li><strong>Connect to the tenant securely using appropriate permission scopes</li>
<li><strong>Provision a single user and assign a Business Premium license via CLI</li>
<li><strong>Bulk provision 15 users from a CSV file in a single scripted operation</li>
<li><strong>Block sign-in and enforce a password change for security operations</li>
<li><strong>Offboard a user by revoking license access and deleting the account</li>
<li><strong>Restore a deleted user from the tenant recycle bin</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>PowerShell (Windows)</b></td>
    <td><b>Microsoft Graph PowerShell SDK</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Graph API</b></td>
    <td><b>Microsoft 365 Admin Center</b></td>
  </tr>
</table>

<h2>1. Environment Setup — Installing Microsoft Graph</h2>
<p>Before any tenant management can happen through PowerShell, the Microsoft Graph PowerShell SDK needs to be installed. This is the official module that lets PowerShell 
communicate with Microsoft 365 through the Graph API. PowerShell was launched as Administrator to ensure the installation had the right permissions.</p>
</br>
<p>The execution policy was set first to allow the module to be downloaded and installed, then the module was installed from the PSGallery 
repository. When prompted about installing from an untrusted repository, the installation was confirmed by selecting Yes.</p>

COMMANDS USED

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Install-Module Microsoft.Graph -Scope CurrentUser
Get-InstalledModule Microsoft.Graph
```

<p>Installation was verified using 'Get-InstalledModule', which confirmed Microsoft Graph SDK version 2.37.0 was successfully installed from PSGallery.</p>


<img src="https://i.imgur.com/6YI4GAU.png"/>



<h2>2. Secure Connectivity — Connecting to Microsoft Graph</h2>

<p>Connecting to the tenant through PowerShell requires specifying permission scopes — these define exactly what the session is allowed to read or modify. Following the principle of least 
privilege, a read-only scope was used first to explore the tenant, then escalated to write permissions only when user provisioning required it.</p>

READ SCOPE: TENANT EXPLORATION

```powershell
Connect-MgGraph -Scopes "User.Read.All","Group.ReadWrite.All"
```

WRITE SCOPE: USER PROVISIONING AND MANAGEMENT

```powershell
Connect-MgGraph -Scopes "User.ReadWrite.All","Directory.AccessAsUser.All"
```

<img src="" PowerShell: Connect-MgGraph authentication output, "Welcome to Microsoft Graph" confirmed/>

<p>After connecting, the current state of the tenant was reviewed by querying all existing users. This confirmed who was already in the tenant before any new provisioning work began.</p>

VIEWING EXISTING USERS

```powershell
Get-MgUser -All | Select-Object DisplayName, Id, UserPrincipalName
```

<img src="" PowerShell: Get-MgUser output showing all existing users in the tenant/>




<h2>3. Single User Provisioning — Mikasa Ackerman</h2>
<p>A single user account was provisioned entirely through PowerShell to demonstrate the full creation workflow. Before creating the user, available license SKUs in the tenant were queried to identify the correct license ID for assignment.</p>
CHECK AVAILABLE LICENSES

```powershell
Get-MgSubscribedSku | Select -Property Sku*, ConsumedUnits -ExpandProperty PrepaidUnits | Format-List
```

<img src="" PowerShell: Get-MgSubscribedSku showing SPB license, 5 consumed of 25 available/>

<p>The user was then created with a display name, UPN, mail nickname, usage location, and a password profile. Usage location must be set before a license can be assigned — it is a required field in Microsoft 365 for compliance and data residency purposes.</p>

CREATE THE USER

```powershell
$PasswordProfile = New-Object -TypeName Microsoft.Graph.PowerShell.Models.MicrosoftGraphPasswordProfile
$PasswordProfile.Password = "<TempPassword>"

New-MgUser -DisplayName "Mikasa Ackerman" `
  -GivenName "Mikasa" -Surname "Ackerman" `
  -UserPrincipalName mikasaa@JosephMintonTech.onmicrosoft.com `
  -UsageLocation "US" -MailNickname "mikasaa" `
  -PasswordProfile $PasswordProfile `
  -AccountEnabled:$true
```

<img src="" PowerShell: New-MgUser output confirming Mikasa Ackerman created with DisplayName, Id, and UPN/>

<p>The SPB (Microsoft 365 Business Premium) license was then assigned to Mikasa and confirmed via a follow-up query.</p>

ASSIGN LICENSE
 ```powershell
$e5Sku = Get-MgSubscribedSku -All | Where SkuPartNumber -eq 'SPB'
Set-MgUserLicense -UserId "mikasaa@JosephMintonTech.onmicrosoft.com" `
  -AddLicenses @{SkuId = $e5Sku.SkuId} -RemoveLicenses @()

Get-MgUserLicenseDetail -UserId "mikasaa@JosephMintonTech.onmicrosoft.com" | Select-Object SkuPartNumber
```

<img src=""PowerShell: Set-MgUserLicense confirming SPB license assigned, Get-MgUserLicenseDetail returning SPB/>

<p>As a bonus step, the usage location was updated across all users in the tenant from US to France and back to US — demonstrating how a bulk attribute change can be applied to every user in a single command rather than updating each account individually.</p>

BULK LOCATION UPDATE
```powershell
Get-MgUser | ForEach-Object { Update-MgUser -UserId $_.Id -UsageLocation "FR" }
Get-MgUser | ForEach-Object { Update-MgUser -UserId $_.Id -UsageLocation "US" }
```

<img src=""PowerShell: UsageLocation bulk update commands cycling all users from US to FR and back to US/>
<p>Mikasa's account was then verified in the M365 Admin Center, confirming her profile showed the correct license and United States location.</p>
<img src=""M365 Admin Center: Mikasa Ackerman profile showing Business Premium license assigned, United States location/>


<h2>4. Bulk User Provisioning — 15 Users via CSV</h2>
<p>To simulate a real onboarding event, 15 user accounts were provisioned in a single scripted operation using a CSV file. Each row in the CSV contained a complete user record — UPN, first name, last name, 
display name, usage location, and mail nickname. The PowerShell loop iterated through every record, created the account, and assigned the SPB license automatically.</p>

<img src=""Notepad: NewAccounts.csv showing 15 Attack on Titan user records with full attribute structure/>


```powershell
$users = Import-Csv -Path "D:\temp\NewAccounts.csv"
$PasswordProfile = @{
    Password = 'TempPass@2026!'
    ForceChangePasswordNextSignIn = $true
}

foreach ($user in $users) {
    $newUser = New-MgUser -DisplayName $user.DisplayName `
        -GivenName $user.FirstName -Surname $user.LastName `
        -UserPrincipalName $user.UserPrincipalName `
        -UsageLocation $user.UsageLocation `
        -MailNickname $user.MailNickname `
        -PasswordProfile $PasswordProfile `
        -AccountEnabled:$true

    $sku = Get-MgSubscribedSku | Where-Object { $_.SkuPartNumber -eq "SPB" }
    Set-MgUserLicense -UserId $newUser.Id `
        -AddLicenses @{SkuId = $sku.SkuId} -RemoveLicenses @()
}
```

<img src="" PowerShell: bulk provisioning loop executing, all 15 users created with output showing DisplayName, Id, UPN/>

<p>All 15 accounts were verified in the M365 Admin Center, filtered by Business Premium license and Sign-in status Allowed, confirming every user was created and licensed correctly.</p>

<img src="" M365 Admin Center: Active users filtered by Business Premium and Sign-in Allowed, all 15 users visible/>

<i>Why CSV bulk provisioning matters</i>
<br />
<i>In a real onboarding scenario involving dozens or hundreds of new employees, manually creating each account through the portal is not practical. A CSV-driven PowerShell script reduces a multi-hour 
manual task to a few minutes, eliminates data entry errors, and produces a consistent result every time. The same script can be reused for future onboarding events with a new CSV file.</i>
<br />


<h2>5. Security Operations — Block Sign-in & Password Management</h2>
<h3>Two critical security operations were performed via PowerShell without using the admin portal — blocking a user's sign-in access and forcing a password change on next login.</h3>
<h3><strong>Block Sign-in</strong></h3>
<p>Mikasa Ackerman's account was disabled to immediately revoke her access to the tenant. This is the first action in any offboarding or security response scenario — it cuts off access instantly while the rest of the offboarding process continues.</p>

DISABLE ACCOUNT
```powershell
Update-MgUser -UserId "mikasaa@JosephMintonTech.onmicrosoft.com" -AccountEnabled:$false

Get-MgUser -UserId "mikasaa@JosephMintonTech.onmicrosoft.com" `
  -Property "DisplayName,AccountEnabled" | Select-Object DisplayName, AccountEnabled
```
<img src=""PowerShell: Update-MgUser executed, Get-MgUser confirming AccountEnabled returns False for Mikasa/>
<p>The block was confirmed in the M365 Admin Center, where Mikasa's account appeared under the Sign-in status Blocked filter.</p>
<img src=""M365 Admin Center: Active users filtered by Sign-in status Blocked, Mikasa Ackerman visible with Business Premium license/>

<h3><strong>Force Password Change on Next Login</strong></h3>
<p>A temporary password was set on Mikasa's account with the ForceChangePasswordNextSignIn flag set to true. This ensures that even if the temporary password is shared or intercepted, the user must replace it immediately on first login.</p>

SET TEMPORARY PASSWORD
```powershell
Update-MgUser -UserId "mikasaa@JosephMintonTech.onmicrosoft.com" `
  -PasswordProfile @{
      Password = "<TempPassword>"
      ForceChangePasswordNextSignIn = $true
  }
```
<img src=""PowerShell: Update-MgUser password profile set, ForceChangePasswordNextSignIn confirmed as true for Mikasa/>



<h2>6. Account Offboarding — John Doe</h2>
<p>A complete offboarding workflow was executed for John Doe entirely through PowerShell. The process followed the correct 
order of operations — verify the license, remove it, delete the account, then confirm the deletion.</p>

<h3><strong>License Verification and Removal</strong></h3>

<p>Before removing the license, a query confirmed which license was assigned to John's account. The SPB license was then removed, releasing that 
seat back into the available pool for reassignment. A follow-up query confirmed the removal was successful by returning an empty result.</p>

PRE-REMOVAL VERIFICATION
```powershell
Get-MgUserLicenseDetail -UserId "johnd@JosephMintonTech.onmicrosoft.com"
```

<img src="" PowerShell: Get-MgUserLicenseDetail showing SPB license present on John Doe pre-removal/>

REMOVE LICENSE
```powershell
Set-MgUserLicense -UserId "johnd@JosephMintonTech.onmicrosoft.com" `
  -AddLicenses @() `
  -RemoveLicenses @("cbdc14ab-d96c-4c30-b9f4-6ada7cdc1d46")

Get-MgUserLicenseDetail -UserId "johnd@JosephMintonTech.onmicrosoft.com"
```

<img src="" PowerShell: Get-MgUserLicenseDetail returning empty after removal — license successfully released/>

<h3><strong>Account Deletion and Verification</strong></h3>
<p>John's account was then deleted and confirmed in both the PowerShell recycle bin query and the M365 Admin Center deleted users list.</p>

DELETE ACCOUNT
```powershell
Remove-MgUser -UserId "johnd@JosephMintonTech.onmicrosoft.com"

Get-MgDirectoryDeletedItemAsUser -All
```

<img src="" PowerShell: Get-MgDirectoryDeletedItemAsUser showing John Doe in the recycle bin alongside other deleted users/>
<img src="" M365 Admin Center: Deleted users list with John Doe highlighted/>



<h2>7. Restoring a Deleted User — John Doe</h2>
<p>Deleted users in Microsoft 365 are held in a recycle bin for 30 days before being permanently removed. This window allows administrators to recover accounts deleted by mistake. 
John Doe's object ID was retrieved from the recycle bin and used to restore the account.</p>

RESTORE FROM RECYCLE BIN
```powershell
Get-MgDirectoryDeletedItemAsUser -All | Select-Object DisplayName, Id

Restore-MgDirectoryDeletedItem -DirectoryObjectId "994c1b7b-3bde-4693-a96f-beaf2155574c"

Get-MgUser -UserId "johnd@JosephMintonTech.onmicrosoft.com" `
  -Property "DisplayName,AccountEnabled" | Select-Object DisplayName, AccountEnabled
```
<img src=""PowerShell: recycle bin query, Restore-MgDirectoryDeletedItem executed, verification showing John Doe restored with AccountEnabled False/>

<i>Why does AccountEnabled return False after restore?</i>
<br />
<i>When an account is restored from the recycle bin, it comes back exactly as it was when it was deleted — including the disabled state from offboarding. This is intentional and expected. AccountEnabled: 
False simply confirms the restore was successful. The account exists again in the tenant but sign-in remains blocked until an administrator explicitly reactivates it, which prevents an accidentally restored account from immediately gaining access.</i>
<br />


<h1>Key Takeaways</h1>
<ul>
<li><strong>Installed and verified the Microsoft Graph PowerShell SDK and connected to the tenant using scoped permissions following least privilege principles</li>
<li><strong>Provisioned a single user with a license and configured usage location entirely through the command line without using the admin portal</li>
<li><strong>Demonstrated bulk attribute management by updating usage location across all tenant users in a single pipeline command</li>
<li><strong>Bulk provisioned 15 users from a structured CSV file using a PowerShell loop that created each account and assigned a license automatically</li>
<li><strong>Blocked sign-in access and enforced a forced password change via PowerShell as security response operations</li>
<li><strong>Executed a complete offboarding workflow — license removal, account deletion, and recycle bin verification — entirely from the CLI</li>
<li><strong>Restored a deleted user from the 30-day recycle bin using the object ID retrieved from a directory query</li>
</ul>
