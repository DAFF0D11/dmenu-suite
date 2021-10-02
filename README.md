# Dmenu-Suite

Dmenu-suite is a custom build of dmenu-5.0 and collection of scripts utilizing its features.

### Installation

Install dmenu-suite just as you would install dmenu from source.

``` sh
git clone https://gitlab.com/DAFF0D11/dmenu-suite.git
cd dmenu-suite
make install
```

***Scripts in the `/scripts` directory are not currently installed globally, you will have to add them to your path manually.***

### General Keybinds

Some default dmenu keybindings have been remapped.

`ctrl-j` select item below<br>
`ctrl-k` select item above<br>
`ctrl-p` select previous history<br>
`ctrl-n` select next history<br>
`ctrl-v` paste<br>

### Patches

- dmenu-center-20200111-8cd37e1.diff
- dmenu-fuzzymatch-4.9.diff
- dmenu-instant-4.7.diff
- dmenu-mousesupporthoverbgcol-5.0.diff
- dmenu-navhistory-5.0.diff
- dmenu-preselect-20200513-db6093f.diff
- dmenu-rejectnomatch-4.7.diff
- dmenu-scroll-20180607-a314412.diff
- dmenu-tsv-20201101-1a13d04.diff
- expect.diff
- expect-multi-selection.diff

