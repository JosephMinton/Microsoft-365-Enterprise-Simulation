<h1><a href="https://www.linkedin.com/in/josephkdminton/">Joseph Minton</a>'s Microsoft 365 Enterprise Lab ☁️</h1>

This project simulates a comprehensive Microsoft 365 enterprise environment, focusing on cloud based identity management, user administration, security governance, and help desk operations. The lab spans the full M365 administrative lifecycle, beginning with the provisioning of a Microsoft 365 Developer Tenant and the configuration of Microsoft Entra ID to manage users, groups, and organizational structure.

<h2>Technologies Used</h2>

<table>
  <tr>
    <td><b>Microsoft 365</b></td>
    <td><b>Microsoft Purview</b></td>
  </tr>
  <tr>
    <td><b>Microsoft 365 Admin Center</b></td>
    <td><b>Microsoft Defender</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Entra ID (Azure AD)</b></td>
    <td><b>Microsoft Health</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Exchange</b></td>
    <td><b>Microsoft Intune</b></td>
  </tr>
  <tr>
    <td><b>Microsoft SharePoint</b></td>
    <td><b>PowerShell</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Teams</b></td>
    <td><b>Okta</b></td>
  </tr>
  <tr>
    <td><b>Microsoft Outlook</b></td>
    <td><b>Conditional Access</b></td>
  </tr>
</table>

Centralized identity control is established through dynamic group membership, role based access control, and the enforcement of conditional access policies, which govern authentication requirements and restrict access based on user risk and device compliance. The project extends into practical help desk operations, simulating real world ticket scenarios involving Outlook, Teams, and SharePoint troubleshooting. The objective of this multipart series is to demonstrate practical competency in Microsoft 365 administration within a scalable cloud enterprise model.

  - [Tenant Architecture & Identity](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Tenant%20Architecture%20%26%20Identity.md) covers standing up a Microsoft 365 Business Premium trial tenant from scratch. Provisioning users manually and via bulk import, configuring scoped RBAC roles following least privilege, exploring the relationship between the Microsoft 365 Admin Center and the Entra ID portal, and capping it off with an Okta SAML 2.0 SSO integration to simulate a federated enterprise identity architecture.

  - [User Lifecycle](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/User%20Lifecycle.md) Managed the full user lifecycle inside a Microsoft 365 environment from assigning licenses and setting up group mailboxes to onboarding external contacts and securely offboarding departing employees. Access was intentionally scoped so users only had the tools and permissions they actually needed.

  - [Security Governance](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Security%20Governance.md) Hardened a Microsoft 365 environment by configuring security policies across identity, messaging, collaboration, and data. Controlled who could access what, restricted how users communicate in Teams, and set up automated rules to manage email data over time.

    - [Conditional Access](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Conditional%20Access.md) Configured sign in policies that evaluate who is authenticating, from what device, and how. Only after that process, user has granted or blocked access accordingly. Implemented a two tier model covering baseline controls for all users and stricter policies for admins, staff, and guests.

    - [Intune: Device Compliance & MDM](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Intune%20-%20Device%20Compliance%20%26%20MDM.md) Enrolled devices into Microsoft Intune through manual Entra ID join and Windows Autopilot, then enforced hardware security requirements, and locked down Control Panel settings to keep every managed device in a known, controlled state.
<!--    - [Purview DLP](your-link-here) Your description here.

  - [Helpdesk Scenarios](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Helpdesk%20Scenarios) Resolved simulated help desk tickets covering common M365 client issues including Outlook profile repair, Teams configuration, and OneDrive sync failures. Each scenario is documented with the reported symptom, root cause, and step by step resolution.
