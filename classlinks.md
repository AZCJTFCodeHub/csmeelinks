## 04 Digital Forensics
These links and steps are meant to be referred to during our digital forensics discussion.

### Atomic Red Team
1. Atomic Red Team Install:
```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1'); Install-AtomicRedTeam -getAtomics
```
NOTE: If you close PowerShell and need to run Atomic Red Team again after installing it, use this command:
```powershell
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
```
1. Examining a specific technique.
```powershell
Invoke-AtomicTest T1078.003 -ShowDetailsBrief
Invoke-AtomicTest T1078.003 -CheckPrereqs
Invoke-AtomicTest T1078.003 -GetPrereqs
```
1. Look up the details of the attack that MITRE has documented here:
```html
https://attack.mitre.org/techniques/T1078/003/
```

1. In the MITRE ATT&CK Navigator, add the attack artifact to the current layer. This is just an exercise step for familiarity, but otherwise doesn't have a practical function.
https://mitre-attack.github.io/attack-navigator/

1. Run exploit (Don't do this, for reference only):
```powershell
Invoke-AtomicTest T1078.003
```
1. Uninstalling it.
```powershell
Invoke-AtomicTest T1078.003 -Cleanup
```
### Zimmerman Tools
1. Download the zimmerman tools: 
```powershell
https://f001.backblazeb2.com/file/EricZimmermanTools/Get-ZimmermanTools.zip
```
Extract the PowerShell script to C:\Working\Zimmerman

Open an administrative PowerShell prompt and change to the folder:
```powershell
CD C:\Working\Zimmerman
```
Run the ‘get-zimmermantools.ps1’ script to download tools. They will be placed in C:\Working\Zimmerman\net9

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
1. Use timeline explorer to view the results in a friendlier way.
```powershell
C:\Working\Zimmerman\Net9\TimelineExplorer.exe
```

### KAPE
1. Download KAPE:
```powershell
https://www.kroll.com/en/services/cyber/incident-response-recovery/kroll-artifact-parser-and-extractor-kape
```
2. Extract it to C:\Working\KAPE
3. Run C:\Working\KAPE\GKAPE.exe
4. Getting started. Show launching of Kape.
   1. Show difference between GUI and command line Kape versions.
5. Look at Targets.
   1. Walk through configuration options of Targets.
   2. Examine the data collected by Targets.
6. Look at Modules.
   1. Walk through configuration options of modules.
   2. Examine the data collected by Modules.

### Velociraptor
1. Velociraptor Overview. Show an overview of the interface.
2. Standup a server. Show command line for generating config file.
   1. Show how client config file is generated.
   2. Start the server
```powershell
.\velociraptor-v0.74.5-windows-amd64.exe --config .\server.config.yaml frontend -v
```
1. Fire up client. Show bubbles going green.
```powershell
.\velociraptor-v0.74.5-windows-amd64.exe --config .\client.config.yaml pool_client --number 100
```