The only patches ***not*** available from [suckless.org](https://tools.suckless.org/dmenu/patches/) are the 'expect' patches.<br>

### [Expect](https://github.com/DAFF0D11/dmenu-suite/blob/master/patches/expect.diff)

The Expect patches are a port of the FZF `--expect` functionality limited to `ctrl-[a-z]` keys.<br>
This allows you to supply your dmenu-suite scripts with ad hoc keybinds to perform different actions on selections.

For example:

``` sh
ls | dmenu-suite -ex "ctrl-r"
```

You can now utilize the `ctrl-r` command to return your selected item(s) prefixed with the key used.<br>
This results in a string containing the key used followed by a tab character followed by the selected item.

``` sh 
ctrl-r    config.def.h
```
This allows you to easily handle different expected keys with just a case statement.

``` sh
#!/bin/sh

DMENU_CHOICE=$(ls | dmenu-suite -ex 'ctrl-r,ctrl-t,ctrl-y' -c -l 10)

case "$DMENU_CHOICE" in
    ctrl-r*) echo "$DMENU_CHOICE";;
    ctrl-t*) echo "$DMENU_CHOICE";;
    ctrl-y*) echo "$DMENU_CHOICE";;
          *) echo "$DMENU_CHOICE";;
esac
```
**Warning: passing ``-ex`` a key that is normally used by dmenu will override dmenu, and make it behave as *expected***

# Scripts

## [Dmenu-chromium](https://github.com/DAFF0D11/dmenu-suite/blob/master/scripts/dmenu-chromium)

Control your chromium browser through dmenu.<br>

**Dependencies:** `xclip`, `jq`, `sqlite3`, a chromium based browser (only brave-browser and chromium tested)<br>

### Features

- Visit tabs
- Close tabs
- Bookmarks
- History
- Search for keywords
- Search with search engine
- Search Incognito
- Search URL
- Copy URL
- Open Local Files

### Setup 

You must start your browser with the debugging flag: `chromium --remote-debugging-port=9222`

### Keybindings

`ctrl-t` Show list of tabs (initial)<br>
`ctrl-h` Show list of history<br>
`ctrl-m` Show list of bookmarks<br>
`ctrl-d` close selected tab (or multiple)<br>
`ctrl-p` Previous search query<br>
`ctrl-n` Next search query<br>
`ctrl-y` Copy url of selection to clipboard<br>
`ctrl-enter` Select multiple <br>
`shift-enter` Search keywords<br>

### Search

Just start typing keywords and hit `shift+enter` to search in the default engine.<br>
Technically you don't need to hold shift as long as no items are selected in dmenu.<br>

#### Search Engines

You can also specify search engines by prefixing your search with one of the included engines.<br>

`dd`  Duckduckgo<br>
`ddl` Duckduckgo lite<br>
`ddi` Duckduckgo images<br>
`gg`  Google<br>
`ggi` Google image<br>
`nx`  Nix package manager packages<br>
`wfa` Wolframalpha<br>
`rd`  Reddit<br>
`yt`  Youtube<br>
`az`  Amazon<br>
`eb`  Ebay<br>
`wd`  Merriam Webster<br>

To use a search engine prefix, you should format your search with a prefix followed by a space followed by your keywords.<br>
`ddi puppies in flowers`

To search for a URL or local file.<br>
`// https://suckless.org`

You can also prefix any prefix with `i` to perform a search incognito.<br>
`idd teaching crabs how to read`
    
### **Warning**
**Running your browser with the remote debugging flag could open up security vulnerabilities.**<br>
**Read more about the remote debugging protocol and its security implications here: https://chromedevtools.github.io/devtools-protocol/**<br>

## [Dmenu-mpv-music-shuffler](https://github.com/DAFF0D11/dmenu-suite/blob/master/scripts/dmenu-mpv-music-shuffler)

A simple music shuffler using MPV back end.

Most music players are far too complicated for my needs.<br>
I like to listen to a single directory of songs on shuffle and choose a 'set' of them to play next.<br>

Its highly recommended to set up [playerctl](https://github.com/altdesktop/playerctl) with [mpv-mpris](https://github.com/hoyon/mpv-mpris) to control the mpv instance.

**Dependencies:** `mpv`<br>
**Recommended:** `mpv-mpris` `playerctl`

### Features
- Choose directory of music
- Play all songs in directory on shuffle
- Choose one ore more songs to play next
- Fuzzy search songs in directory

### Setup
- Change $MUSIC to point at your music directory<br>
- Start the mpv instance `dmenu-mpv-music-shuffler -i`<br>

### Keybinds
`ctrl-l` List playlist<br>
`ctrl-h` Choose song(s)<br>
`ctrl-enter` Multi select songs to play next<br>

#### Inspired by and borrowed from: [slakkenhuis/scripts/dmenu-mpv](https://github.com/slakkenhuis/scripts/blob/master/dmenu-mpv)

## [Dmenu-tmux](https://github.com/DAFF0D11/dmenu-suite/blob/master/scripts/dmenu-tmux)

Control tmux with dmenu.

**Dependencies:** `tmux`<br>

**Recommended:** `wmctrl`<br>

### Features

- Switch to (pane,window,session)
- Close (panes,windows,sessions)
- Swap (panes,windows)
- Grab (panes,windows)

### Setup

Using `wmctrl` to focus a Tmux window automatically on switch, you need to set its title in your .tmux.conf, and in the script.

In .tmux.conf
```
set-option -g set-titles-string 'TMUX' 
```

In dmenu-suite/scripts/dmenu-tmux.
```
TMUX_TITLE="TMUX"
```

### Keybinds

`ctrl-p` List all panes(initial)<br>
`ctrl-w` List all windows <br>
`ctrl-s` List all sessions<br>
`ctrl-g` Grab panes/windows<br>
`ctrl-x` Swap pane/window<br>
`ctrl-d` Close panes/windows/sessions<br>

## [Dmenu-emacs](https://github.com/DAFF0D11/dmenu-suite/blob/master/scripts/dmenu-emacs)

Control Emacs with dmenu.

**Dependencies:** `emacs`<br>
**Recommended:** `wmctrl`<br>

### Features

- Switch to buffer
- close buffer(s)
- View buffers
- View file buffers
- View hidden buffers
- View log buffers
- View magit buffers

### Setup

Emacs must be run in daemon mode with a name.

`emacs --daemon=MAIN`

Connect to the Emacs server with your prefered client

Launch GUI Emacs `emacsclient -c --socket-name=MAIN`<br>
Terminal emacs `emacsclient -nw --socket-name=MAIN`<br>

To automatically switch to your Emacs window on selection, you must set its title in the script, as well as your Emacs configuration.<br>
`EMACS_TITLE="EMACS"`<br>
`(setq-default frame-title-format "EMACS")`<br>

### Keybinds

`ctrl-a` show all buffers (initial)<br>
`ctrl-f` show file buffers<br>
`ctrl-g` show magit buffers<br>
`ctrl-l` show log buffers<br>
`ctrl-o` show hidden buffers<br>
`ctrl-x` close buffer(s)<br>
`enter` switch to buffer<br>

#### **Warning**

I do not know Emacs or Elisp very well.

The naming/grouping of buffers may be incorrect, and the Elisp may be unreliable.

## [Dmenu-wmctrl](https://github.com/DAFF0D11/dmenu-suite/blob/master/scripts/dmenu-wmctrl)

Control desktop windows with dmenu.

**Dependencies:** `wmctrl`<br>

### Features

- Switch to windows
- Grab windows from other workspaces and bring them to your current workspace
- Close windows

### Keybinds 

`enter` Switch to window<br>
`ctrl-g` Grab window(s)<br>
`ctrl-x` Close window(s)<br>


#### **Warning**
If you are using DWM, be aware that the 'grab' functionality will not work due to the way DWM handles workspaces by default.

## [Dmenu-which](https://github.com/DAFF0D11/dmenu-suite/blob/master/scripts/dmenu-which)

A crude replica of the Emacs which-key package.

Unlike the emacs package, this script must be crafted by __you__ to define the commands in the list.
Some commands have been provided as examples to help you craft your own 'which-key' menus.

_Some sample commands rely on $TERMINAL and $BROWSER variables_

### Features

- Nest many commands behind a single keybind
- Incremental command menus
- Choose commands through (semantic) key-chords

#### **Warning**
When creating your own menus, you must use uppercase letters for the labels, and lowercase for the trigger keys(or vice versa).
This is because we are abusing dmenu's case sensitive nature to label and trigger keys.
