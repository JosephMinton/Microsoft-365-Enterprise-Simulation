# [Microsoft 365 Enterprise Simulation ☁️](https://github.com/JosephMinton/Microsoft-365-Enterprise/blob/main/README.md)
# [Help Desk Scenarios 🛠️](https://github.com/JosephMinton/Microsoft-365-Enterprise-Simulation/blob/main/Helpdesk%20Scenarios.md)
# Outlook Command Line Switches 📨


<h2>Description</h2>
Outlook command line switches are special instructions you add to the end of the Outlook startup command. They tell Outlook to do something specific when it opens anything like skipping add-ins, clearing corrupted rules, or resetting the layout. These switches are run through the Windows Run dialog and are one of the fastest ways to fix common Outlook problems without touching the user's profile or making them wait hours for emails to download again.
<br />

<h2>Objective</h2>

<ul>
<li><strong>Use outlook.exe /safe to isolate and fix add-in related crashes</li>
<li><strong>Use outlook.exe /cleancategories to reset corrupted email category colors</li>
<li><strong>Use outlook.exe /cleanclientrules to clear corrupted inbox rules</li>
<li><strong>Use outlook.exe /cleanviews to restore the default Outlook layout</li>
<li><strong>Use outlook.exe /nopreview to bypass a corrupted email causing Outlook to hang</li>
</ul>

<h2>Technologies Used 🧪</h2>

<table>
  <tr>
    <td><b>Microsoft Outlook (Desktop)</b></td>
    <td><b>Windows Run Dialog</b></td>
  </tr>
  <tr>
    <td><b>COM Add-ins Manager</b></td>
    <td><b>Windows 11</b></td>
  </tr>
</table>

<h2>Switch 1</h2>
<h2>outlook.exe /safe - Launch Without Add-ins</h2>
<p>When Outlook crashes every time it opens, the most likely cause is a broken or conflicting add-in. Running Outlook in Safe Mode loads the application without any add-ins active, which tells you immediately whether an add-in is the problem. If Outlook opens fine in Safe Mode but crashes normally, the add-in is the culprit.</p>
<h3>To run it, press Windows + R to open the Run dialog, type the command, and press OK.
</h3>

```powershell
outlook.exe /safe
```

<img src="https://i.imgur.com/AjCSYI4.png"/>

<p>Outlook opens in Safe Mode with a simplified interface. Once inside, navigate to File, then Options, then Add-ins. At the bottom of the page, make sure the Manage dropdown is set to COM Add-ins and click Go.</p>
<img src="https://i.imgur.com/zz1Gr0G.png"/>

<p>The Add-ins page in Outlook Options shows all active and inactive add-ins installed on the machine. From here, clicking Go opens the COM Add-ins window where individual add-ins can be toggled on or off.</p>
<img src="https://i.imgur.com/LxZxool.png"/>

<p>In the COM Add-ins popup, uncheck the add-in that is causing the problem. Sometimes Outlook has already unchecked it automatically after a crash. Enabling it again or leaving it off and restarting Outlook normally is usually enough to resolve the issue.</p>
<img src="https://i.imgur.com/Xa0Bknd.png"/>


<h2>Switch 2</h2>
<h2>outlook.exe /cleancategories - Reset Category Colors</h2>
<p>Outlook lets users assign color categories to emails for quick visual organization. In shared mailbox environments, these categories can become corrupted or stop syncing correctly across team members. Running this switch clears all custom category names and color assignments and restores Outlook's default category set. Below are two visuals compared to one another.</p>

```powershell
outlook.exe /cleancategories
```
> *__Warning__*
>
> *This switch removes all custom category names and colors permanently. Any categories the user has created will need to be set up again after running it. Make sure the user is aware before running this command.*

<img src="https://i.imgur.com/bBzGQ7u.png"/>
<img src="https://i.imgur.com/b7N8X9r.png"/>


<h2>Switch 3</h2>
<h2>outlook.exe /cleanclientrules - Clear Corrupted Inbox Rules</h2>
<p>Inbox rules in Outlook automatically sort, move, flag, or delete incoming emails based on conditions the user sets up. Over time these rules can become corrupted. The rules they stop running, throw error messages, or conflict with each other. This switch deletes all client side rules, giving the user a clean slate to rebuild from.</p>

```powershell
outlook.exe /cleanclientrules
```

<p>The before and after screenshots below show a rule called "sent only to me" that was active before the command was run. To cofirm the client rules were sucessful, the same rule appears unchecked after running the swtich.</p>

<img src="https://i.imgur.com/4g7S1hP.png"/>
<img src="https://i.imgur.com/rxmEBhy.png"/>

> *__Client rules vs server rules__*
>
> *This switch only clears client side rules that run on the local machine inside the Outlook app. Server side rules, which run in Exchange Online and apply even when Outlook is closed, are not affected. If the user has important rules they want to keep, server side rules should be checked first in the Exchange Admin Center before running this command.*



<h2>Switch 4</h2>
<h2>outlook.exe /cleanviews - Reset the Outlook Layout</h2>
<p>Outlook allows users to heavily customize the layout - column widths, sort orders, reading pane positions, folder views, and more. If a user has made too many changes and the interface becomes confusing or broken, this switch resets everything back to the default view. It is also useful when a layout change causes certain emails or folders to appear missing.</p>

```powershell
outlook.exe /cleanviews
```

<p>The two screenshots below show the difference. The first shows Outlook with a customized layout - tighter spacing, adjusted columns, and a modified view. After running the switch, Outlook reverts to the standard default layout with all views restored to their original settings.</p>

<img src="https://i.imgur.com/dLdWCi9.png"/>
<img src="https://i.imgur.com/EEBIkpF.png"/>



<h2>Switch 5</h2>
<h2>outlook.exe /nopreview - Disable the Reading Pane on Launch</h2>
<p>The reading pane automatically previews whichever email is selected when Outlook opens. If a corrupted or unusually large email is at the top of the inbox, Outlook may freeze or crash every time it tries to render it 
in the preview. Running this switch opens Outlook without the reading pane active, letting the administrator access the inbox and deal with the problematic email without triggering the crash.</p>

```powershell
outlook.exe /nopreview
```

<img src="https://i.imgur.com/V27usXq.png"/>

> *__What to do once Outlook is open__*
>
> *With the reading pane off, locate the email that was causing the issue and delete it or move it to another folder. Once the problematic email is out of the way, Outlook can be restarted normally and the reading pane will come back on its own.*

<h1>Key Takeaways</h1>
<ul>
<li><strong>Command line switches let an administrator fix specific Outlook problems quickly without deleting profiles or reinstalling software</li>
<li><strong>outlook.exe /safe isolates add-in conflicts by loading Outlook without any plugins active</li>
<li><strong>outlook.exe /cleancategories resets corrupted category colors - useful for shared mailbox environments where categories stop syncing</li>
<li><strong>outlook.exe /cleanclientrules clears broken inbox rules while leaving server-side Exchange rules untouched</li>
<li><strong>outlook.exe /cleanviews restores the default Outlook layout when a user's customizations have made the interface unusable</li>
<li><strong>outlook.exe /nopreview bypasses a frozen reading pane caused by a corrupted or oversized email</li>
</ul>
