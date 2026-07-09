this# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Help Desk Scenarios 🛠️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)


<h2>Description</h2>
Phase 4 shifts from building and securing the environment to operating and maintaining it. This phase covers the day to day administrative tasks that IT support staff 
encounter in real organizations like setting up communication addresses, configuring mail routing, tracking down missing emails, supporting mobile users, 
and resolving common Outlook issues. It also introduces command line troubleshooting and PowerShell automation as more advanced tools in the administrator's toolkit.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Configure a SIP address and a shorter email alias for cleaner communication</li>
<li><strong>Set up email forwarding for users who have changed roles or left the organization</li>
<li><strong>Use Message Trace to track and investigate mail delivery issues</li>
<li><strong>Support mobile users through Outlook Mobile settings and account management</li>
<li><strong>Troubleshoot common Outlook issues including add in conflicts, search failures, and credential problems</li>
<li><strong>Use Outlook command line switches for targeted repairs without touching the user's profile</li>
<li><strong>Automate administrative tasks using PowerShell</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Exchange Admin Center</b></td>
    <td><b>Outlook (Desktop and Mobile)</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Defender XDR</b></td>
    <td><b>Windows Credential Manager</b></td>
  </tr>
  <tr>
    <td><b>Windows Services</b></td>
    <td><b>Control Panel</b></td>
  </tr>
  <tr>
    <td><b>PowerShell</b></td>
    <td><b>Outlook Command Line Switches</b></td>
  </tr>
  <tr>
    <td><b>Outlook Mobile (iOS)</b></td>
    <td><b>Windows 11</b></td>
  </tr>
</table>

<h2>1. Unified Communications: SIP Address & Email Alias Setup</h2>
<p>A SIP (Session Initiation Protocol) address is a contact identifier used in voice and video communication systems. It works like a phone number that lives everywhere at 
once. Other users can call or message a person using their SIP address across platforms like Teams or Skype for Business. In Exchange, 
each mailbox can have multiple address types assigned to it including SIP, SMTP (standard email), and SPO (SharePoint).</p>
<h3>In this example, the default mailbox showed three address types already in place:
</h3>

<ul>
  <li><strong>SPO</strong>: SharePoint Online identifier</li>
  <li><strong>SIP</strong>: set as the default reply address for unified communications</li>
  <li><strong>SMTP</strong>: the standard email address</li>
</ul>

<img src="https://i.imgur.com/kjArKvo.png"/>

<h3>A new SMTP alias was added to shorten the user's address. Instead of typing out the full display name format, other users can now reach the same mailbox using a simpler address
</h3>

<ul>
  <li><strong>JoeM@JosephMintonTech.onmicrosoft.com</strong></li>
  <li>This was set as the primary email address going forward.</li>
</ul>

<i>Why add an email alias?</i>
<br />
<i>Long or complex email addresses slow people down and lead to typos. Adding a shorter alias gives users a cleaner address to share while the original address continues to 
work. This is common when a user changes their name, changes roles, or simply needs a more practical address for external communication.</i>
<br />

<img src="https://i.imgur.com/4iJ2vPi.png"/>
<img src="https://i.imgur.com/cDJulrA.png"/>


<h2>2. Mail Flow: Email Forwarding</h2>
<p>Email forwarding is used when a user changes roles or leaves the organization and someone else needs to receive their incoming mail. Instead of the email bouncing back or sitting unread, 
it gets automatically redirected to another mailbox. This keeps communication flowing without requiring the sender to update their contact information.</p>

<h3>In this example, Kevin Stroud's mailbox was configured to forward all incoming email to Carla Mendez. In this scenario, Kevin may have been reassigned or departed from the company and Carla is now covering his responsibilities during the role transitions.</h3>

<ul>
  <li><strong>Forward all emails sent to this mailbox</strong>: On</li>
  <li><strong>Forwarding address</strong>: Carla Mendez (internal)</li>
  <li><strong>Deliver message to both forwarding address and mailbox</strong>: Enabled</li>
</ul>

<i>Internal vs external forwarding</i>
<br />
<i>Forwarding to an internal address keeps mail within the organization and is straightforward to configure. Forwarding to an external address such as a personal Gmail account carries security risks because it moves company email outside 
of the tenant's control. Microsoft flags this with a warning in the Exchange Admin Center and recommends limiting external forwarding through outbound spam policies.</i>
<br />


<img src="https://i.imgur.com/yguR6Ku.png"/>



