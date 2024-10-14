---
published: true
---

# Windows Configs

The following configs make my Windows Usage experience and usage better.

## App Execution Aliases

> Manage App Execution Aliases

Once you have installed Python and added its Path to the System Environment Variables.
You can disable the Python App Installer App execution aliases to avoid spawning the
Microsoft Store each time you write `python` or `python3` in the command prompt/Git Bash.

P.S; Another way to execute python `py`

## Disable Windows Bing Search Suggestions on Start Menu and on Task Bar Search

Disable Windows search suggestions via `regedit`. [Source](https://twitter.com/HenkPoley/status/1786284566837116991)

```.sh
# Disables task bar search suggestion via Command Prompt (Windows 10)
REG ADD HKCU\Software\Policies\Microsoft\Windows\Explorer /v DisableSearchBoxSuggestions /t REG_DWORD /d 1 /f >nul 2>&1

# Disables bing search suggestions via PowerShell (Windows 11)
Set-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" -Name "BingSearchEnabled" -Value 0 -Type DWord
```

## Group Policies to Disable

> Mostly for VulnResearch and ExpDev 😉😅

### Microst Defender

Computer Configuration -> Administrative Templates -> Windows Components -> Microsoft Defender Antivirus

Then Enable (Turn off Microsoft Defender Antivirus)

#### Microsoft Defender Real-time Protection

[Source](https://support.waters.com/KB_Inf/MassLynx/WKB203790_How_to_disable_Real_Time_Protection_in_Windows_10)

Computer Configuration -> Administrative Templates -> Windows Components -> Microsoft Defender Antivirus -> Real-time Protection

Then Enable (Turn off real-time protection)

#### Microsoft Defender Automatic Sample Submission and Cloud-deliverance Protection

Disable the following from Virus & threat protection settings in Windows Security

- Cloud-delivered protection

Computer Configuration -> Administrative Templates-> Windows Components -> Microsoft Defender Antivirus -> MAPS

Then Enable (Join Microsoft MAPS) under options set to `Disabled`.

- Automatic sample submission

Computer Configuration -> Administrative Templates-> Windows Components -> Microsoft Defender Antivirus -> MAPS

Then Enable (Send file samples when further analysis is required) under options set to `Never send`.

### Automatic Updates

Computer Configuration -> Administrative Templates -> Windows Components -> Windows Update -> Manage end user experience

Then Disable (Configure Automatic Updates)

### Windows Error Reporting

Computer Configuration -> Administrative Templates -> Windows Components -> Windows Error Reporting

Then Enable (Disable Windows Error Reporting)

### Resources

[Fix Windows Search Bat File](https://gist.github.com/davidsaccavino/7f6487a7053322764889bd2271ff724a)
