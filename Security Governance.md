# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Security & Governance</h1>



<h2>Description</h2>
Phase 3 focuses on hardening the M365 environment built in Phases 1 and 2. Security controls were applied across identity,
messaging, collaboration, and data layers — reflecting the defense-in-depth approach used in enterprise IT and security operations. 
This phase also serves as the foundation for the Conditional Access and Intune subsections that follow.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Validate and document the Okta SSO integration established in Phase 1</li>
<li><strong>Configure Teams messaging and guest access policies to restrict unauthorized communication behaviors</li>
<li><strong>Customize Teams app setup policies to standardize the user experience</li>
<li><strong>Configure SharePoint site permissions and external sharing controls</li>
<li><strong>Apply a mailbox retention policy to manage data lifecycle and storage</li>
<li><strong>Establish the Conditional Access and Intune frameworks covered in dedicated subsections</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Entra ID</b></td>
    <td><b>Okta (SAML 2.0 / SSO)</b></td>
  </tr>
  <tr>
    <td><b>Microsoft 365 Admin Center</b></td>
    <td><b>SharePoint Admin Center</b></td>
  </tr>
  <tr>
    <td><b>Exchange Online</b></td>
    <td><b>Microsoft Purview (DLP)</b></td>
  </tr>
  <tr>
    <td><b>Conditional Access (Entra ID)</b></td>
    <td><b>Microsoft Intune</b></td>
  </tr>
</table>

<h2>1. Advanced Identity & SSO Integration</h2>
<p>The Okta SAML 2.0 integration was initially configured and documented in Phase 1, Section 1.6. The screenshots below provide supplementary 
evidence of the integration in an operational state — specifically the Tenant ID retrieval from Azure, user assignment within Okta, and the resulting account 
status across both identity systems.
To complete the Okta integration, the tenant's unique identifier was retrieved from the Azure portal and supplied to Okta during the SAML configuration process. 
This handshake establishes the trust relationship between Okta as the Identity Provider and Microsoft 365 as the Service Provider.
</h3>
<img src="" />
<p>Following configuration, the Office 365 application within Okta was assigned directly to the lab admin account (Joseph Minton / JosephMinton@JosephMintonTech.onmicrosoft.com). 
Individual assignment reflects a deliberate access control decision — in production, this would typically be assigned to a group rather than an individual to support scalable provisioning.</p>
<img src="" />
<p>The Okta user list reflects two account states for the same identity — the Gmail address showing "Pending user action" and the Microsoft tenant account showing "Active." 
This split view illustrates the federation in progress: the external identity (Gmail) has been linked to the Okta directory but requires the user to complete setup, 
while the M365 account is fully provisioned and operational.
</p>

<img src="https://i.imgur.com/FFk2Pyt.png" />




<h2>2. Teams Governance — Messaging & Guest Access Policies</h2>
<p>Microsoft Teams messaging and guest access policies were configured to enforce communication boundaries appropriate for a corporate environment. Two distinct policy 
layers were addressed: internal messaging policies applied to org members, and guest access settings controlling what external participants can do when invited into the tenant.
</p>

<h3>Internal Messaging Policy</h3>
<h4>A custom messaging policy was configured with the following restrictions applied to internal users:</h4>

<ul>
  <li><strong>Delete sent messages</strong>- Off</li>
  <li><strong>Edit sent messages</strong>- Off</li>
  <li><strong>Delete messages sent by bots</strong>- Off</li>
  <li><strong>Chat</strong>- Off</li>
  <li><strong>Giphy, Memes, Stickers</strong>- Off</li>
  <li><strong>URL previews</strong>- Off</li>
</ul>

<i>Why restrict message deletion and editing?</i>
<br />
<i>Allowing users to delete or edit sent messages undermines the integrity of communication records. In regulated industries, chat logs may be subject to 
compliance holds or eDiscovery requests — permitting deletion creates gaps in the audit trail. Disabling these controls preserves message immutability across the tenant.</i>

<img src="https://i.imgur.com/es4FtsB.png" />

<h3>Guest Access — Messaging Settings</h3>
<h4>Guest access was left enabled to support B2B collaboration scenarios, but heavily restricted at the feature level. 
All message manipulation capabilities were disabled for guest users:</h4>

<ul>
  <li><strong>Edit sent messages</strong>- Off</li>
  <li><strong>Delete sent messages</strong>- Off</li>
  <li><strong>Delete chat</strong>- Off</li>
  <li><strong>Chat</strong>- Off</li>
  <li><strong>Giphy, Memes, Stickers</strong>- Off</li>
</ul>

<img src="https://i.imgur.com/es4FtsB.png" />


