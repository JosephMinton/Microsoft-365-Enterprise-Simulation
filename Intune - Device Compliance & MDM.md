# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Security Governance 🔒](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Intune - Device Compliance & MDM</h1>


<h2>Description</h2>
This section demonstrates end-to-end device lifecycle management using Microsoft Intune. Starting from tenant configuration 
in Entra ID, devices were enrolled through both manual Entra ID join and Windows Autopilot — a zero-touch provisioning method that 
automatically configures a device with corporate settings the moment a user signs in. Compliance and 
configuration policies were applied to enforce security baselines across enrolled devices.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Configure Entra ID device settings to enable MDM enrollment</li>
<li><strong>Enroll devices using both manual Entra ID join and Windows Autopilot</li>
<li><strong>Create dynamic device groups for automated policy targeting</li>
<li><strong>Extract hardware hashes and register devices for Autopilot provisioning</li>
<li><strong>Configure a compliance policy requiring BitLocker, Secure Boot, and minimum OS version</li>
<li><strong>Apply device restriction profiles to lock down Control Panel settings</li>
<li><strong>Validate enrollment and compliance status across the device inventory</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Intune (Endpoint Manager)</b></td>
    <td><b>Microsoft Entra ID</b></td>
  </tr>
  <tr>
    <td><b>Windows Autopilot</b></td>
    <td><b>Windows 11 (VMware)</b></td>
  </tr>
  <tr>
    <td><b>PowerShell</b></td>
    <td><b>Dynamic Device Groups</b></td>
  </tr>
  <tr>
    <td><b>Compliance Policies</b></td>
    <td><b>Device Restriction Profiles</b></td>
  </tr>
</table>

<h2>1. Intune Admin Center Overview</h2>
<p>The Microsoft Intune Admin Center serves as the central management hub for all enrolled devices, apps, and policies. The dashboard provides an at-a-glance view of the environment's health — including 
enrollment status, compliance state, configuration conflicts, and app installation results.</p>
<h3>At the time of documentation, the lab environment reflected:
</h3>

<ul>
  <li><strong>5 Windows devices enrolled</strong></li>
  <li>3 devices compliant, 2 not yet evaluated<strong></strong></li>
  <li><strong>No enrollment failures in the last 7 days</strong></li>
  <li><strong>No app installation failures</strong></li>
  <li><strong>No configuration policy conflicts</strong></li>
</ul>

<img src="" />



<h2>2. Tenant & Device Settings Configuration</h2>

<h3>Before any device can be enrolled, Entra ID must be configured to allow it. Device settings were accessed through the Entra Admin Center under Devices and configured as follows:</h3>

<ul>
  <li><strong>Users may join devices to Microsoft Entra</strong>: All</li>
  <li><strong>Users may register their devices with Microsoft Entra: All</strong></li>
  <li><strong>Require MFA to register or join devices</strong>: Yes</li>
  <li><strong>Maximum number of devices per user</strong>: 20 (reduced from the default of 50)</li>
</ul>

<i>Why reduce the device limit?</i>
<br />
<i>The default limit of 50 devices per user is far more than any individual needs and can mask unauthorized enrollments. 
Setting a realistic limit — such as 20 — makes it easier to detect anomalies and prevents a compromised account from registering a large number of rogue devices.</i>
<br />


<img src="" />

<img src="" />



<h2>3. Manual Device Enrollment — Entra ID Join</h2>
<p>A custom Teams setup policy was configured to standardize the application bar experience for users assigned to the policy. Pinned apps are automatically installed and surfaced in the Teams sidebar for all policy members, ensuring consistent access to essential tools without requiring individual setup.</p>
<h3>Applications pinned in the following order:</h3>
<ul>
  <li>Activity</li>
  <li>Chat</li>
  <li>Teams</li>
  <li>Calendar</li>
  <li>Calling</li>
  <li>OneDrive</li>
  <li>Calendly</li>
</ul>

<i>Why standardize pinned apps via policy?</i>
<br />
<i>When every user configures their own Teams sidebar, you end up with inconsistency across the org. Some people missing key tools, while others cluttered with apps they don't need. Pinning apps through an admin policy gives everyone the same starting point, reduces onboarding friction, and cuts down on "where do I find X" support tickets. Adding a third party app like Calendly also shows that Teams can be extended beyond Microsoft's own toolset to fit how a team desires to work.</i>
<br />

<img src="https://i.imgur.com/0j4pSDM.png"/>


<h2>4. Collaboration Security & SharePoint Governance</h2>
<p>A SharePoint Team Site was created for the IT Support team using a Help Desk template. The site was provisioned as a private group, meaning membership is controlled and the site is not discoverable by the broader part of the organization. This configuration is appropriate for teams handling sensitive operational data such as incident tickets, internal runbooks, or escalation procedures.</p>

