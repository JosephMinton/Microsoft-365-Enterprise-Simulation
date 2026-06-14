# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Security & Governance 🔒</h1>



<h2>Description</h2>
Phase 3 focuses on hardening the Microsoft 365 environment built in Phases 1 and 2. Security controls were applied across identity,
messaging, collaboration, and data layers. This defense in depth approach is often seen in enterprise IT and security operations. 
This phase also serves as the foundation for the Conditional Access, Intune amd Purview subsections that follow.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Validate and document the Okta SSO integration established in Phase 1</li>
<li><strong>Configure Teams messaging and guest access policies to restrict unauthorized communication behaviors</li>
<li><strong>Customize Teams app setup policies to standardize the user experience</li>
<li><strong>Configure SharePoint site permissions and external sharing controls</li>
<li><strong>Apply a mailbox retention policy to manage data lifecycle and storage</li>
<li><strong>Establish the Conditional Access, Intune, and Purview frameworks covered in dedicated subsections</li>
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
  <tr>
    <td><b>Purview</b></td>
    <td><b></b></td>
  </tr>
</table>

<h2>1. Advanced Identity & SSO Integration</h2>
<p>The Okta SAML 2.0 integration was initially configured and documented in Phase 1, Section 1.6. The screenshots below confirm the integration is working, showing account status across both identity systems.</p>
<h3>In simple terms, users log into Okta once and get access to Microsoft 365 without needing a separate set of credentials. Setting this up requires Microsoft 365 to trust Okta, which is done by sharing a unique identifier called a Tenant ID.
</h3>
<img src="https://i.imgur.com/SUe9Cff.png" />
<p>Following configuration, the Office 365 application within Okta was assigned directly to the lab admin account. 
The user was assigned individually for this demo. In a real work environment, this would typically be done by group to save time and scale easily.<\p>

<p>The Okta dashboard shows two accounts for the same person. The Gmail account is still pending setup, while the Microsoft 
365 account is fully active. This shows that both systems are successfully connected and communicating with each other.
</p>

<img src="https://i.imgur.com/pR6uZ8n.png" />




<h2>2. Teams Governance - Messaging & Guest Access Policies</h2>
<p>Microsoft Teams was configured to control who can communicate with whom. Two types of policies were set up: 
one for internal employees and one for external guests, ensuring communication stays within appropriate boundaries for a corporate environment.al participants can do when invited into the tenant.
</p>

<h3>Internal Messaging Policy</h3>
<h4>A custom messaging policy was configured with the following restrictions applied to internal users:</h4>

<ul>
  <li><strong>Delete sent messages</strong> - Off</li>
  <li><strong>Edit sent messages</strong> - Off</li>
  <li><strong>Delete messages sent by bots</strong> - Off</li>
  <li><strong>Chat</strong> - Off</li>
  <li><strong>Giphy, Memes, Stickers</strong> - Off</li>
  <li><strong>URL previews</strong> - Off</li>
</ul>

<i>Why restrict message deletion and editing?</i>
<br />
<i>Allowing users to delete or edit sent messages undermines the integrity of communication records. In regulated industries, chat logs may be subject to 
compliance holds or eDiscovery requests. if the ability of deletion was permitted, there would likely be gaps within the audit trail.
Turning these off ensures that messages cannot be edited or deleted across the organization.<\i>

<img src="https://i.imgur.com/9PUMo8A.png" />

<h3>Guest Access — Messaging Settings</h3>
<h4>Guest access was left enabled to support B2B collaboration scenarios, but heavily restricted at the feature level. 
All editing and deleting options were turned off for guest users:</h4>

<ul>
  <li><strong>Edit sent messages</strong> - Off</li>
  <li><strong>Delete sent messages</strong> - Off</li>
  <li><strong>Delete chat</strong> - Off</li>
  <li><strong>Chat</strong> - Off</li>
  <li><strong>Giphy, Memes, Stickers</strong> - Off</li>
</ul>

<img src="https://i.imgur.com/eOSIQWf.png" />


<h3>Guest Access — Meeting Settings</h3>
<h4>Screen sharing was explicitly disabled for guest participants in meetings. This prevents external users from presenting content to internal audiences without prior authorization.</h4>

<ul>
  <li><strong>Screen sharing</strong> - Not enabled</li>
  <li><strong>Video conferencing</strong> - On (guests can join with video)</li>
  <li><strong>External participants can get control</strong> - Off</li>
</ul>

<img src="https://i.imgur.com/CtFe80b.png" />

<h2>3. Teams App Setup Policy — Pinned Applications</h2>
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
  <li>URL: [IT Support Tech Team](https://josephmintontech.sharepoint.com/sites/HelpDesk/SitePages/ITHelpdeskHome.aspx?e=4%3AKBySh0&web=1&at=9)</li>
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
<p>A Data Loss Prevention policy was configured in Microsoft Purview to detect sensitive financial data — specifically credit and debit card numbers — across the tenant. The policy was built around the US Financial Data and PCI DSS compliance template and applied across Exchange email, SharePoint, OneDrive, and Teams, meaning any attempt to share card data in any of those locations gets flagged automatically. When a user tries to send a credit card number via email, a Policy Tip appears in real time warning them the content may be blocked — stopping the leak before it happens. Newly created DLP policies can take up to 2 hours to sync across locations, or up to 24 hours when scoped to specific users or groups.</p>

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
