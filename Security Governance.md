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
<img src="https://i.imgur.com/SUe9Cff.png" />
<p>Following configuration, the Office 365 application within Okta was assigned directly to the lab admin account (Joseph Minton / JosephMinton@JosephMintonTech.onmicrosoft.com). 
Individual assignment reflects a deliberate access control decision — in production, this would typically be assigned to a group rather than an individual to support scalable provisioning.</p>

<p>The Okta user list reflects two account states for the same identity — the Gmail address showing "Pending user action" and the Microsoft tenant account showing "Active." 
This split view illustrates the federation in progress: the external identity (Gmail) has been linked to the Okta directory but requires the user to complete setup, 
while the M365 account is fully provisioned and operational.
</p>

<img src="https://i.imgur.com/pR6uZ8n.png" />




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

<img src="https://i.imgur.com/9PUMo8A.png" />

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

<img src="https://i.imgur.com/eOSIQWf.png" />


<h3>Guest Access — Meeting Settings</h3>
<h4>Screen sharing was explicitly disabled for guest participants in meetings. This prevents external users from presenting content to internal audiences without prior authorization — a meaningful data exposure risk in environments handling sensitive information.</h4>

<ul>
  <li><strong>Screen sharing</strong>- Not enabled</li>
  <li><strong>Video conferencing</strong>- On (guests can join with video)</li>
  <li><strong>External participants can get control</strong>- Off</li>
</ul>

<img src="https://i.imgur.com/CtFe80b.png" />

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

<img src="https://i.imgur.com/0j4pSDM.png"/>


<h2>4. Collaboration Security & SharePoint Governance</h2>
<p>A SharePoint Team Site was created for the IT Support team using the Help Desk template. The site was provisioned as a private group — meaning membership is controlled and the site is not discoverable by the broader organization. This configuration is appropriate for teams handling sensitive operational data such as incident tickets, internal runbooks, or escalation procedures.</p>

<img src="https://i.imgur.com/qKL3FTT.png"/>

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
<img src="https://i.imgur.com/ZjrTzIW.png"/>

<p>For further guidance on configuring SharePoint external sharing policies, see Microsoft's official documentation: https://learn.microsoft.com/en-us/purview/dlp-microsoft-teams?tabs=purview</p>



<h2>5. Mailbox Retention & Data Lifecycle Management</h2>
<p>A retention policy was applied to a user mailbox via the Exchange Admin Center. The Default MRM Policy was selected — Microsoft's built-in collection of retention tags applied to a mailbox at the folder level, governing how long items are retained before being moved to archive or permanently deleted.</p>
<h3>Policy applied:</h3>
<ul>
  <li><strong>Retention policy</strong>: Default MRM Policy</li>
  <li><strong>Sharing policy</strong>: Default Sharing Policy</li>
  <li><strong>Role assignment policy</strong>: Default Role Assignment Policy</li>
</ul>

<i>Why does retention matter?</i>
<br />
<i>Without a retention policy, user mailboxes grow indefinitely on the local client — eventually causing performance degradation or machine crashes. Retention policies automatically move older emails to a cloud archive, keeping the local mailbox lean while preserving data for legal, compliance, or business continuity purposes. The archive is accessible to the user at any time through Outlook or the web client.</i>
<br />
<h3>Enterprise context — third-party archiving tools</h3>
<p>While M365 provides native retention through MRM policies and Purview, many enterprises augment this with third-party solutions such as Mimecast, Proofpoint, or Veritas Enterprise Vault — particularly for long-term legal hold, advanced eDiscovery, or cross-platform archiving requirements. Familiarity with the native tooling provides the foundation for administering or integrating these platforms.</p>

<img src="https://i.imgur.com/rnl2Ndd.png" alt="Default Retention Policy"/>

<h2>6. Conditional Access</h2>
<p>Conditional Access policy configuration is covered in full in the dedicated Conditional Access subsection following Phase 3. That section documents policy design, conditions, access controls, and enforcement outcomes using the Microsoft Entra ID Conditional Access framework.</p>

<img src="https://i.imgur.com/hsH8d7b.png" alt="Conditional Access Dashboard"/>
[Conditional Access]()

<h2>7. Intune — Device Compliance & MDM</h2>
<p>Microsoft Intune configuration is covered in full in the dedicated Intune subsection following Phase 3. That section documents device enrollment, compliance policies, and integration with Conditional Access for device-based access enforcement.</p>

<img src="https://i.imgur.com/zrVzG7V.png" alt="Intune Dashboard"/>
[Intune Device Compliance & MDM]()

<h2>8. Purview Data Loss Prevention (DLP)</h2>
<p>A Data Loss Prevention (DLP) policy was configured in Microsoft Purview targeting US Financial Data / PCI DSS compliance, designed to detect and act on credit and debit card numbers across the tenant. The policy was applied across multiple locations — Exchange email, SharePoint sites, OneDrive accounts, and Teams chats — providing unified coverage regardless of where sensitive data is shared.</p>

<img src="blob:https://imgur.com/d46cedfa-6d4c-4cbe-92bb-91b31057d3ca" alt="Purview Dashboard"/>
[Purview Data Loss Prevention (DLP)]()


<h1>Key Takeaways</h1>
<ul>
<li><strong>Validated the Okta SAML SSO integration end-to-end, documenting the Tenant ID handshake, user assignment, and dual-account federation state</li>
<li><strong>Configured layered Teams governance across internal messaging policies and guest access settings, restricting message manipulation and screen sharing</li>
<li><strong>Standardized the Teams app bar experience via a custom setup policy including a third-party app integration (Calendly)</li>
<li><strong>Provisioned a private SharePoint Team Site for the IT support group with controlled membership and validated external sharing behavior</li>
<li><strong>Applied the Default MRM Policy to a user mailbox, establishing automated data lifecycle management and cloud archiving</li>
<li><strong>Established the Conditional Access and Intune frameworks as dedicated subsections within Phase 3</li>
</ul>
