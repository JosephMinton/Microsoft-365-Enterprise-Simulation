# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Security Governance 🔒](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Conditional Access</h1>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Entra ID</b></td>
    <td><b>Conditional Access Policies</b></td>
  </tr>
  <tr>
    <td><b>Authentication Strengths</b></td>
    <td><b>Temporary Access Pass (TAP)</b></td>
  </tr>
  <tr>
    <td><b>Access Reviews (Entra ID P2)</b></td>
  </tr>
</table>

<h2>Description</h2>
Conditional Access is how Microsoft 365 decides whether to let someone in — and under what conditions. Think of it as a set of 
rules that run every time a user tries to sign in. It checks who they are, what device they are using, and how they are authenticating 
before deciding to grant or block access. This lab implements a two-level model: a baseline that applies to everyone, 
and persona-specific policies that layer additional controls on top for admins, staff, and guest users.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Define a custom Modern MFA authentication strength</li>
<li><strong>Build five baseline security policies covering all users</li>
<li><strong>Implement persona-based policies for admins, staff, and guests</li>
<li><strong>Handle real-world exceptions using Temporary Access Passes</li>
<li><strong>Establish periodic Access Reviews for exclusion groups</li>
</ul>
 
<h2>Step 1: Naming Conventions & Persona Groups</h2>
<p>Before building any policies, a consistent naming convention was established. Vague names like "Policy 1" or "MFA Policy" make 
troubleshooting difficult as the number of policies grows. All policies follow a Who, What, Action structure:

`[Target Group] — [App or Resource] — [Requirement or Action]`

Four persona groups were created in Entra ID:

| Group | Description |
|---|---|
| CA-Admins | Highly privileged accounts |
| CA-Staff | General internal employees |
| CA-Guests | External contractors and partners |
| CA-Break-Glass | Emergency access accounts — excluded from most policies |

<img src="https://i.imgur.com/Tfxlg1Q.png" alt="Admin account listed under Read and manage and Send as"/>

<i>Why does the Break-Glass group matter?</i>
<br />
<i>If a Conditional Access policy is misconfigured and locks out all admins, a Break-Glass account is the emergency exit. It is intentionally excluded 
from restrictive policies so that access to the tenant can always be recovered. These accounts must be tightly controlled and audited regularly.</i>
<br />

<h2>Step 2: Baseline Security Policies</h2>
<p>These five policies form the security floor for the entire tenant. Every policy was initially set to Report-only mode — meaning it logs what would 
have happened without actually blocking anyone. This is a best practice that allows administrators to review impact 
in the sign-in logs before switching a policy to On and risking a lockout.
</p>


<h2>2.1 Define Modern MFA Authentication Strength</h2>
<p>Standard MFA that relies on SMS text codes or voice calls is no longer considered sufficient — both methods can be intercepted or socially engineered. 
A custom authentication strength named Modern MFA was created in Entra ID, defining which authentication methods are considered acceptable across the tenant.</p>
<h3>Methods included in Modern MFA:</h3>
<ul>
  <li>Windows Hello for Business</li>
  <li>Passkeys (FIDO2)</li>
  <li>Microsoft Authenticator (Push Notifications)</li>
  <li>Password + Microsoft Authenticator</li>
  <li>Temporary Access Pass (one-time use)</li>
</ul>

<img src="https://i.imgur.com/rSYYuWx.png" />


<h2>2.2 Require Strong MFA for All Users</h2>
<p>A policy was created requiring every user in the tenant to authenticate using the Modern MFA strength when 
accessing any cloud application. The CA-Break-Glass group was excluded to preserve emergency access.
</p>

<h3>All Users — All Apps — Require Strong MFA</h3>
<ul>
  <li><strong>Users</strong>: All users, CA-Break-Glass excluded</li>
  <li><strong>Target resources</strong>: All cloud apps</li>
  <li><strong>Grant</strong>: Require authentication strength — Modern MFA</li>
  <li><strong>State</strong>: On</li>
</ul>

<img src="https://i.imgur.com/LqZ5Iua.png" />