<h3>Distribution Group vs. Security Group</h3>
<ul>
  <li><strong>Distribution Group</strong>: Email delivery only. Cannot be used to assign permissions to resources. Best used for team or department mailing lists.</li>
  <li><strong>Security Group</strong>: Used to grant or restrict access to M365 resources. Does not inherently have email capability unless mail enabled. 
Best used for role based access control.</li>
</ul>
<p>The allow external senders setting was enabled on the Distribution Group to permit inbound mail from outside the organization.
</p>

<img src="https://i.imgur.com/es4FtsB.png" />

<h2>3. Teams App Setup Policy — Pinned Applications</h2>
<p>A custom Teams setup policy was configured to standardize the application bar experience for users assigned to the policy. Pinned apps are automatically installed and surfaced in the Teams sidebar for all policy members, ensuring consistent access to essential tools without requiring individual configuration.</p>
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
<i>Shared Mailboxes are free up to 50GB and do not require a dedicated license. They are a standard tool in enterprise environments for managing high volume or shared communication channels.</i>
<br />

<img src="https://i.imgur.com/Hn05fz3.png"/>


<h2>4. Collaboration Security & SharePoint Governance</h2>
<p>A SharePoint Team Site was created for the IT Support team using the Help Desk template. The site was provisioned as a private group — meaning membership is controlled and the site is not discoverable by the broader organization. This configuration is appropriate for teams handling sensitive operational data such as incident tickets, internal runbooks, or escalation procedures.</p>

<h3>Site details:</h3>
<ul>
  <li>Site name: Help Desk</li>
  <li>Type: Private group</li>
  <li>Description: IT Support Tech Team</li>
</ul>
<h3>Team Site vs. Communication Site</h3>
<ul>
  <li><strong>Team Site</strong> Designed for a small, defined group of collaborators. All members can be owners with full control. Best for internal team workspaces with restricted access.</li>
  <li><strong>Communication Site</strong>: Designed for broad audiences — potentially thousands of visitors — with a small number of owners maintaining full control. Best for company-wide announcements or knowledge bases.</li>
</ul>
<p>The Help Desk site was created as a Team Site intentionally, limiting visibility and edit access to the IT support group only.
External sharing was tested by sharing the site with a personal Gmail account, confirming that the permission model correctly grants access to explicitly invited external users while keeping the site private from the general public.
</p>
<img src="https://i.imgur.com/EyOyxVI.png" alt="HR contact Mail Tip: Please allow up to two business days for a response (Gwen Stacy)"/>



<h2>5. Mailbox Retention & Data Lifecycle Management</h2>
<h3>Room and Equipment mailboxes were created to enable calendar based scheduling of physical office assets through Exchange Online.</h3>
<ul>
  <li><strong>Room Mailbox</strong> - configured for an 11th floor conference room, including capacity and location metadata</li>
  <li><strong>Equipment Mailbox</strong> - configured for a loaner laptop asset available for reservation</li>
</ul>
<p>Both resources appear in the organization's address book and can be invited to calendar events, 
allowing users to reserve physical assets directly from Outlook without a separate booking system.</p>

<img src="https://i.imgur.com/VUzN3l9.png" alt="Capacity and Location details visible"/>
<img src="https://i.imgur.com/DeIcQhC.png" alt="Room and Equipment mailboxes"/>

<h2>6. User Offboarding & License Reclamation</h2>
<p>A secure offboarding workflow was executed for a test user (Mei-Lin Torres), simulating the standard process for a departing employee. The account was deleted through the Admin Center, which automatically unassigns the user's Business Premium license and returns it to the available pool.</p>

<h3>Offboarding steps performed:</h3>
<ul>
  <li>User account deletion initiated using Admin Center</li>
  <li>Business Premium license automatically unassigned and returned to the available pool</li>
  <li>Email and OneDrive data confirmed as retained for 30 days before permanent deletion</li>
</ul>

<i>Why does the 30 day window matter?</i>
<br />
<i>The 30 day retention period is a critical safeguard. It provides a recovery window in cases of accidental deletion, active legal holds, or post departure 
data retrieval needs. Data permanently deleted outside this window is unrecoverable without a third party backup solution, 
making awareness of this window a required component of any enterprise offboarding operational procedures.</i>

<img src="https://i.imgur.com/y3xG0eb.png" alt="Delete user confirmation and 30 day retention message of Mei-Lin Torres"/>

<h1>Key Takeaways</h1>
<ul>
<li><strong>Applied least privilege at the application level by selectively disabling unused apps within an assigned license</li>
<li><strong>Configured Distribution and Security groups with a clear understanding of their distinct functional roles</li>
<li><strong>Deployed a Shared Mailbox as a cost effective, license free solution for centralized support communication</li>
<li><strong>Integrated external contacts into the global address list with mail tips to reduce misdirected communication</li>
<li><strong>Executed a complete offboarding workflow including license reclamation and 30 day data retention awareness</li>
</ul>
