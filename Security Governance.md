# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Security & Governance 🔒</h1>



<h2>Description</h2>
Phase 3 focuses on hardening the Microsoft 365 environment built in Phases 1 and 2. Security controls were applied across identity,
messaging, collaboration, and data layers. This defense in depth approach is often seen in enterprise IT and security operations. 
This phase also serves as the foundation for the Conditional Access, Intune and Purview subsections that follow.
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
</table>

<h2>1. Advanced Identity & SSO Integration</h2>
<p>The Okta SAML 2.0 integration was initially configured and documented in Phase 1, Section 1.6. The screenshots below confirm the integration is working, showing account status across both identity systems.</p>
<h3>In simple terms, users log into Okta once and get access to Microsoft 365 without needing a separate set of credentials. Setting this up requires Microsoft 365 to trust Okta, which is done by sharing a unique identifier called a Tenant ID.
</h3>
<img src="https://i.imgur.com/SUe9Cff.png" />
<p>Following configuration, the Office 365 application within Okta was assigned directly to the lab admin account. 
The user was assigned individually for this demo. In a real work environment, this would typically be done by group to save time and scale easily.</p>

<p>The Okta dashboard shows two accounts for the same person. The Gmail account is still pending setup, while the Microsoft 
365 account is fully active. This shows that both systems are successfully connected and communicating with each other.
</p>

<img src="https://i.imgur.com/pR6uZ8n.png" />




<h2>2. Teams Governance: Messaging & Guest Access Policies</h2>
<p>Microsoft Teams was configured to control who can communicate with whom. Two types of policies were set up. One for internal employees and one for external guests, ensuring communication stays within appropriate boundaries for a corporate environment.
</p>

<h3>Internal Messaging Policy</h3>

> *__Why restrict message deletion and editing?__*
>
> *Allowing users to delete or edit sent messages undermines the integrity of communication records. In regulated industries, chat logs may be subject to 
> compliance holds or eDiscovery requests. if the ability of deletion was permitted, there would likely be gaps within the audit trail.
> Turning these off ensures that messages cannot be edited or deleted across the organization.*

<h4>A custom messaging policy was configured with the following restrictions applied to internal users.</h4>

<img src="https://i.imgur.com/9PUMo8A.png" />

<h3>Guest Access: Messaging Settings</h3>
<h4>Guest access was left enabled to support B2B collaboration scenarios, but heavily restricted at the feature level. 
All editing and deleting options were turned off for guest users:</h4>

<h4>All message editing, deletion, and rich media features were disabled for both internal users and guests.</h4>

<img src="https://i.imgur.com/eOSIQWf.png" />


<h3>Guest Access: Meeting Settings</h3>
<h4>Screen sharing was explicitly disabled for guest participants in meetings. This prevents external users from presenting content to internal audiences without prior authorization.</h4>

<table>
  <tr>
    <td><b>Screen sharing</b></td>
    <td><b>Not enabled</b></td>
  </tr>
  <tr>
    <td><b>Video conferencing</b></td>
    <td><b>On (guests can join with video)</b></td>
  </tr>
  <tr>
    <td><b>External participants can get control</b></td>
    <td><b>Off</b></td>
  </tr>
</table>

<img src="https://i.imgur.com/CtFe80b.png" />

<h2>3. Teams App Setup Policy: Pinned Applications</h2>
<p>A custom Teams setup policy was configured to standardize the application bar experience for users assigned to the policy. Pinned apps are automatically installed and surfaced in the Teams sidebar for all policy members, ensuring consistent access to essential tools without requiring individual setup.</p>
<h3>Applications pinned in the following order:</h3>

<table>
  <tr>
    <td><b>1. Activity</b></td>
  </tr>
  <tr>
    <td><b>2. Chat</b></td>
  </tr>
  <tr>
    <td><b>3. Teams</b></td>
  </tr>
  <tr>
    <td><b>4. Calendar</b></td>
  </tr>
  <tr>
    <td><b>5. Calling</b></td>
  </tr>
  <tr>
    <td><b>6. OneDrive</b></td>
  </tr>
  <tr>
    <td><b>7. Calendly</b></td>
  </tr>
</table>