<h2>2.3 Block Legacy Authentication</h2>
<p>Legacy protocols like POP and IMAP do not support modern authentication methods at all — meaning they cannot prompt for MFA. 
Attackers frequently target these protocols specifically because they bypass MFA entirely. This policy blocks them at the tenant level.</p>

<h3>All Users — Legacy Auth Clients — Block Access</h3>
<ul>
  <li><strong>Users</strong>: All users, CA-Break-Glass excluded</li>
  <li><strong>Target resources</strong>: Client apps — Exchange ActiveSync and Other clients checked; Browser and Mobile apps unlocked</li>
  <li><strong>Grant</strong>: Block access</li>
</ul>

<img src="https://i.imgur.com/I9PhWQU.png"/>


<h2>2.4 Secure Device Registration</h2>
<p>When a device is registered to the tenant, it gains a level of trust that can be used by other policies. Requiring MFA at the point of device 
registration ensures that only authenticated users can enroll devices — preventing an attacker from registering a rogue device using stolen credentials alone.</p>

<h3>All Users — Device Registration — Require MFA</h3>
<ul>
  <li><strong>Users</strong>: All users</li>
  <li><strong>Target resources</strong>: User actions — Register or join devices</li>
  <li><strong>Grant</strong>: Require authentication strength — Modern MFA</li>
</ul>

<i>What is the User Actions toggle?</i>
<br />
<i>Instead of targeting a specific application, this policy targets a user action — the act of registering a device. This is a different configuration 
path than a standard app-based policy and is easy to miss. Switching "Cloud apps" to "User actions" in the Target resources dropdown reveals this option.</i>

<img src="https://i.imgur.com/hn4SvD3.png" alt=""/>


<h2>2.5 Block Device Code Flow</h2>
<p>Device code flow is an authentication method designed for devices without a browser — like smart TVs or printers. Attackers have repurposed it as a 
phishing technique, tricking users into approving authentication requests that hand over access tokens. Most 
businesses do not need this flow and blocking it removes the attack surface entirely.</p>
<h3>All Users — Device Code Flow — Block Acces</h3>
<ul>
  <li><strong>Users</strong>: All users</li>
  <li><strong>Target resources</strong>: Authentication flows — Device code flow</li>
  <li><strong>Grant</strong>: Block access</li>
</ul>

<img src="https://i.imgur.com/hxeUvvY.png" alt=""/>

<h2>Step 3: Persona-Based Policies</h2>
<p>Conditional Access uses additive logic — if a user belongs to multiple groups, the strictest policy that applies to them wins. 
Persona-based policies allow more granular control on top of the baseline, without having to touch the baseline policies themselves.</p>

<h2>3.1 Protect Administrative Accounts</h2>
<p>Admin accounts are the highest value targets in any Microsoft 365 tenant. A compromised admin account gives an attacker full control. This policy requires 
admins to use phishing-resistant MFA — meaning authentication methods that cannot be intercepted or faked, such as FIDO2 security keys or Windows Hello. 
Additionally, admins are required to be on a company-owned device.</p>
<h3>CA-Admins — All Apps — Phishing Resistant MFA</h3>
<ul>
  <li><strong>Users</strong>: CA-Admins group, CA-Break-Glass excluded</li>
  <li><strong>Target resources</strong>: All cloud apps</li>
  <li><strong>Grant</strong>: Require authentication strength — Phishing-resistant MFA</li>
</ul>

<img src="https://i.imgur.com/uV4Glty.png" alt=""/>

<h2>3.2 Protect Staff with Personal Smartphones</h2>
<p>Staff are allowed to use personal smartphones to access company resources, provided those devices meet compliance requirements. This policy was created 
separately from the admin policy intentionally — it allows staff device requirements to be adjusted independently without touching admin security settings. 
For example, staff can use a personal phone that passes compliance checks, while admins remain locked to company-owned hardware.</p>
<h3>CA-Staff — All Apps — Require Compliant Device</h3>
<ul>
  <li><strong>Users</strong>: CA-Staff group, CA-Break-Glass excluded</li>
  <li><strong>Target resources</strong>: All cloud apps</li>
  <li><strong>Conditions</strong>: Device filter — deviceOwnership Equals Company OR compliant device</li>
  <li><strong>Grant</strong>: Require device to be marked as compliant</li>
