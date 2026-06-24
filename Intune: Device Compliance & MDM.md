# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Security Governance 🔒](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
<h1>Intune: Device Compliance & MDM 📱</h1>


<h2>Description</h2>
This section demonstrates end to end device lifecycle management using Microsoft Intune. Starting from tenant configuration 
in Entra ID, devices were enrolled through both manual Entra ID join and Windows Autopilot. A zero touch provisioning method that 
automatically configures a device with corporate settings the moment a user signs in. Compliance and 
configuration policies were applied to enforce security baselines across enrolled devices.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Configure Entra ID device settings to enable MDM enrollment</li>
<li><strong>Enroll devices using both manual Entra ID join and Windows Autopilot</li>
<li><strong>Create dynamic device groups for automated policy targeting</li>
<li><strong>Extract hardware hashes and register devices for Autopilot provisioning</li>
<li><strong>Configure a compliance policy requiring BitLocker, Secure Boot, and minimum OS version</li>
<li><strong>Apply device restriction profiles to lock down Control Panel settings</li>
<li><strong>Validate enrollment and compliance status across the device inventory</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Intune (Endpoint Manager)</b></td>
    <td><b>Microsoft Entra ID</b></td>
  </tr>
  <tr>
    <td><b>Windows Autopilot</b></td>
    <td><b>Windows 11 (VMware)</b></td>
  </tr>
  <tr>
    <td><b>PowerShell</b></td>
    <td><b>Dynamic Device Groups</b></td>
  </tr>
  <tr>
    <td><b>Compliance Policies</b></td>
    <td><b>Device Restriction Profiles</b></td>
  </tr>
</table>

<h2>1. Intune Admin Center Overview</h2>
<p>The Microsoft Intune Admin Center serves as the central management hub for all enrolled devices, apps, and policies. The dashboard provides an at a glance view of the environment's health, presenting enrollment status, compliance state, configuration conflicts, and app installation results.</p>
<h3>At the time of documentation, the lab environment reflected:
</h3>

<ul>
  <li><strong>5 Windows devices enrolled</strong></li>
  <li>3 devices compliant, 2 not yet evaluated<strong></strong></li>
  <li><strong>No enrollment failures in the last 7 days</strong></li>
  <li><strong>No app installation failures</strong></li>
  <li><strong>No configuration policy conflicts</strong></li>
</ul>

<img src="https://i.imgur.com/WddSekO.png" />



<h2>2. Tenant & Device Settings Configuration</h2>

<h3>Before any device can be enrolled, Entra ID must be configured to allow it. Device settings were accessed through the Entra Admin Center under Devices and configured as follows:</h3>

| Setting | Value |
|---|---|
| Users may join devices to Microsoft Entra | All |
| Users may register their devices with Microsoft Entra | All |
| Require MFA to register or join devices | Yes |
| Maximum number of devices per user | 20 (reduced from default of 50) |

<i>Why reduce the device limit?</i>
<br />
<i>The default limit of 50 devices per user is far more than any individual needs and can mask unauthorized enrollments. 
Setting a realistic limit like 20 makes it easier to detect anomalies and prevents a compromised account from registering a large number of rogue devices.</i>
<br />


<img src="https://i.imgur.com/JSqbqQY.png" />





<h2>3. Manual Device Enrollment: Entra ID Join</h2>
<p>The first enrollment method demonstrated was a manual Entra ID join performed directly from a Windows VM. This method is used when a device needs to be brought under management without going through Autopilot. Like when an existing device being onboarded midlife.</p>
<h3>Steps performed on the Windows VM:</h3>
<ul>
  <li>1. Navigated to Settings > Accounts > Access work or school</li>
  <li>2. Selected Connect, then clicked "Join this device to Microsoft Entra ID" rather than entering an email address at the top</li>
  <li>3. Signed in with a licensed user's credentials</li>
  <li>4. Device joined and confirmed connected to the tenant</li>
</ul>

<i>Why use "Join this device to Microsoft Entra ID" instead of just entering an email?</i>
<br />
<i>Entering an email address at the top of the Connect screen only registers the device that only gives the organization limited management capability. Clicking "Join this device to Microsoft Entra ID" performs a full domain join, giving Intune complete management authority over the device including policy enforcement, compliance checks, and remote wipe.</i>
<br />

<img src="https://i.imgur.com/agTFT7R.png" />

