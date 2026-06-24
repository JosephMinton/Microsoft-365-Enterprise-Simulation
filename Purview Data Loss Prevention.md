# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Security Governance 🔒](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Purview Data Loss Prevention (DLP) 🔍</h1>


<h2>Description</h2>
Microsoft Purview brings together data governance, security, and compliance tooling in a single portal — covering 
everything from Data Loss Prevention to eDiscovery to Insider Risk Management. This section focuses specifically on 
DLP: building a policy that detects sensitive financial data anywhere it might be shared across the tenant, and proving 
that detection actually works in practice rather than just on paper.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Navigate the Microsoft Purview portal and identify the Data Loss Prevention solution</li>
<li><strong>Create a DLP policy using the built-in PCI DSS compliance template</li>
<li><strong>Apply the policy across multiple locations: Exchange, SharePoint, OneDrive, Teams, and on-premises repositories</li>
<li><strong>Validate the policy by triggering a real Policy Tip in Outlook</li>
<li><strong>Understand policy propagation timing across the tenant</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Purview Portal</b></td>
    <td><b>Data Loss Prevention (DLP)</b></td>
  </tr>
  <tr>
    <td><b>PCI DSS Policy Template</b></td>
    <td><b>Exchange Online</b></td>
  </tr>
  <tr>
    <td><b>SharePoint Online</b></td>
    <td><b>OneDrive</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Teams</b></td>
    <td><b></b></td>
  </tr>
</table>

<h2>1. Navigating to Data Loss Prevention</h2>
<p>The Microsoft Purview portal consolidates a wide range of compliance and governance solutions under a single navigation menu — Audit, 
Compliance Manager, Data Catalog, Data Loss Prevention, eDiscovery, Information Protection, Insider Risk Management, and more. 
Data Loss Prevention was selected from the Solutions menu to begin building a policy targeting sensitive financial data.</p>

<img src="https://i.imgur.com/BczfTkz.png" />



<h2>2. Building the PCI DSS Policy</h2>
<p>A new DLP policy was created using Microsoft's built-in PCI Data Security Standard (PCI DSS) template, which is purpose-built to detect the presence of credit
and debit card numbers. Rather than writing detection rules from scratch, the template provides pre-configured rules that align with payment card industry compliance requirements.</p>

| Setting | Value |
|---|---|
| Information to protect | PCI Data Security Standard (PCI DSS) |
| Locations | Exchange, SharePoint, OneDrive, Teams, On-premises repositories |
| Mode | Turn the policy on immediately |
| Rules | Low volume detected (PCI DSS), High volume detected (PCI DSS) |

<p>Covering all five locations in a single policy ensures that sensitive card data is caught wherever it might be shared — whether through an email, a SharePoint 
document, a synced OneDrive file, or a Teams message — rather than requiring separate policies per platform.</p>


<img src="https://i.imgur.com/yGNSC5I.png"/>

<i>Low volume vs. high volume detection rules</i>
<br />
<i>DLP policies can apply different actions depending on how much sensitive data is detected in a single piece of content. A low volume match — such as one credit card number — might trigger a warning, 
while a high volume match — such as a spreadsheet containing hundreds of card numbers — can trigger a stricter response like an outright block. This tiered approach avoids 
over-blocking minor, low-risk instances while still catching serious bulk exposure.</i>
<br />



<h2>3. Validating the Policy — Outlook Policy Tip</h2>
<p>To confirm the policy was actually working and not just configured on paper, a test email containing a fabricated credit card number was sent between two lab accounts. 
Outlook flagged the message immediately, surfacing a Policy Tip that identified two separate issues with the message.</p>
<h3>Issues flagged by the Policy Tip:</h3>
<ul>
  <li>Message was sent to people outside the organization</li>
  <li>Message contained sensitive information: Credit Card Number</li>
</ul>

<img src="https://i.imgur.com/Hva6FO4.png" />

<i>Why does real-time validation matter?</i>
<br />
<i>A DLP policy that exists only in the admin portal but has never been tested against real content is an assumption, not a proven control. 
Triggering an actual Policy Tip confirms the detection engine is live, correctly scoped to the right locations, and surfacing 
the right information to end users at the moment of risk — not just after an audit finds the leak.</i>
<br />


<h2>4. Policy Propagation Timing</h2>
<p>Newly created DLP policies do not take effect instantly across every location in the tenant.</p>

<h3>Sync time varies depending on scope:</h3>
<ul>
  <li><strong>Tenant-wide policies: Up to 2 hours to fully propagate</strong></li>
  <li><strong>Policies scoped to specific users or groups: Up to 24 hours to fully propagate</strong></li>
</ul>

<i>Operational takeaway</i>
<br />
<i>This delay is an important consideration during testing — if a policy doesn't appear to be working immediately after creation, the first step should be checking how 
much time has passed, not assuming the configuration is wrong. Planning around this propagation window also matters for incident response timelines in a live environment.</i>
<br />




<h1>Key Takeaways</h1>
<ul>
<li><strong>Navigated the Microsoft Purview portal and identified Data Loss Prevention among its broader suite of governance and compliance solutions</li>
<li><strong>Built a DLP policy using the built-in PCI DSS template, covering Exchange, SharePoint, OneDrive, Teams, and on-premises repositories in a single policy</li>
<li><strong>Validated the policy with a real-world test, confirming Outlook surfaced a Policy Tip flagging both an external recipient and a detected credit card number</li>
<li><strong>Understood the operational impact of policy propagation timing — up to 2 hours tenant-wide, up to 24 hours when scoped to specific users or groups</li>
</ul>
