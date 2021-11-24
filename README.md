Many GUI (graphical user interface) applications on Linux have corresponding versions on Microsoft Windows, such as [chromium](https://chocolatey.org/packages/chromium), [digikam](https://chocolatey.org/packages/digikam), [firefox](https://chocolatey.org/packages/firefox), [thunderbird](https://chocolatey.org/packages/thunderbird), [gimp](https://chocolatey.org/packages/gimp), [goldendict](https://chocolatey.org/packages/goldendict), [gvim](https://chocolatey.org/packages/vim), [inkscape](https://chocolatey.org/packages/inkscape), [krita](https://chocolatey.org/packages/krita), [libreoffice](https://chocolatey.org/packages/libreoffice), [mupdf](https://chocolatey.org/packages/mupdf), [mpv](https://chocolatey.org/packages/mpv), [okular](https://chocolatey.org/packages/okular), [scribus](https://chocolatey.org/packages/scribus), [smplayer](https://chocolatey.org/packages/smplayer), [vlc](https://chocolatey.org/packages/vlc) and [zeal](https://chocolatey.org/packages/zeal).

This repository contributes small Shell scripts to start these (GUI) applications from the [Git BASH](https://gitforwindows.org/) or [WSL](https://docs.microsoft.com/en-us/windows/wsl/install) (Windows Subsystem for Linux) shell just like from a Linux shell.
(Provided that their executables have been installed either into the path proposed during installation, or into a directory whose path is listed in the Microsoft Windows environment variable `%PATH%` --- conveniently set by, say, [Rapidee](https://www.rapidee.com).)

# Installation

1. Clone this repository into `~/bin` by `mkdir -p ~/bin && cd ~/bin && git clone https://github.com/Konfekt/win-bash-gui-bins`.
2. The paths of the shell scripts have to be added to the environment variable `$PATH`.
    For this, add to your `~/.profile` (for Bash, or `~/.zshenv` for ZSH) the following line for Git BASH

```sh
[ -z ${MSYSTEM+x} ] || export PATH="${PATH:+"$PATH:"}$HOME/bin/win-bash-gui-bins"
```

respectively, for WSL,

```sh
[ -z ${WSLENV+x} ] || export PATH="${PATH:+"$PATH:"}$HOME/bin/win-bash-gui-bins"
```

# Hints

As an upshot, the environment variables `$BROWSER` and `$PDFVIEWER` can be defined as under Linux, for example,

```sh
    export BROWSER=firefox
    export PDFVIEWER=mupdf
```

To imitate `xdg-open` (or `open` on `MacOS`), define aliases

```sh
  xdg_open() { eval cmd.exe /c /start /b "$(winpath -aw "$1")"; }
  alias xdg-open=xdg_open
  alias     open=xdg_open
```

This lets you, for example, open the current work dir in `Windows Explorer` by `o .`.

## Batch files under WSL

If you use `ZSH` under `WSL`, then to start `Batch` (and `CMD`) files as under Windows, define a suffix alias

```sh
  wslbatch() { eval cmd.exe /c "$(wslpath -aw "$1")"; }
  alias -s {cmd,bat}='wslbatch'
```

## File manager

As a file manager, install [totalcommander](https://chocolatey.org/packages/totalcommander) to start it by `totalcmd`.
To open the current work dir in by an alias, say `o.` :

```sh
  command -v totalcmd >/dev/null 2>&1 && alias o.='totalcmd /O /T /R="$(winpath -aw "$(pwd)")"'
```

## Image Viewer

As an alternative to an image viewer such as `feh` or `sviv` on Linux, install [irfanview](https://chocolatey.org/packages/irfanview) to start it by `iv`.
If you use `ZSH`, then to view an image file by entering its path on the command line, define a suffix alias :

```sh
command -v iv  >/dev/null 2>&1 && alias -s {tiff,raw,jpg,jpeg,png,gif,bmp}='iv'
```

