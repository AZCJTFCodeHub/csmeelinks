## Digital Forensics
These links and steps are meant to be referred to during our digital forensics discussion.

We are going to run a technique that creates a new account on a system and places it in the administrators group on the box to achieve elevated permissions.

### Atomic Red Team
1. Install Atomic Red Team with the following command:
```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1'); Install-AtomicRedTeam -getAtomics
```
NOTE: If you close PowerShell and need to run Atomic Red Team again after installing it, use this command:
```powershell
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
```
2. Examining a specific technique. Run each of the commands below one at a time, examine the output of each command before running the next one.
```powershell
Invoke-AtomicTest T1078.003 -ShowDetailsBrief
Invoke-AtomicTest T1078.003 -CheckPrereqs
Invoke-AtomicTest T1078.003 -GetPrereqs
```
3. Look up the details of the attack that MITRE has documented here:
```http
https://attack.mitre.org/techniques/T1078/003/
```

4. In the MITRE ATT&CK Navigator, add the attack artifact to the current layer. This is just an exercise step for familiarity, but otherwise doesn't have a practical function.
```http
https://mitre-attack.github.io/attack-navigator/
```
5. Run the exploit with the below command (this is optional if you would prefer to not run this on your system. You can still do follow on steps, you may not see all expected results).
```powershell
Invoke-AtomicTest T1078.003
```
6. This command will uninstall the artifacts put in place for the technique.
```powershell
Invoke-AtomicTest T1078.003 -Cleanup
```
### Zimmerman Tools
1. Download the zimmerman tools: 
```http
https://f001.backblazeb2.com/file/EricZimmermanTools/Get-ZimmermanTools.zip
```

2. Extract the PowerShell script to C:\Working\Zimmerman

3. Open an administrative PowerShell prompt and change to the folder:
```powershell
CD C:\Working\Zimmerman
```

4. Run the ‘get-zimmermantools.ps1’ script to download tools. They will be placed in C:\Working\Zimmerman\net9

### MFTEcmd
1. Run mftecmd:
```powershell
C:\Working\zimmerman\net9\MFTECmd.exe -f 'c:\$mft' --csv c:\working\MFTEOutput
```
1. Look at mftecmd output
    1. Using the following command to use PowerShell's native 'Grid-View' to view the CSV data.
```powershell
Import-Csv -Path (Get-ChildItem C:\working\MFTEOutput -Filter *.csv | Sort-Object LastWriteTime -Descending | Select-Object -First 1).FullName | Out-Gridview
```
2. Use timeline explorer to view the results in a friendlier way.
```powershell
C:\Working\Zimmerman\Net9\TimelineExplorer.exe
```
3. Group the view by file extension by dragging the extension column to the header bar. On the right hand side, search for ps1.
4. Explore the interface for familiararity.
5. Close out timeline explorer.

6. Use Registry Explorer to look at registry.
```powershell
C:\Working\Zimmerman\Net9\RegistryExplorer\RegisterExplorer.exe
```
7. Click 'File', then Open, then Live System and click 'SAM'.
8. Drill down to 'Root\SAM\Domains\Account\Users\Names'
9. Observe listed names, look for anything out of place.
10. Click 'File', then Open, then Live System and click 'Amcache'.
11. Explore the entries listed here.
12. Close registry explorer.

### KAPE
1. Download KAPE:
```http
https://www.kroll.com/en/services/cyber/incident-response-recovery/kroll-artifact-parser-and-extractor-kape
```
2. Extract it to C:\Working\KAPE
3. Run C:\Working\KAPE\GKAPE.exe to launch the GUI version of KAPE.
4. Configure the target side to gather data from the system:
```Powershell
Target Source: C:\
Target Destination: C:\Target_Dest (New Folder)
Check: !Basic Collector and !SANS Triage
```
5. Configure the module side to process the collected data:
```PowerShell
Module Source: C:\Target_Dest
Module Destination: C:\Module_Dest (New Folder)
Check: !EZParser
```
6. Click 'Execute'.
7. Explore the analyzed files located in C:\Mod_dest using the TimelineExplorer.exe utility from earlier located in C:\Working\zimmerman\net9\TimelineExplorer.
8. When done, close Timeline Explorer.

### Velociraptor
1. Download velociraptor at the following URL:
```html
https://github.com/Velocidex/velociraptor/releases/download/v0.75/velociraptor-v0.75.1-windows-amd64.exe
```
2. Save it to:
```powershell
C:\working\velociraptor
```

3. We first need to generate a server configuration file by doing the following:
Run the following command:
```powershell
C:\working\velociraptor\velociraptor-v0.75.1-windows-amd64.exe config generate -i
```
A wizard is started. Use the following answers:
Self-signed certificate (Default) - Enter
Change OS to Windows - Enter
Defaults for everything else is fine.

4. Generate a client config file:
```powershell
C:\working\velociraptor\velociraptor-v0.75.1-windows-amd64.exe --config .\server.config.yaml config client > client.config.yaml
```
   1. Start the server
```powershell
c:\working\velociraptor\velociraptor-v0.75.1-windows-amd64.exe --config .\server.config.yaml frontend -v
```
1. Fire up client. Show bubbles going green.
```powershell
c:\working\velociraptor\velociraptor-v0.75.1-windows-amd64.exe --config .\client.config.yaml pool_client --number 100
```