<p>Following enrollment, the connection was confirmed in Windows Settings under Access work or school, showing the device linked to the JosephMintonTech Entra ID tenant under the enrolled user.</p>

<img src="https://i.imgur.com/hScAjo4.png"/>


<h2>4. Device Inventory: Entra ID & Intune</h2>
<p>Following enrollment, the full device inventory was reviewed across both the Entra Admin Center and the Intune Admin Center. The two views serve different purposes. Entra shows identity and join type, while Intune shows management and compliance status.</p>

<h3>The Entra device list showed 7 devices with the following characteristics:</h3>
<ul>
  <li>Most devices joined via Microsoft Entra join (full join)</li>
  <li>Some devices registered via Microsoft Entra registered (BYOD/partial enrollment)</li>
  <li>MDM column reflected Microsoft Intune for fully managed devices</li>
  <li>Compliance column showed a mix of compliant and noncompliant states depending on whether the device met policy requirements</li>
</ul>

<i>Entra joined vs. Entra registered</i>
<br />
<i>A device that is Entra joined is fully under organizational control. The device simply signs in with a work account and Intune can push any policy to it. A device that is Entra registered is typically a personal device where the user has added a work account but the organization has limited management authority. This distinction directly affects what compliance policies and Conditional Access rules apply to that device.</i>
<br />

<img src="https://i.imgur.com/lnmhzhu.png"/>

<p>The Intune device list displayed five managed devices. Some devices were intentionally left without full Intune licensing to demonstrate the difference. The devices without the required license appear in the list but present zero MDM management and cannot have policies applied.</p>

<img src="https://i.imgur.com/qbRvcfF.png"/>



<h2>5. Dynamic Device Groups</h2>
<p>A dynamic security group was created in Entra ID to automatically include Windows devices running a specific OS version. Unlike static groups where members are added manually, dynamic groups evaluate membership rules in real time. Any device that meets the rule is added automatically, and any device that no longer meets it is removed.</p>
<h3>Membership rule configured:</h3>
<ul>
  <li><strong>Rule 1</strong>: deviceOSType equals Windows</li>
  <li><strong>Rule 2</strong>: deviceOSVersion starts with 10.0.2</li>
</ul>

| Property | Operator | Value |
|---|---|---|
| `device.deviceOSType` | `-eq` | `"Windows"` |
| `device.deviceOSVersion` | `-startsWith` | `"10.0.2"` |

<i>Why use dynamic groups for device management?</i>
<br />
<i>In a growing organization, manually maintaining group membership is error prone and time consuming. Dynamic groups ensure that new devices are automatically included in the right policy scope the moment they enroll and meet the criteria. This way, no manual intervention is required. This is especially valuable for compliance policies and Autopilot deployment profiles.</i>
<br />

<img src="https://i.imgur.com/4M0L2MJ.png"/>

<p>The rule was validated using the built in validation tool, which confirmed that enrolled devices matching the criteria were correctly included in the group.</p>

<img src="https://i.imgur.com/kL1QrZa.png"/>

<h2>6. Windows Autopilot: Zero Touch Provisioning</h2>
<p>Windows Autopilot is a cloud based provisioning method that replaces traditional device imaging. Instead of manually configuring each machine, Autopilot uses a hardware identifier to recognize a device the moment it connects to the internet, then automatically applies corporate settings, apps, and policies, no administrator actions necessary.</p>

<h3><strong>Hardware Hash Extraction</strong></h3>
<p>To register the Windows VM with Autopilot, its unique hardware hash was extracted using a PowerShell script run directly on the machine. The script generated an AutopilotHWID.csv file containing the device's serial number and hardware fingerprint, which was then imported into the Intune portal.</p>

<img src="https://i.imgur.com/TnlqZZk.png"/>

<h3><strong>Device Import & Profile Assignment</strong></h3>
<p>The CSV was imported into the Windows Autopilot Devices list in Intune. Upon import, the device appeared with a profile status of "Not assigned" while the deployment profile was being configured.</p>

<img src="https://i.imgur.com/gkXlWtr.png"/>

<h3><strong>Deployment Profile: Out of Box Experience</strong></h3>
<h3>A user driven deployment profile was created to define the out of box experience for any device registered under this Autopilot configuration. Key settings configured:</h3>

