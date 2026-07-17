# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Tenant Architecture & Identity 🏗️</h1>



<h2>Description</h2>
This phase establishes the foundational Microsoft 365 environment used throughout the lab. Starting from a Business Premium trial tenant, the 
environment was progressively expanded with Azure, Entra ID P2, and Purview trials as the lab scope grew into security and compliance 
scenarios. Phase one covers tenant provisioning, initial admin navigation, user creation, bulk onboarding, role based access control, Entra ID 
directory exploration, and external identity provider integration via Okta SSO.
<br />

<h2>Objective</h2>

<p>Set up an Active Directory (AD) domain from scratch to simulate a corporate IT environment and practice standard AD administrative tasks.</p>
<ul>
<li><strong>Provision a Microsoft 365 tenant using the Business Premium 30 day trial</li>
<li><strong>Navigate and document the M365 Admin Center as the primary management interface</li>
<li><strong>Create user accounts following a standardized enterprise naming convention</li>
<li><strong>Configure role based access control (RBAC) using the principle of least privilege</li>
<li><strong>Explore the Microsoft Entra portal as the backend identity directory</li>
<li><strong>Integrate Okta as an external Identity Provider via SAML 2.0</li>
</ul>

<h2>Technologies Used 🧪</h2>

 - <b>Microsoft 365 Business Premium Trial</b> 
 - <b>Microsoft Entra ID (Azure AD)</b>
 - <b>M365 Admin Center</b>

<h2>Environment</h2>

- <b>Cloud based (no local VMs)</b> 

<h2>1. Environment & Admin Provisioning</h2>
<p>The lab environment was provisioned using the Microsoft 365 Business Premium 30 day trial, which provides 25 user licenses and access to core enterprise services including Entra ID, SharePiont, and Exchange. As the lab expanded into advanced security and compliance work, additional trials were layered in separately. Azure, Intune, and Okta activated as those phases of the lab required them.</p>
<p>This incremental approach reflects how organizations realistically license Microsoft services: purpose built access added per workload, rather than a single all inclusive SKU.</p>

<img src="https://i.imgur.com/LSaweR9.png" alt="M365 Business Premium Trial and 25 available licenses"/>

<p>Upon tenant activation, the M365 Admin Center was accessed as the primary management interface. The Admin Center provides centralized control over users, licenses, service
health, and billing. Because the interface is cloud hosted and updated frequently, familiarization was treated as an ongoing practice rather than a one time step.</p>

<img src="https://i.imgur.com/L18oreZ.png" alt="M365 Admin Center home page"/>

<h2>2. User Provisioning & Naming Conventions</h2>
<p>A user account was created manually through the Admin Center following a standard enterprise naming convention: first initial followed by last name. 
Consistent naming conventions support predictable account identifiers, simplified directory lookups, and cleaner audit trails.</p>

<h3>The following settings were configured during account creation:</h3>
<ul>
  <li>Display name and username following the defined convention</li>
  <li>"Force password change on first login" enabled as a security baseline</li>
  <li>License assignment applied at creation</li>
</ul>

> *__Why force a password change on first login?__*
>
> *System generated passwords are often distributed via email, chat, or printed handouts. All of which carry exposure risk. Forcing a change at first login ensures 
that no one other than the account owner retains a working credential, closing a common attack vector especially relevant during bulk onboarding.*

<img src="https://i.imgur.com/lFsM1bb.png" alt="Add a user"/>
<img src="https://i.imgur.com/W3JnPpS.png" alt="Display force change password on first login"/>

<h2>3. Scalability & Bulk Onboarding</h2>
<p>To simulate enterprise scale provisioning, multiple user accounts were created simultaneously using a "list of users" bulk import through the Admin Center. 
This method is standard practice during large onboarding events such as company mergers, seasonal hiring, or system migrations where manual 
creation would be both time prohibitive and error prone.</p>

<h3>The bulk template included the following fields:</h3>
<ul>
  <li>Display name</li>
  <li>Username</li>
  <li>Initial password</li>
  <li>License assignment flag</li>
</ul>
<p>A successful import confirmation was recorded, validating the template structure and end-to-end import process.</p>

