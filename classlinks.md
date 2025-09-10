## 04 Digital Forensics
### Atomic Red Team
1. Atomic Red Team Install
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1'); Install-AtomicRedTeam -getAtomics

NOTE: If you close PowerShell and need to run Atomic Red Team again after installing it, use this command:
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
2. Examining a technique.
Invoke-AtomicTest T1078.003 -ShowDetailsBrief
Invoke-AtomicTest T1078.003 -CheckPrereqs
Invoke-AtomicTest T1078.003 -GetPrereqs
3. Mapping to MITRE.
https://attack.mitre.org/techniques/T1078/003/

4. Take a look at the navigator
https://mitre-attack.github.io/attack-navigator/

5. Run exploit (Don't do this, for reference only):
Invoke-AtomicTest T1078.003
6. Uninstalling it.
Invoke-AtomicTest T1078.003 -Cleanup

### Zimmerman Tools
1. Download the zimmerman tools: https://f001.backblazeb2.com/file/EricZimmermanTools/Get-ZimmermanTools.zip
https://f001.backblazeb2.com/file/EricZimmermanTools/Get-ZimmermanTools.zip

Extract the PowerShell script to C:\Working\Zimmerman

Open an administrative PowerShell prompt and change to the folder:
CD C:\Working\Zimmerman

Run the ‘get-zimmermantools.ps1’ script to download tools. They will be placed in C:\Working\Zimmerman\net9

### MFTEcmd
1. Run mftecmd
C:\Working\zimmerman\net9\MFTECmd.exe -f 'c:\$mft' --csv c:\working\MFTEOutput
2. Look at mftecmd output
    1. Using grid-view
Import-Csv -Path (Get-ChildItem C:\working\MFTEOutput -Filter *.csv | Sort-Object LastWriteTime -Descending | Select-Object -First 1).FullName | Out-Gridview
3. Use timeline explorer to view the results in a friendlier way.
C:\Working\Zimmerman\Net9\TimelineExplorer.exe

### KAPE
1. Download KAPE:
https://www.kroll.com/en/services/cyber/incident-response-recovery/kroll-artifact-parser-and-extractor-kape
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
.\velociraptor-v0.74.5-windows-amd64.exe --config .\server.config.yaml frontend -v
3. Fire up client. Show bubbles going green.
.\velociraptor-v0.74.5-windows-amd64.exe --config .\client.config.yaml pool_client --number 100