<img src="https://i.imgur.com/qKL3FTT.png"/>

<h3>Site details:</h3>
<ul>
  <li>Site name: Help Desk</li>
  <li>Template: Team Site</li>
  <li>Type: Private group</li>
  <li>Description: IT Support Tech Team</li>
  <li>URL: <a href="https://josephmintontech.sharepoint.com/sites/HelpDesk/SitePages/ITHelpdeskHome.aspx?e=4%3AKBySh0&web=1&at=9">IT Support Tech Team</a></li>
</ul>
<h3>Team Site vs. Communication Site</h3>
<ul>
  <li><strong>Team Site</strong> Designed for a small, defined group of collaborators. All members can be owners with full control. Best for internal team workspaces with restricted access.</li>
  <li><strong>Communication Site</strong>: Designed for public audiences including thousands of visitors with a small number of owners maintaining full control. Best for company wide announcements or knowledge bases.</li>
</ul>
<p>The Help Desk site was created as a Team Site intentionally, limiting visibility and edit access to the IT support group only.
External sharing was tested by sharing the site with a personal Gmail account, confirming that the permission model correctly grants access to explicitly invited external users while keeping the site private from the general public.
</p>
<img src="https://i.imgur.com/ZjrTzIW.png"/>



<h2>5. Mailbox Retention & Data Lifecycle Management</h2>
<p>A retention policy was applied to a user mailbox via the Exchange Admin Center. The Default MRM Policy was selected. It is Microsoft's built in collection of retention tags applied to a mailbox at the folder level, governing how long items are retained before being moved to archive or permanently deleted.</p>
<h3>Policy applied:</h3>
<ul>
  <li><strong>Retention policy</strong>: Default MRM Policy</li>
  <li><strong>Sharing policy</strong>: Default Sharing Policy</li>
  <li><strong>Role assignment policy</strong>: Default Role Assignment Policy</li>
</ul>

<i>Why does retention matter?</i>
<br />
<i>Without a retention policy, user mailboxes grow indefinitely on the local client which could eventually cause performance degradation or machine crashes. Retention policies automatically move older emails to a cloud archive, keeping the local mailbox lean while preserving data for legal, compliance, or business continuity purposes. The archive is accessible to the user at any time through Outlook or the web client.</i>
<br />

<img src="https://i.imgur.com/rnl2Ndd.png" alt="Default Retention Policy"/>

<h2>6. Conditional Access</h2>
<p>Conditional Access policy configuration is covered in full in the dedicated Conditional Access subsection following Phase 3. That section documents policy design, conditions, access controls, and enforcement outcomes using the Microsoft Entra ID Conditional Access framework. Configuration details are covered in the dedicated subsection following Phase 3.</p>

<img src="https://i.imgur.com/hsH8d7b.png" alt="Conditional Access Dashboard"/>
<!-- [Conditional Access]() -->

<h2>7. Intune - Device Compliance & MDM</h2>
<p>Microsoft Intune configuration is covered in full in the dedicated Intune subsection following Phase 3. That section documents device enrollment, compliance policies, and integration with Conditional Access for device based access enforcement. Configuration details are covered in the dedicated subsection following Phase 3.</p>

<img src="https://i.imgur.com/zrVzG7V.png" alt="Intune Dashboard"/>
<!-- [Intune Device Compliance & MDM]() -->

<h2>8. Purview Data Loss Prevention (DLP)</h2>
<p>A Data Loss Prevention policy was configured in Microsoft Purview to detect sensitive financial data, specifically credit and debit card numbers across the tenant. The policy was built around the US Financial Data and PCI DSS compliance template and applied across Exchange email, SharePoint, OneDrive, and Teams, meaning any attempt to share card data in any of those locations gets flagged automatically. When a user tries to send a credit card number via email, a Policy Tip appears in real time warning and stopping the leak before it happens. Configuration details are covered in the dedicated subsection following Phase 3.</p>

<img src="https://i.imgur.com/A3uwt7g.png" alt="Purview Dashboard"/>
<!-- [Purview Data Loss Prevention (DLP)]() -->


<h1>Key Takeaways</h1>
<ul>
<li><strong>Validated the Okta SAML SSO integration, documenting the Tenant ID handshake, user assignment, and dual account federation trust</li>
<li><strong>Configured layered Teams governance across internal messaging policies and guest access settings, restricting message manipulation and screen sharing</li>
<li><strong>Standardized the Teams app bar experience via a custom setup policy including a third party app integration (Calendly)</li>
<li><strong>Provisioned a private SharePoint Team Site for the IT support group with controlled membership and validated external sharing behavior</li>
<li><strong>Applied the Default MRM Policy to a user mailbox, establishing automated data lifecycle management and cloud archiving</li>
<li><strong>Established Conditional Access, Intune, Purview frameworks as dedicated subsections</li>
</ul>
