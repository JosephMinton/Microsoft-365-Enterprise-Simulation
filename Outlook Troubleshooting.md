# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Help Desk Scenarios 🛠️](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Helpdesk%20Scenarios.md)
<h1>Outlook Troubleshooting 📧</h1>

<h2>Description</h2>
Outlook is one of the most used applications in any Microsoft 365 environment, and it is also one of the most frequently reported 
sources of IT support tickets. This section documents the troubleshooting process for the most common issues — starting with the least 
invasive fix and escalating only when needed. The goal is always to resolve the issue without deleting the user's profile or making them 
wait hours for their emails to re-download.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Identify and resolve add-in conflicts causing Outlook crashes</li>
<li><strong>Rebuild the Windows Search index to fix broken search functionality</li>
<li><strong>Restart the Windows Search service as an alternative search fix</li>
<li><strong>Run a Microsoft 365 Online Repair to resolve deeper application issues</li>
<li><strong>Clear saved credentials in Windows Credential Manager to resolve repeated password prompts</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Outlook (Desktop)</b></td>
    <td><b>COM Add-ins Manager</b></td>
  </tr>
  <tr>
    <td><b>Windows Control Panel</b></td>
    <td><b>Indexing Options</b></td>
  </tr>
  <tr>
    <td><b>Windows Services</b></td>
    <td><b>Control Panel</b></td>
  </tr>
  <tr>
    <td><b>Windows Services</b></td>
    <td><b>Windows Credential Manager</b></td>
  </tr>
</table>

<h2>Issue 1</h2>
<h2>Outlook Crashes on Startup — Add-in Conflicts</h2>
SYMPTOMS
<table>
  <tr>
    <td><b><strong>Cause</strong></b></td>
    <td><b>A COM add-in conflict or corrupted plugin preventing Outlook from loading normally</b></td>
  </tr>
  <tr>
    <td><b><strong>Common culprits</strong></b></td>
    <td><b>Zoom, Teams, Bloomberg, or any third party add-in installed alongside Outlook</b></td>
  </tr>
</table>
<p>When Outlook crashes on startup, the first step is to open it in Safe Mode by running outlook.exe /safe from the Windows Run dialog. Safe Mode loads Outlook without any add-ins. If 
Outlook opens normally in Safe Mode but crashes otherwise, the problem is an add-in — not the profile or the application itself.</p>
</br>
<p>From there, navigate to File, then Options, then Add-ins, and click Go next to COM Add-ins. Review the list of installed add-ins and look for any that are unchecked — sometimes 
Outlook automatically disables a problematic add-in after a crash and that unchecked box is the giveaway. Re-enabling it or disabling the conflicting one usually resolves the issue.</p>

<img src="https://i.imgur.com/znLOzip.png"/>

> *__Escalation path if re-enabling does not work__*
>
> *If adjusting the add-in does not fix the issue, the next step is to go to Control Panel, open Uninstall a Program, find the affected application such as Zoom, and choose Repair rather than Uninstall. Repair fixes the application files without removing the software entirely. If the
> problem persists after that, creating a new Outlook profile is the last resort — but this means the user's emails will need to re-download, which can take a long time on large mailboxes. As a workaround while that process runs, point the user to Outlook on the web so they can keep working.*


<h2>Issue 2</h2>
<h2>Search Not Working — Rebuild the Search Index</h2>
SYMPTOMS
<table>
  <tr>
    <td><b><strong>Cause</strong></b></td>
    <td><b>The Windows Search index has become outdated or corrupted, so Outlook cannot find emails even though they exist</b></td>
  </tr>
  <tr>
    <td><b><strong>Signs</strong></b></td>
    <td><b>Searching for an email returns no results or incomplete results despite the message being visible in the folder</b></td>
  </tr>
</table>

<p>The fix is to rebuild the search index from scratch. Open Control Panel and navigate to Indexing Options. Click Advanced, then under the Troubleshooting section click Rebuild. 
Windows will delete the existing index and build a new one — this process runs in the background and can take a while depending on how many items are indexed.</p>

<img src="https://i.imgur.com/yQ67lPu.png"/>

<h3><strong>Alternative — Restart the Windows Search Service</strong></h3>
<p>A quicker option that sometimes resolves search issues without a full rebuild is to restart the Windows Search service. Open the Services application on Windows, scroll down to 
Windows Search, and click Stop followed by Start. This refreshes the search service without wiping and rebuilding the entire index.</p>

<img src="https://i.imgur.com/Ai4usyM.png"/>

<h2>Issue 3</h2>
<h2>Persistent Issues — Microsoft 365 Online Repair</h2>
SYMPTOMS
<table>
  <tr>
    <td><b><strong>Cause</strong></b></td>
    <td><b>Corrupted Microsoft 365 installation files causing slow performance, frequent crashes, or features not working as expected</b></td>
  </tr>
  <tr>
    <td><b><strong>When to use this</strong></b></td>
    <td><b>After simpler fixes like add-in adjustments and profile repairs have not resolved the issue</b></td>
  </tr>
</table>
<p>Online Repair is the most thorough repair option available for Microsoft 365. It downloads a fresh copy of the Office installation files from Microsoft's servers and replaces any corrupted components. To run it, open Control Panel and go to Uninstall a Program. Find Microsoft 365, click Change, and select Online Repair from the options presented.</p>
</br>
<p>Online Repair takes longer than Quick Repair because it requires an internet connection throughout, but it has a significantly higher success rate for deep application issues. Quick Repair is faster but only fixes problems it can resolve without downloading files — if that does not work, Online Repair is the correct next step.</p>

<img src="https://i.imgur.com/90poPb5.png"/>


<h2>Issue 4</h2>
<h2>Password Prompt Keeps Appearing — Credential Manager</h2>
SYMPTOMS
<table>
  <tr>
    <td><b><strong>Cause</strong></b></td>
    <td><b>Outlook is holding on to an old or expired password stored in Windows Credential Manager. Every time it tries to connect to the server using the wrong password, it fails and prompts again</b></td>
  </tr>
  <tr>
    <td><b><strong>Signs</strong></b></td>
    <td><b>A login popup appears repeatedly even after entering the correct password</b></td>
  </tr>
</table>
<p>The fix is to remove the saved credential so Windows stops trying to use the old one. Open Control Panel and navigate to User Accounts, then Credential Manager. Switch to Windows Credentials and look for any entry related to Microsoft 365 or the user's email address. Click Remove to delete the stored credential. The next time Outlook connects, it will prompt the user for their current password and save it fresh.</p>
</br>

> *__Also worth checking__*
>
> *If the password prompt keeps returning after clearing credentials, the user's Active Directory account may be locked out or their password may have expired at the directory level. In 
> that case the fix is on the admin side — unlocking the account or resetting the password in the Microsoft 365 Admin Center or via PowerShell rather than on the local machine.*

<img src="https://i.imgur.com/urNM6fE.png"/>



<h1>Key Takeaways</h1>
<ul>
<li><strong>Used Safe Mode to isolate an add-in conflict and resolved it through the COM Add-ins manager without touching the user's profile</li>
<li><strong>Rebuilt the Windows Search index via Control Panel and restarted the Windows Search service as a lighter alternative fix</li>
<li><strong>Ran a Microsoft 365 Online Repair to address deep application issues that simpler fixes could not resolve</li>
<li><strong>Cleared stored credentials in Windows Credential Manager to stop repeated password prompts caused by an expired saved password</li>
<li><strong>Documented additional common issues including offline mode, PST corruption, attachment problems, calendar sync, and slow performance as a practical support reference</li>
</ul>