<h2>3. Tenant Health & Mail Flow Monitoring</h2>
<p>When a user reports that an email never arrived, the first tool to reach for is Message Trace in the Exchange Admin Center. It lets an administrator search for any email by sender, recipient, 
date, or subject and see if it was delivered, blocked, delayed, or bounced.</p>
</br>
<p>A Message Trace was performed to track an email sent from an external Gmail account to the lab tenant. The trace confirmed the message was successfully delivered to the recipient's inbox, including the exact delivery timestamp.</p>

<img src="https://i.imgur.com/dANd4hH.png"/>

<i>When an email looks suspicious</i>
<br />
<i>If a traced email appears to be spam, phishing, or malware, it can be submitted directly to Microsoft for analysis through the Microsoft Defender portal. The submission form accepts the email's network message ID, identifies the affected 
recipient, and allows the administrator to categorize the threat. Microsoft reviews the submission and uses it to improve detection across the platform.</i>
<br />

<img src="https://i.imgur.com/yeky4sg.png"/>


<h2>4. Mobile Workplace Support: Outlook Mobile</h2>
<p>Supporting mobile users is a routine part of IT support. Many users access their work email from their personal phones and run into issues ranging from missing 
Hiemails to sync failures. A few settings adjustments in Outlook Mobile resolve the majority of these complaints without needing to touch anything on the server side.</p>

<h2>Focused Inbox</h2>
<p>Focused Inbox automatically sorts emails into two tabs, Focused and Other, based on what it thinks is important. While well intentioned, this feature is one of the most common reasons users report missing 
emails. Turning it off puts all emails back into a single inbox view, which is simpler and more predictable for users who are not comfortable with the split.</p>

<img src="https://i.imgur.com/ewbyCX1.jpeg"/>

<h2>Automatic Replies</h2>
<p>Users who are away from their desk can set up automatic replies directly from their phone without needing access to a computer. This is useful during travel, time off, or periods of limited availability. 
The automatic reply was configured to reply to everyone with a message letting senders know to expect a response within two business days.</p>

<img src="https://i.imgur.com/y6Vj0vQ.jpeg"/>

<h2>Reset Account</h2>
<p>When a user's mobile email stops syncing, the first step is always to reset the account rather than remove it. Resetting restores the connection to the server without deleting any local data. 
Removing the account is a last resort because it wipes all locally stored email from the app and requires the user to sign back in and wait for everything to sync again.</p>

<img src="https://i.imgur.com/5IlZI72.jpeg"/>





<h2>5. Outlook Command Line Switches
</h2>
<p>Outlook includes a set of command line switches that let an administrator target and fix specific problems without deleting the user's entire profile. These are run through the Windows 
Run dialog and can resolve issues like corrupted inbox rules, broken views, or add in related crashes. Full documentation for this section is covered in the dedicated subsection.</p>

<h3>Dedicated documentation section coming soon</h3>

<h2>6. Outlook Troubleshooting</h2>
<p>This subsection covers hands on troubleshooting of the most common Outlook issues encountered in a support environment such as add in conflicts, broken search, repeated password prompts, application crashes, and Microsoft 365 repair. Each scenario 
includes the likely cause and a step by step resolution path. Full documentation is covered in the dedicated subsection.</p>

<h3>Dedicated documentation section coming soon</h3>


<h2>7. Advanced Administrative Automation with PowerShell</h2>
<p>PowerShell allows administrators to manage Microsoft 365 at scale. Powershell helps expedite tasks in seconds that would take hours through the admin portal. 
This subsection covers automation scripts for user management, reporting, and bulk operations across the tenant. Full documentation is covered in the dedicated subsection.</p>

<h3>Dedicated documentation section coming soon</h3>

<h1>Key Takeaways</h1>
<ul>
<li><strong>Configured a SIP address and a shorter email alias to improve communication clarity for end users</li>
<li><strong>Set up email forwarding in Exchange to redirect mail for a departed or reassigned user without disrupting incoming communication</li>
<li><strong>Used Message Trace to investigate a mail delivery scenario and submitted a suspicious email to Microsoft Defender for phishing analysis</li>
<li><strong>Resolved common mobile Outlook issues by disabling Focused Inbox, configuring automatic replies, and demonstrating the Reset Account workflow</li>
<li><strong>Introduced Outlook command line switches and PowerShell automation as dedicated subsections covering targeted repair and administrative efficiency</li>
</ul>