</ul>

<img src="https://i.imgur.com/iTU2jSY.png" alt=""/>


<h2>3.3 Restrict Guest Access to Browser Only</h2>
<p>Guest users — external contractors and partners — are forced to access company resources through a web browser only. Mobile apps and desktop clients 
are blocked. This prevents guests from syncing company files to their personal unmanaged devices, which is a common data leakage path.</p>
<h3>CA-Guests — All Apps — Block Mobile and Desktop Apps</h3>
<ul>
  <li><strong>Users</strong>: CA-Guests group</li>
  <li><strong>Target resources</strong>: All cloud apps</li>
  <li><strong>Conditions</strong>: Client apps — Mobile apps, desktop clients, Exchange ActiveSync, and Other clients checked; Browser unchecked</li>
  <li><strong>Grant</strong>: Block access</li>
</ul>

<img src="https://i.imgur.com/jQjkEzD.png" alt=""/>

<h2>Step 4: Managing Exceptions — Temporary Access Pass</h2>
<p>Real-world environments require exceptions. A user might lose their phone, forget their security key, or need emergency access. The solution is a 
Temporary Access Pass (TAP) — a short-lived, time-limited code issued by an administrator that lets a user sign in once without their usual authentication method.
In this scenario, a test user — Historia Reiss — simulates a CEO who has left their authentication device at home. Rather than creating a permanent 
workaround or weakening the policy, a one-time-use TAP was issued with a one hour expiration window.
</p>

<h3>Steps performed:</h3>
<ul>
  <li>Navigated to Users > Historia Reiss > Authentication methods</li>
  <li>Selected Add authentication method and chose Temporary Access Pass</li>
  <li>Set One-time use to Yes with a 1 hour activation duration</li>
  <li>TAP code provided to the user for immediate sign-in</li>
</ul>

<i>Why use a TAP instead of a permanent exclusion?</i>
<br />
<i>Creating a permanent policy exception for one user — even a CEO — introduces a persistent security gap. A TAP expires after one use, meaning the security 
perimeter is restored the moment the user signs in. It is the right tool for temporary access issues and avoids the risk of forgotten exclusions accumulating over time.</i>

<img src="https://i.imgur.com/7QTD1Tr.png" alt=""/>
<img src="https://i.imgur.com/bo0zJTw.png" alt=""/>

<h2>Step 5: Periodic Access Reviews</h2>
<p>Exclusion groups like CA-Break-Glass are a necessary part of any Conditional Access deployment, but they create a permanent gap in security 
coverage if left unmonitored. Access Reviews are scheduled audits that periodically verify whether users still need to be in a given group.</p>

<h3>Access Reviews were configured for the CA-Break-Glass group with the following settings:</h3>
<ul>
  <li><strong>Frequency</strong>: Monthly</li>
  <li><strong>Reviewer decision helpers</strong>: Enabled — flags users who have not signed in within 30 days</li>
  <li><strong>Action on denial</strong>: Remove access automatically</li>
</ul>

<i>Why review exclusion groups monthly?</i>
<br />
<i>Exclusion groups tend to grow quietly over time — a user gets added for a temporary reason and is never removed. Monthly reviews with automatic 
removal on denial ensure that the Break-Glass group stays small and intentional, rather than becoming a shadow list of policy bypasses.</i>


<h1>Key Takeaways</h1>
<ul>
<li><strong>Defined a custom Modern MFA authentication strength to move beyond SMS and voice-based verification</li>
<li><strong>Built five baseline policies covering strong MFA enforcement, legacy auth blocking, device registration security, and device code flow prevention</li>
<li><strong>Implemented persona-based policies that layer stricter controls on admins, allow compliant personal devices for staff, and restrict guests to browser-only access</li>
<li><strong>Used a Temporary Access Pass to handle a real-world exception without creating a permanent security gap</li>
<li><strong>Configured monthly Access Reviews for exclusion groups to prevent policy bypass accumulation over time</li>
<li><strong>Applied Report-only mode across all policies before enforcement to safely validate impact without risking user lockouts</li>
</ul>