> *__Why standardize pinned apps via policy?__*
>
> *When every user configures their own Teams sidebar, you end up with inconsistency across the org. Some people missing key tools, while others cluttered with apps they don't need. Pinning apps through an admin policy gives everyone the same starting point, reduces onboarding friction, and cuts down on "where do I find X" support tickets. Adding a third party app like Calendly also shows that Teams can be extended beyond Microsoft's own toolset to fit how a team desires to work.*

<img src="https://i.imgur.com/0j4pSDM.png"/>


<h2>4. Collaboration Security & SharePoint Governance</h2>
<p>A SharePoint Team Site was created for the IT Support team using a Help Desk template. The site was provisioned as a private group, meaning membership is controlled and the site is not discoverable by the broader part of the organization. This configuration is appropriate for teams handling sensitive operational data such as incident tickets, internal runbooks, or escalation procedures.</p>

<img src="https://i.imgur.com/qKL3FTT.png"/>

<h3>Site details:</h3>

<table>
  <tr>
    <td><b>Site name</b></td>
    <td><b>Help Desk</b></td>
  </tr>
  <tr>
    <td><b>Template</b></td>
    <td><b>Team Site</b></td>
  </tr>
  <tr>
    <td><b>Type</b></td>
    <td><b>Private group</b></td>
  </tr>
  <tr>
    <td><b>Description</b></td>
    <td><b>IT Support Tech Team</b></td>
  </tr>
  <tr>
    <td><b>URL</b></td>
    <td><b><a href="https://josephmintontech.sharepoint.com/sites/HelpDesk/SitePages/ITHelpdeskHome.aspx?e=4%3AKBySh0&web=1&at=9">IT Support Tech Team</a></b></td>
  </tr>
</table>

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

> *__Why does retention matter?__*
>
> *Without a retention policy, user mailboxes grow indefinitely on the local client which could eventually cause performance degradation or machine crashes. Retention policies automatically move older emails to a cloud archive, keeping the local mailbox lean while preserving data for legal, compliance, or business continuity purposes. The archive is accessible to the user at any time through Outlook or the web client.*

<img src="https://i.imgur.com/rnl2Ndd.png" alt="Default Retention Policy"/>

# 6. [Conditional Access](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Conditional%20Access.md) 🚧
<p>Conditional Access policy configuration is covered in full in the dedicated Conditional Access subsection in the link below. That section documents policy design, conditions, access controls, and enforcement outcomes using the Microsoft Entra ID Conditional Access framework.</p>

<img src="https://i.imgur.com/hsH8d7b.png" alt="Conditional Access Dashboard"/>

# [Conditional Access](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Conditional%20Access.md) 🚧

# 7. [Intune: Device Compliance & MDM](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Intune%3A%20Device%20Compliance%20%26%20MDM.md) 📱
<p>Microsoft Intune configuration is covered in full in the dedicated Intune subsection link down below. That section documents device enrollment, compliance policies, and integration with Conditional Access for device based access enforcement.</p>

<img src="https://i.imgur.com/zrVzG7V.png" alt="Intune Dashboard"/>

# [Intune: Device Compliance & MDM](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Intune%3A%20Device%20Compliance%20%26%20MDM.md) 📱

# 8. [Purview: Data Loss Prevention](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Purview%20Data%20Loss%20Prevention%20(DLP).md) 🔍
<p>Data Loss Prevention configuration is covered in the dedicated subsection in the link below. That section documents the PCI DSS policy build, multi-location coverage across Exchange, SharePoint, OneDrive, and Teams, and real-world validation using a live Outlook Policy Tip test.</p>

<img src="https://i.imgur.com/A3uwt7g.png" alt="Purview Dashboard"/>

# [Purview: Data Loss Prevention](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Purview%20Data%20Loss%20Prevention%20(DLP).md) 🔍


<h1>Key Takeaways</h1>
<ul>
<li><strong>Validated the Okta SAML SSO integration, documenting the Tenant ID handshake, user assignment, and dual account federation trust</li>
<li><strong>Configured layered Teams governance across internal messaging policies and guest access settings, restricting message manipulation and screen sharing</li>
<li><strong>Standardized the Teams app bar experience via a custom setup policy including a third party app integration (Calendly)</li>
<li><strong>Provisioned a private SharePoint Team Site for the IT support group with controlled membership and validated external sharing behavior</li>
<li><strong>Applied the Default MRM Policy to a user mailbox, establishing automated data lifecycle management and cloud archiving</li>
<li><strong>Established Conditional Access, Intune, Purview frameworks as dedicated subsections</li>
</ul>
