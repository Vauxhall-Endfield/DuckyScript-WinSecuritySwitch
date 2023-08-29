# DisableWinUAC
DuckyScript-DisableWinUAC is a fork based on the project by [yokokho](https://github.com/yokokho/Another-Rubber-Duck-Payloads). The purpose of this project is simply to add new features to the original project.

## About
The original project contained a DuckyScript payload to disable all essential security measures on the target device, by reducing the level of security of the target device's User Account Control (UAC) settings to the minimum, disabling Automatic Sample Submission and Virus and Threat Protection in Windows Defender, and disabling Windows Firewall. All of these actions are made possible by instructing the DuckyScript payload to use Windows PowerShell.

Judging by the methodology of the original payload, it is possible for the end user to undo the changes made by the aforementioned payload by utilizing Windows PowerShell as well. This will require us to create a second payload, which will focus on restoring the default settings for the target device's User Account Control (UAC) settings, Windows Defender and Windows Firewall.
