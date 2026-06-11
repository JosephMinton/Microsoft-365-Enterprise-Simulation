# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>User Lifecycle & Licensing</h1>



<h2>Description</h2>
This phase covers the full user lifecycle within a Microsoft 365 environment from license assignment and group configuration to shared resource management, external 
identity integration, and secure offboarding. Workflows were designed to reflect real enterprise operating procedures, including licensing decisions, and proper data 
preservation during user deletion.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Assign and manage licenses with selective app level control.</li>
<li><strong>Provision a Microsoft 365 tenant</li>
<li><strong>Configure Distribution and Security groups for organizational collaboration</li>
<li><strong>Create and manage Shared Mailboxes</li>
<li><strong>Integrate external contacts into the Global Address List with mail tips</li>
<li><strong>Provision Room and Equipment mailboxes for physical asset scheduling</li>
<li><strong>Execute a secure offboarding workflow with license reclamation and data retention</li>
</ul>

<h2>Technologies Used 🧪</h2>

 - <b>Microsoft 365 Business Premium Trial</b> 
 - <b>M365 Admin Center</b>
 - <b>Exchange Online</b>
 - <b>Microsoft Entra ID</b>
 - <b>Shared & Resource Mailboxes</b>
 - <b>Distribution & Security Groups</b>
 - <b>Mail Contacts & Mail Tips</b>

<h2>Environment</h2>

- <b>Exchange Online (cloud hosted)</b> 

<h2>1. Standardized Licensing & App Management</h2>
<p>A Business Premium license was assigned to a user with selective app level controls applied. Rather than enabling the full suite by default, unnecessary applications were manually toggled off. This reflects principles of least privilege applied to software access, not just permissions.</h3>
<ul>
  <li>Microsoft Defender: not applicable to the simulated org structure</li>
  <li>Intune: restricted to users with a defined automation workflow need</li>
</ul>

</ul>
<i>Why manage app access within a license?</i>
<br />
<i>Even within a single license tier, not every included application should be available to every user. Disabling unused apps reduces the attack surface. Fewer active services means 
fewer potential entry points. It also establishes a cleaner audit trail and reflects intentional access design rather than default permissiveness.</i>
<br />


<img src="https://i.imgur.com/FFk2Pyt.png" />




<h2>2. Organizational Collaboration & Group Configuration</h2>
<h3>Two group types were created to demonstrate the functional difference between collaboration and security grouping in Microsoft 365:</h3>
<ul>
  <li><strong>Distribution Group</strong> (All Staff) - an email only group used to reach a defined set of users simultaneously, such as a department or the entire organization</li>
  <li><strong>Security Group</strong> (HR Confidential) - used to control access to resources such as SharePoint sites, shared drives, or applications</li>
</ul>

<h3>Distribution Group vs. Security Group</h3>
<ul>
  <li><strong>Distribution Group</strong>: Email delivery only. Cannot be used to assign permissions to resources. Best used for team or department mailing lists.</li>
  <li><strong>Security Group</strong>: Used to grant or restrict access to M365 resources. Does not inherently have email capability unless mail enabled. 
Best used for role based access control.</li>
</ul>
<p>The allow external senders setting was enabled on the Distribution Group to permit inbound mail from outside the organization.
</p>

<img src="https://i.imgur.com/es4FtsB.png" />

<h2>3. Shared Resource Management & Mailboxes</h2>
<p>A Shared Mailbox was created to simulate a centralized support inbox 
(ServiceDeskSupport@domain.com) accessible by multiple administrators without requiring a dedicated per user license.</p>

<img src="https://i.imgur.com/Hn05fz3.png"/>


<h3>Permissions configured:</h3>
<ul>
  <li><strong>Full Access</strong> - allows reading and managing all mailbox content</li>
  <li><strong>Send As</strong> - allows sending email as the shared mailbox address rather than the individual admin's account</li>
</ul>

<i>Why use a Shared Mailbox?</i>
<br />
<i>Shared Mailboxes are free up to 50GB and do not require a dedicated license. They are a standard tool in enterprise environments for managing high volume or shared communication channels.</i>
<br />

<p>End-to-end functionality was validated by accessing the shared mailbox through Outlook, sending a test message to the support 
address, and confirming receipt and reply capability from within the shared inbox.</p>

<img src="https://i.imgur.com/0C3w7TF.png" alt=""/>
<img src="https://i.imgur.com/c0t06DA.png" alt="Admin account listed under Read and manage and Send as"/>
<img src="https://i.imgur.com/pQehFTj.png" alt="Shared mailbox visible in Admin Center"/>

<h3>Known Shared Mailbox Limitations</h3>
<p>Shared Mailboxes can exhibit inconsistent behavior in multiuser environments. Common issues include duplicate send events when multiple delegates are active simultaneously, 
and unreliable email categorization rules. Awareness of these limitations is relevant for troubleshooting and setting user expectations during deployment.
</p>

<h2>4. External Vendor Integration & Mail Contacts</h2>
<p>A Mail Contact was created for an external email address to make an outside party discoverable in the organization's company contact directory. 
This allows internal users to locate and email external contacts without needing to know their full address.</p>

<h3>Configuration included:</h3>
<ul>
  <li>External SMTP address mapped to the contact record</li>
  <li>Custom Mail Tip added: "This is an external consultant; do not share internal passwords."</li>
</ul>
<img src="https://i.imgur.com/zViOHQu.png" alt="HR contact Mail Tip: external SMTP address and custom Mail Tip visible"/>
<h3>A second Mail Tip was configured for an internal HR contact (Gwen Stacy) to manage response expectations:</h3>
<ul>
  <li>Mail Tip: "Please allow up to two business days for a response."</li>
</ul>
<img src="https://i.imgur.com/EyOyxVI.png" alt="HR contact Mail Tip: Please allow up to two business days for a response (Gwen Stacy)"/>
<i>What is a Mail Tip?</i>
<br />
<i>A Mail Tip is an automated advisory message that appears in Outlook when a user begins composing an email to a specific recipient. They are used to surface important context before 
a message is sent such as warning about external recipients or alerting users to sensitivity. Mail tips reduce misdirected emails and 
improve communication hygiene across the organization.</i>



<h2>5. Physical Resource & Facility Management</h2>
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
