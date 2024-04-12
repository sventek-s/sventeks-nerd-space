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

## Disable Windows Search Suggestions

Disable Windows search suggestions via `regedit`. [Source](https://www.groovypost.com/howto/monitor-cpu-temperature-in-windows-10/)

```.sh
REG ADD HKCU\Software\Policies\Microsoft\Windows\Explorer /v DisableSearchBoxSuggestions /t REG_DWORD /d 1 /f >nul 2>&1
```

### Resources

[Fix Windows Search Bat File](https://gist.github.com/davidsaccavino/7f6487a7053322764889bd2271ff724a)
