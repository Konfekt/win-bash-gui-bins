Many GUI (graphical user interface) applications on Linux have corresponding versions on Microsoft Windows, such as [chromium](https://chocolatey.org/packages/chromium), [digikam](https://chocolatey.org/packages/digikam), [firefox](https://chocolatey.org/packages/firefox), [gimp](https://chocolatey.org/packages/gimp), [goldendict](https://chocolatey.org/packages/goldendict), [gvim](https://chocolatey.org/packages/vim), [inkscape](https://chocolatey.org/packages/inkscape), [krita](https://chocolatey.org/packages/krita), [libreoffice](https://chocolatey.org/packages/libreoffice), [imagemagick](https://chocolatey.org/packages/imagemagick), [mupdf](https://chocolatey.org/packages/mupdf), [okular](https://chocolatey.org/packages/okular), [scribus](https://chocolatey.org/packages/scribus), [vlc](https://chocolatey.org/packages/vlc) and [zeal](https://chocolatey.org/packages/zeal).

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
    _wslbatch() { eval cmd.exe /c "$(wslpath -w "$1")"; }
    alias -s {cmd,bat}='_wslbatch'
```

----

As an alternative to `xdg-open` (or `open` on `MacOS`), define an alias

```sh
    alias o='cmd.exe /c start /b'
```

This lets you, for example, open the current work dir in `Windows Explorer` by `o .`.

----

As an alternative to an image viewer such as `feh` or `sviv` on Linux, install [irfanview](https://chocolatey.org/packages/irfanview) and define an alias `iv` by

```sh
ProgramFiles="$(wslpath "$(cmd.exe /c "<nul set /p=%ProgramFiles%" 2>/dev/null)")"
if command -v i_view64.exe >/dev/null 2>&1; then
  alias iv='i_view64.exe'
elif command -v "${ProgramFiles}/IrfanView"/i_view64.exe >/dev/null 2>&1; then
  alias iv="${ProgramFiles}/IrfanView/i_view64.exe"
elif command -v "${ProgramFiles} (x86)/IrfanView"/i_view32.exe >/dev/null 2>&1; then
  alias iv="${ProgramFiles} (x86)/IrfanView/i_view32.exe"
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
      alias totalcmd="$COMMANDER_PATH/totalcmd64.exe"
    elif [ -f "$COMMANDER_PATH"/totalcmd.exe ]; then
      alias totalcmd="$COMMANDER_PATH/totalcmd.exe"
    fi
  elif command -v totalcmd64.exe >/dev/null 2>&1 ; then
    alias totalcmd="totalcmd64.exe"
  elif command -v totalcmd.exe >/dev/null 2>&1 ; then
    alias totalcmd="totalcmd.exe"
  fi
  alias totalcmd >/dev/null 2>&1 && alias o.='totalcmd /O /T /R="$(wslpath -w "$(pwd)")"'
```

# Related

The repository [xclip-xsel-WSL](https://github.com/Konfekt/xclip-xsel-WSL) contributes a small Shell script to make `xclip` and `xsel` in `WSL` (Windows Subsystem for Linux) read and write on the Microsoft Windows clipboard instead of the Linux clipboard.

