---
published: true
---

# TMUX on Windows Terminal on MYSYS2 and Git Bash

To get tmux on Windows Terminal follow the below steps:

1. Install [MYSYS2](https://www.msys2.org/)
2. Install [Git for Windows](https://gitforwindows.org/)
3. Install tmux on [MYSYS2](https://packages.msys2.org/package/tmux?repo=msys&variant=x86_64) `pacman -S tmux`
4. Install util-linux on [MYSYS2](https://packages.msys2.org/package/util-linux?repo=msys&variant=x86_64) `pacman -S util-linux`
5. Copy the following binaries from `C:\msys64\usr\bin` to `C:\Program Files\Git\usr\bin`
   * `tmux`
   * `msys-event_core-2-1-7.dll` - may change just try to start tmux on git bash and see what dependancy is missing.
   * `script.exe`
6. Then copy the following alias to your preffered file. I use `~/.bash_profile`
   ```.bash
   tmux() {
   # execute tmux with script
     TMUX="command tmux ${@}"
     SHELL=/usr/bin/bash script -qO /dev/null -c "eval $TMUX"
   }
   ```
7. Close all the windows then start them once more.
8. Use this [tmux.conf](https://github.com/iAmG-r00t/dotfiles/blob/master/server/tmux.conf) config.

## Credits / References

- [Where I got the `script` solution](https://github.com/microsoft/terminal/issues/5132#issuecomment-1820073875)  from Microsoft Terminal GitHub Issues
- [Where I stole the ALIAS](https://github.com/alacritty/alacritty/issues/1687#issuecomment-1119979280) from Alacritty GitHub Issues
