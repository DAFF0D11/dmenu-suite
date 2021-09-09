# Dmenu-Suite

dmenu-suite is a custom build of dmenu and collection of scripts utilizing its features.

### Install

Install dmenu-suite just as you would install dmenu from source.

``` bash
git clone https://gitlab.com/DAFF0D11/dmenu-suite.git
cd dmenu-suite
make install
```

**Scripts in the ``/script`` directory are not currently installed globally, you will have to add them to your path by hand**

### General Keybinds

Some default keybindings nothing been remapped.

`ctrl-j` select item below<br>
`ctrl-k` select item above<br>
`ctrl-p` select previous from history<br>
`ctrl-n` select next from history<br>
`ctrl-v` paste<br>

### Patches

Look to the ``/patches`` directory to view all patches used.<br>

The only patches not avaliable from [thing](https://tools.suckless.org/dmenu/patches/) are the 'expect' patches.<br>

#### Expect

The Expect patches are a simple port of the FZF `--expect` functionality, just limited to `ctrl-[a-z]` keys<br>
This allows you to supply your dmenu-suite scripts with ad-hoc keybinds to perform different actions on selections.

For example:

``` bash
ls | dmenu-suite -ex "ctrl-r"
```

You may now utilize the `ctrl-r` command to return your selected item(s) prefixed with the key used.<br>

This results in a string containing the key used followed by a tab character followed by the selected item.

``` bash 
ctrl-r    config.def.h
```

**Warning:** Passing ``-ex`` a key that is normally used by dmenu will override dmenu, and make it behave as *expected*

# Scripts

### Dmenu-chromium

Control your chromium browser through dmenu.

#### Features

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

**Dependencies:** `xclip`, `jq`, `sqlite3`, a chromium based browser (only brave-browser and chromium tested)<br>

**IMPORTANT:** You must start your browser with the remote debugging flag: `chromium --remote-debugging-port=9222`<br>

#### Keybindings

`ctrl-t` Show list of tabs (initial)<br>
`ctrl-h` Show list of history<br>
`ctrl-m` Show list of bookmarks<br>
`ctrl-d` close selected tab (or multiple)<br>
`ctrl-p` Previous search query<br>
`ctrl-n` Next search query<br>
`ctrl-y` Copy url of selection to clipboard<br>
`ctrl-enter` Select multiple <br>
`shift-enter` Search keywords<br>

#### Search

Just start typing keywords and hit `shift+enter` to search in the default engine.<br>
Techinally you dont need to hold shift as long as nothing is selected.<br>

##### Search Engines

You can also specify search engines by prefixing your search with one of the included engines.<br>

`dd` Duckduckgo<br>
`ddl` Duckduckgo lite<br>
`ddi` Duckduckgo images<br>
`gg` Google<br>
`ggi` Google image<br>
`nx` Nix package manager packages<br>
`wfa` Wolframalpha<br>
`rd` Reddit<br>
`yt` Youtube<br>
`az` Amazon<br>
`eb` Ebay<br>
`wd` Merriam Webster<br>

To use a search engine prefix, you should format your search with a prefix followed by a space followed by your keywords.

For example: `ddi puppies in flowers`

To search for a URL or local file, prefix your query with two forward slashes followed by a space.

`// https://suckless.org/`

You can also prefix the prefix with `i` to perform a search incognito.

`idd teaching crabs how to read`
    
#### **Warning**
**Running your browser with the remote debugging flag could open up security vulnerabilities.**<br>
**Read more about the remote debugging protocol and its security implications here: https://chromedevtools.github.io/devtools-protocol/**<br>
