Many GUI (graphical user interface) applications on Linux have corresponding versions on Microsoft Windows, such as [chromium](https://chocolatey.org/packages/chromium), [digikam](https://chocolatey.org/packages/digikam), [firefox](https://chocolatey.org/packages/firefox), [thunderbird](https://chocolatey.org/packages/thunderbird), [gimp](https://chocolatey.org/packages/gimp), [goldendict](https://chocolatey.org/packages/goldendict), [gvim](https://chocolatey.org/packages/vim), [inkscape](https://chocolatey.org/packages/inkscape), [krita](https://chocolatey.org/packages/krita), [libreoffice](https://chocolatey.org/packages/libreoffice), [mupdf](https://chocolatey.org/packages/mupdf), [mpv](https://chocolatey.org/packages/mpv), [okular](https://chocolatey.org/packages/okular), [scribus](https://chocolatey.org/packages/scribus), [smplayer](https://chocolatey.org/packages/smplayer), [vlc](https://chocolatey.org/packages/vlc) and [zeal](https://chocolatey.org/packages/zeal).

This repository contributes small Shell scripts to start these (GUI) applications from the `WSL` (Windows Subsystem for Linux) shell just like from a Linux shell.
(Provided that their executables have been installed either into the path proposed during installation, or into a directory whose path is listed in the Microsoft Windows environment variable `%PATH%` --- which can be conveniently set by, say, [Rapidee](https://www.rapidee.com).)

# Installation

1. Clone this repository into `~/bin` by `mkdir -p ~/bin && cd ~/bin && git clone https://github.com/Konfekt/wsl-gui-bins`.
2. The paths of the shell scripts have to be added to the environment variable `$PATH` in WSL.
    For this, add to your file `~/.profile` (for Bash, or `~/.zshenv` for ZSH) the line

```sh
[ -z ${WSLENV+x} ] || export PATH="${PATH:+"$PATH:"}$HOME/bin/wsl-gui-bins"
```

# Hints

As an upshot, the environment variables `$BROWSER` and `$PDFVIEWER` can be defined as under Linux, for example,

```sh
    export BROWSER=firefox
    export PDFVIEWER=mupdf
```

----

If you use `ZSH`, then to start `Batch` (and `CMD`) files as under Windows, define a suffix alias

```sh
  wslbatch() { eval cmd.exe /c "$(wslpath -w "$1")"; }
  alias -s {cmd,bat}='wslbatch'
```

----

To imitate `xdg-open` (or `open` on `MacOS`), define aliases

```sh
  xdg_open() { eval cmd.exe /c /start /b "$(wslpath -w "$1")"; }
  alias xdg-open=xdg_open
  alias     open=xdg_open
```

This lets you, for example, open the current work dir in `Windows Explorer` by `o .`.

----

As an alternative to an image viewer such as `feh` or `sviv` on Linux, install [irfanview](https://chocolatey.org/packages/irfanview) and define an alias `iv` by

```sh
ProgramFiles="$(wslpath "$(cmd.exe /c "<nul set /p=%ProgramFiles%" 2>/dev/null)")"
if   [ -x "${ProgramFiles}/IrfanView"/i_view64.exe ]; then
  iv() { if [ $# -eq 1 ] && [ -e "$1" ]; then "${ProgramFiles}/IrfanView/i_view64.exe" "$(wslpath -w "$1")"; else "${ProgramFiles}/IrfanView/i_view64.exe" "$@"; fi; }
elif [ -x "${ProgramFiles} (x86)/IrfanView"/i_view32.exe ]; then
  iv() { if [ $# -eq 1 ] && [ -e "$1" ]; then "${ProgramFiles}/IrfanView/i_view32.exe" "$(wslpath -w "$1")"; else "${ProgramFiles}/IrfanView/i_view32.exe" "$@"; fi; }
fi
```

----

If you use `ZSH`, then to view an image file by entering its path on the command line, define a suffix alias

```sh
alias -s {tiff,raw,jpg,jpeg,png,gif,bmp}='iv'
```

----

To open the current work dir in [totalcommander](https://chocolatey.org/packages/totalcommander) by an alias, say `o.`,

```sh
  COMMANDER_PATH="$(wslpath "$(cmd.exe /c "<nul set /p=%COMMANDER_PATH%" 2>/dev/null)")"
  if [ -d "$COMMANDER_PATH" ] ; then
    if [ -f "$COMMANDER_PATH"/totalcmd64.exe ]; then
      totalcmd() { if [ $# -eq 1 ] && [ -e "$1" ]; then "$COMMANDER_PATH/totalcmd64.exe" "$(wslpath -w "$1")"; else "$COMMANDER_PATH/totalcmd64.exe" "$@"; fi; }
    elif [ -f "$COMMANDER_PATH"/totalcmd.exe ]; then
      totalcmd() { if [ $# -eq 1 ] && [ -e "$1" ]; then "$COMMANDER_PATH/totalcmd.exe" "$(wslpath -w "$1")"; else "$COMMANDER_PATH/totalcmd.exe" "$@"; fi; }
    fi
  fi
  alias totalcmd >/dev/null 2>&1 && alias o.='totalcmd /O /T /R="$(wslpath -w "$(pwd)")"'
```

