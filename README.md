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

  - [Tenant Architecture & Identity](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Tenant%20Architecture%20%26%20Identity.md) covers standing up a Microsoft 365 Business Premium trial tenant from scratch. Provisioning users manually and via bulk import, configuring scoped RBAC roles following least privilege, exploring the relationship between the M365 Admin Center and the Entra ID portal, and capping it off with an Okta SAML 2.0 SSO integration to simulate a federated enterprise identity architecture.

  - [User Lifecycle](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/User%20Lifecycle.md) Implemented centralized administrative control using Group Policy to enforce password policies, system restrictions, and enterprise wide configuration standards across the domain.

  - [Security Governance](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Security%20Governance) Enforced enterprise security standards by enabling Multifactor Authentication, configuring Conditional Access policies, and implementing role based access control. Demonstrated the principle of least privilege by scoping administrative roles to specific functions without granting unnecessary tenant wide permissions.

<!--  - [Helpdesk Scenarios](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/Helpdesk%20Scenarios) Resolved simulated help desk tickets covering common M365 client issues including Outlook profile repair, Teams configuration, and OneDrive sync failures. Each scenario is documented with the reported symptom, root cause, and step by step resolution.