| Setting | Value |
|---|---|
| Deployment mode | User Driven |
| Join to Microsoft Entra ID as | Microsoft Entra joined |
| User account type | Standard (no local admin rights) |
| Apply device name template | Yes - AUTOPILOT-%RAND:3% |
| Privacy settings and license terms | Hidden |

<i>Why set the user account type to Standard?</i>
<br />
<i>Giving end users local administrator rights on their machines is a common security gap. The security flaw allows them to install unauthorized software, disable security tools, or make system changes that break compliance. Setting the account type to Standard through Autopilot ensures that no user arrives on a new device with more access than they need.</i>
<br />

<img src="https://i.imgur.com/kCGooNL.png"/>

<p>After the profile was created and assigned to the dynamic device group, the Autopilot device list was refreshed and the VMware VM's profile status updated from "Not assigned" to "Assigned", confirming the device is now registered and ready for zero touch provisioning.</p>

<img src="https://i.imgur.com/SR8b0F0.png"/>




<h2>7. Compliance Policy: IT Devices</h2>
<p>A Windows 10/11 compliance policy named "IT Devices Compliance" was created to define the minimum security requirements a device must meet before being considered trusted. Devices that fail any of these checks are immediately marked noncompliant, which integrates directly with Conditional Access to block access to company resources.</p>

<h3>IT Devices Compliance: Policy Summary</h3>

| Setting | Value |
|---|---|
| Platform | Windows 10 and later |
| BitLocker | Required |
| Secure Boot | Required |
| Minimum OS version | Windows 11 |
| Action on noncompliance | Mark device noncompliant - Immediately |

<i>Why require BitLocker and Secure Boot?</i>
<br />
<i>BitLocker encrypts the device's hard drive which means if the device is lost or stolen, the data on it is unreadable without the recovery key. Secure Boot prevents the device from loading unauthorized software during startup, blocking a class of attacks that target the boot process before the operating system loads. Together, these two controls form the hardware security baseline for any managed Windows device.</i>
<br />


<img src="https://i.imgur.com/68AHz46.png"/>


<h2>8. Device Configuration Profile: Control Panel Restrictions</h2>
<p>A device restriction profile named "Control Panel Configurations" was created and assigned to the IT Devices group. This profile uses Intune's built in settings catalog to block specific areas of the Windows Control Panel and Settings app, preventing users from making changes that could compromise security or create support issues.</p>

<h3>Settings blocked in this profile:</h3>

<table>
  <tr>
    <td><b>System</b></td>
    <td><b>Network and Internet</b></td>
  </tr>
  <tr>
    <td><b>Apps</b></td>
    <td><b>Accounts</b></td>
  </tr>
  <tr>
    <td><b>Gaming</b></td>
    <td><b>Privacy</b></td>
  </tr>
  <tr>
    <td><b>Update and Security</b></td>
    <td><b></b></td>
  </tr>
</table>

<i>Why lock down Control Panel settings?</i>
<br />
<i>Allowing users unrestricted access to Control Panel settings is a common source of both security incidents and support tickets. Users modifying network settings, installing apps outside approved channels, or disabling security features can undermine the entire compliance posture. Locking these settings through policy ensures the device stays in a known, controlled state regardless of who is using it.</i>
<br />

<img src="https://i.imgur.com/3mVZuWB.png"/>

<p>The profile was reviewed and confirmed before creation, showing the full list of blocked settings and the assigned group "IT Devices", which contains 3 devices and 0 users.</p>

<img src="https://i.imgur.com/xDlsJ61.png"/>


<h1>Key Takeaways</h1>
<ul>
<li><strong>Configured Entra ID device settings to enable MDM enrollment for all users with a reduced device cap of 20 per user</li>
<li><strong>Enrolled devices using both manual Entra ID join and Windows Autopilot, demonstrating the difference between registered and fully joined devices</li>
<li><strong>Built a dynamic device group using OS type and version rules to automate policy targeting without manual membership management</li>
<li><strong>Extracted hardware hashes via PowerShell and registered a VM with Windows Autopilot, progressing from "Not assigned" to "Assigned" profile status</li>
<li><strong>Configured a user driven Autopilot deployment profile with Standard user accounts and a device name template for consistent naming at enrollment</li>
<li><strong>Enforced BitLocker, Secure Boot, and minimum OS version requirements through a compliance policy tied to Conditional Access</li>
<li><strong>Applied a device restriction profile blocking key Control Panel sections to maintain a controlled, auditable device state</li>
</ul>