<img src="https://i.imgur.com/ChTqqv4.png" alt="Bulk import"/>

<h2>4. Identity Governance & Role Based Access Control (RBAC)</h2>
<p>Role assignment was configured following the principle of least privilege. Meaning users should hold only 
the permissions necessary for their designated function. Rather than assigning Global Administrator broadly, a scoped role was applied to a support account.</p>

<h3>The Helpdesk Administrator role was assigned, which includes:</h3>
<ul>
  <li>Password resets for nonadmin users</li>
  <li>Service request submission</li>
  <li>Service health monitoring</li>
</ul>

<h3>The Helpdesk Administrator role explicitly excludes:</h3>
<ul>
  <li>Tenant level configuration changes</li>
  <li>License management</li>
  <li>Security policy modification</li>
  <li>Deletion of users or the tenant</li>
</ul>

> *__Why avoid Global Administrator for everyday accounts?__*
>
> *Global Admin is the highest privilege level in a Microsoft 365 tenant. It's basically the equivalent to root access. Assigning it broadly increases the 
blast radius of any compromised account. Scoped roles like Helpdesk Administrator limit what an attacker can do if credentials are stolen, and enforce 
accountability by restricting access to only what each role legitimately requires.*

<img src="https://i.imgur.com/9kVSe6Z.png" alt="Manage Roles pane: User checked, Global Administrator unchecked"/>

<h2>5. Directory Architecture & Entra ID Backend</h2>
<h3>Navigation was performed from the M365 Admin Center into the Microsoft Entra portal to examine the underlying identity directory. The two portals serve distinct but related purposes:</h3>
<ul>
  <li>M365 Admin Center: simplified interface for routine administrative tasks (users, licenses, service health)</li>
  <li>Microsoft Entra Portal: full identity infrastructure including sign in logs, audit trails, Conditional Access policies, and directory level role assignments</li>
</ul>
<p>Users created via the Admin Center were confirmed visible in the Entra portal with their configured attributes, verifying directory consistency between the two interfaces.</p>

<img src="https://i.imgur.com/RGhiJsX.png" alt="Entra ID user list"/>

<h2>6. Advanced Identity Integration: Okta SSO 🔒</h2>
<p>As a capstone to Phase 1, Okta was integrated as an external Identity Provider (IdP) for the Microsoft 365 tenant using SAML 2.0 federation. This configuration reflects 
a common enterprise architecture where organizations maintain an IdP outside of Microsoft's ecosystem typically to enable SSO across a broader portfolio of SaaS applications.</p>

<h3>Steps performed:</h3>
<ul>
  <li>Office 365 application added to the Okta dashboard</li>
  <li>SAML 2.0 assertions configured and mapped to the tenant's accepted domain</li>
  <li>Integration confirmed via successful application status in the Okta dashboard</li>
</ul>

> *__Why use SSO?__*
>
> *Single Sign-On reduces the total number of credential sets users must manage. Password related help desk tickets routinely account for 20–50% of request volume in large 
organizations and SSO directly reduces that load. It also consolidates authentication events into a single auditable stream, simplifying anomaly detection and incident response.*

<img src="https://i.imgur.com/Cuhp9ea.png" alt="Okta dashboard showing successfully integrated"/>
<img src="https://i.imgur.com/Fw2o00Y.png" alt="Integration"/>
<img src="https://i.imgur.com/8aANwtW.png"/>

<h1>Key Takeaways</h1>
<ul>
<li><strong>Provisioned a Microsoft 365 tenant from scratch using publicly available trials, layering in Azure, Entra ID, and Okta as lab scope expanded</li>
<li><strong>Established enterprise standard user provisioning practices including naming conventions, forced password changes, and bulk onboarding</li>
<li><strong>Configured RBAC using the principle of least privilege, assigning scoped roles rather than broad Global Admin access</li>
<li><strong>Navigated the relationship between the M365 Admin Center and the Entra ID portal as two layers of the same identity directory</li>
<li><strong>Integrated Okta as an external IdP via SAML 2.0, simulating a federated enterprise identity architecture</li>
</ul>
