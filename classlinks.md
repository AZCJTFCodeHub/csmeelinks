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
5. Run the exploit with the below command (don't do this for now, this is only for your reference):
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
1. Use timeline explorer to view the results in a friendlier way.
```powershell
C:\Working\Zimmerman\Net9\TimelineExplorer.exe
```

### KAPE
1. Download KAPE:
```http
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
   1. Show how client config file is generated.
   2. Start the server
```powershell
.\velociraptor-v0.74.5-windows-amd64.exe --config .\server.config.yaml frontend -v
```
1. Fire up client. Show bubbles going green.
```powershell
.\velociraptor-v0.74.5-windows-amd64.exe --config .\client.config.yaml pool_client --number 100
```