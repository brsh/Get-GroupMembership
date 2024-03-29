### Inventory/audit group memberships on servers and domains

I had an old vbscript that did this ... but it was old. And it was vbscript.

Now, how hard could it be to re-write that simple vbscript into Powershell? There already were functions that did kinda what I wanted (get-adgroupmember, for example). BUT nothing did quite what I wanted (get-adgroupmember doesn't work locally). And then I wanted to make pictures of the info??!!! That's just crazy talk.

So, long story short: this script will inventory group memberships - both local system and AD - and do it recusively - both local system and AD. 

#### Requires: 
* PowerShell v4 (lightly tested) or v5/5.1 (heavily tested)
* The PowerShell ActiveDirectory module
  * On Servers, it's a Features (as in Add Roles and Features) under Remote Server Administration Tools->AD DS and AD LDS Tools
  * On Workstations, search Microsoft's site for the RSAT (Remote Server Administration Tools) package - i.e., it's an add-on
    * I think it's now built-in on Windows 10+

#### Features: 
* Recursive!
* Recursion depth configurable
* Local and AD
* Supports Computer by name, by ip, and, if local, by . (that's a dot, by the way)
* Outputs to png
* Outputs objects
* Supports pipeline (computernames in and users out)
* Cool tree structure
* Tree depth indicator configurable
* Or no tree structure - your choice!
* Line Item and Group Member counts

The Default output is remote style as string/text. Use the -raw switch to get PSObjects.

#### Command Line:
```
C:\Scripts\GetGroupMembers\Get-GroupMembership.ps1 -Computer <String[]> -group <String> [-depth <Int16>] [-LevelIndicator <Char>] [-Picture] [-folder <String>]

C:\Scripts\GetGroupMembers\Get-GroupMembership.ps1 -Computer <String[]> -group <String> [-depth <Int16>] [-LevelIndicator <Char>] [-raw]

C:\Scripts\GetGroupMembers\Get-GroupMembership.ps1 [-Domain <String>] -group <String> [-depth <Int16>] [-LevelIndicator <Char>] [-raw]

C:\Scripts\GetGroupMembers\Get-GroupMembership.ps1 [-Domain <String>] -group <String> [-depth <Int16>] [-LevelIndicator <Char>] [-Picture] [-folder <String>]
```

#### Examples:
```
.EXAMPLE 
    PS C:\> .\Get-GroupMembership.ps1 -Computer ThatMachine -Group Administrators

    Outputs the group members from ThatMachine

.EXAMPLE 
    PS C:\> get-content computers.txt | .\Get-GroupMembership.ps1 -Group Administrators

    Outputs the group members from all systems listed in the file computers.txt

.EXAMPLE 
    PS C:\> ".", "server1", "server2" | .\Get-GroupMembership.ps1 -Group Administrators

    Outputs the group members from the local system, server1, and server2

.EXAMPLE 
    PS C:\> .\Get-GroupMembership.ps1 -Computer ThatMachine -Group Administrators -raw

    Outputs a PSObject for further modification or piping to additional cmdlets

.EXAMPLE 
    PS C:\> .\Get-GroupMembership.ps1 -Computer ThatMachine -Group Administrators -picture

    Creates a .png of the data, named a la computer_group_date-time

.EXAMPLE 
    PS C:\> .\Get-GroupMembership.ps1 -Group Administrators
    
    Outputs the group members from the current domain

.EXAMPLE 
    PS C:\> .\Get-GroupMembership.ps1 -Domain MyADDomain -Group Administrators

    Outputs the group members from the domain MyADDomain
```
