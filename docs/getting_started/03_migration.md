---
title: '♻️ Migration'
image: img/logo/320x320.png
description: Migration guide
keywords:
  - migration
  - setup
---

## Oh-My-Zsh

### OMZ Shorthand Syntax

```shell showLineNumbers
zi snippet <URL>        # Raw Syntax with URL
zi snippet OMZ::<PATH>  # Shorthand OMZ/          (http://github.com/ohmyzsh/ohmyzsh/raw/master/)
zi snippet OMZL::<PATH> # Shorthand OMZ/lib/      (http://github.com/ohmyzsh/ohmyzsh/raw/master/lib)
zi snippet OMZT::<PATH> # Shorthand OMZ/themes/   (http://github.com/ohmyzsh/ohmyzsh/raw/master/themes)
zi snippet OMZP::<PATH> # Shorthand OMZ/plugins/  (http://github.com/ohmyzsh/ohmyzsh/raw/master/plugins)
```

### OMZ Library

Importing the [clipboard][1] and [termsupport][2] from the OMZ library sample:

Raw Syntax:

```shell showLineNumbers
zi snippet https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/clipboard.zsh
zi snippet https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/termsupport.zsh
```

OMZ Shorthand Syntax:

```shell showLineNumbers
zi snippet OMZ::lib/clipboard.zsh
zi snippet OMZ::lib/termsupport.zsh
```

OMZL Shorthand Syntax:

```shell showLineNumbers
zi snippet OMZL::clipboard.zsh
zi snippet OMZL::termsupport.zsh
```

### OMZ Plugins

```diff showLineNumbers
- plugins=(
-  git
-  dotenv
-  rake
-  rbenv
-  ruby
-)

+ zi snippet OMZP::git
+ zi snippet OMZP::dotenv
+ zi snippet OMZP::rake
+ zi snippet OMZP::rbenv
+ zi snippet OMZP::ruby
```

:::tip

Bundle multiple commands to a single file:

`zi snippet <some/path/or/url/bundled-snippets.zsh`

:::

### Plugins

```diff showLineNumbers
- plugins=(
-  git
-  dotenv
-  rake
-  rbenv
-  ruby
-)

+ zi snippet OMZP::git
+ zi snippet OMZP::dotenv
+ zi snippet OMZP::rake
+ zi snippet OMZP::rbenv
+ zi snippet OMZP::ruby
```

If it consists of a single file, with ZI you can just load it: `zi snippet <some/path/or/url/undled-snnippets.zsh`

Use `zi ice svn` if multiple files require an entire subdirectory.

- [gitfast][4]
- [osx][5]
- [history-substring-search][18]

```shell showLineNumbers
zi ice svn
zi snippet OMZP::gitfast

zi ice svn
zi snippet OMZP::osx

zi ice svn
zi snippet OMZP::history-substring-search
```

Use `zi ice as"completion"` to directly add single file completion snippets.

- [docker][6]
- [fd][7]
- [ag][19]

```shell showLineNumbers
zi ice as"completion"
zi snippet OMZP::docker/_docker

zi ice as"completion"
zi snippet OMZP::fd/_fd

zi ice as"completion"
zi snippet OMZP::ag/_ag
```

[You can see an extended explanation of OMZ setup in the Wiki][8]

### OMZ Themes

Themes are stored in the `themes` directory. All and loaded in the background. with the simple syntax:

```shell
ZSH_THEME="robbyrussell"
```

However, ZI doesn't support the `ZSH_THEME` variable natively.

To use **themes** created for OMZ, it requires loading shown below as it would be the same as OMZ does in the
background.

> Some themes may require additional configuration it can be determined from the theme configuration file.

- Load `Git` library
- Load `Git` plugin
- Load library dependencies
- Enable `setopt promptsubst`

If any of the above are not in order or missing, the theme will break similar as shown below:

```shell
… $(build_prompt) …
```

If the `Git` library is not loaded or loaded in the wrong order, then it may appear similar to the following:

```shell showLineNumbers
........:1: command not found: git_prompt_status
........:1: command not found: git_prompt_short_sha
```

If you encounter any issue with the theme, OMZ support libraries are to be loaded

- If your theme isn't colored when it should, you will want to load `theme-and-appearance.zsh`

- If you encounter an error message similar to:

```shell showLineNumbers
zsh: command not found: ruby_prompt_info
```

You need to load `prompt_info_functions.zsh`

All together it looks like this:

```shell showLineNumbers
zi snippet OMZL::git.zsh
zi snippet OMZP::git
zi snippet OMZL::theme-and-appearance.zsh
zi snippet OMZL::prompt_info_functions.zsh
# Other libraries that might be needed
zi cdclear -q
```

Then load the prompt:

```shell showLineNumbers
setopt promptsubst
zi snippet OMZT::robbyrussell
```

### External theme sample: [NicoSantangelo/Alpharized][3]

Load with OMZ:

```shell
ZSH_THEME="alpharized"
```

Load with ZI:

```shell
zi snippet OMZL::git.zsh
```

Load `Git` plugin from OMZ:

```shell showLineNumbers
zi snippet OMZP::git
zi cdclear -q

setopt promptsubst

zi light NicoSantangelo/Alpharized
```

## Prezto basics

Raw Syntax with URL:

```shell
zi snippet <URL>
```

Shorthand PZT: <https://github.com/sorin-ionescu/prezto/tree/master/>

```shell
zi snippet PZT::<PATH>
```

Shorthand PZT/modules:

```shell
zi snippet PZTM::<PATH>
```

### Prezto modules

Importing the [environment][9] and [terminal][10] Prezto Modules Sample:

Prezto Setting:

```shell showLineNumbers
zstyle ':prezto:load' pmodule 'environment' 'terminal'
```

ZI Setting:

> Import raw syntax from URL.

```shell showLineNumbers
zi snippet https://github.com/sorin-ionescu/prezto/blob/master/modules/environment/init.zsh
zi snippet https://github.com/sorin-ionescu/prezto/blob/master/modules/terminal/init.zsh
```

PZT Shorthand Syntax:

```shell showLineNumbers
zi snippet PZT::modules/environment
zi snippet PZT::modules/terminal
```

PZTM Shorthand Syntax:

```shell showLineNumbers
zi snippet PZTM::environment
zi snippet PZTM::terminal
```

Use `zi ice svn` if multiple files require an entire subdirectory.

- [docker][11]
- [git][12]

```shell showLineNumbers
zi ice svn
zi snippet PZTM::docker

zi ice svn
zi snippet PZTM::git
```

Use `zi ice as"null"` if don't exist `*.plugin.zsh`, `init.zsh`, `*.zsh-theme*` files in module.

- [archive][13]:

```shell showLineNumbers
zi ice svn as"null"
zi snippet PZTM::archive
```

Use `zi ice atclone"git clone <repo> <location>"` if module have external module.

- [completion][14]:

```shell showLineNumbers
zi ice svn blockf \
  atclone"git clone --recursive https://github.com/zsh-users/zsh-completions.git external"
zi snippet PZTM::completion
```

Use `blockf` to prevent any unnecessary additions to fpath, as ZI manages fpath.

:::tip

What is `zstyle`?

- Official (zsh.sourceforge.net): [zstyle][15]
- StackExchange: [What does `zstyle` do?][16]

:::

## Zgen

### Load OMZ library

```diff showLineNumbers
- zgen oh-my-zsh

+ zi snippet OMZL::<ANY OF THEM>
```

### Load OMZ plugins

```diff showLineNumbers
- zgen oh-my-zsh <PATH>

+ zi snippet OMZP::<PATH>
```

### Load Prezto modules

```diff showLineNumbers
- zgen prezto

+ zi snippet PZTM::<ANY FROM LIST BELOW>
```

- environment
- terminal
- editor
- history
- directory
- spectrum
- utility
- completion
- prompt

```diff showLineNumbers
- zgen prezto <modulename>

+ zi snippet PZTM::<modulename>
```

Load repositories as prezto plugins:

```diff showLineNumbers
- zgen pmodule <reponame> <branch>

+ zi ice ver"<branch>"
+ zi load <repo/plugin>
```

### Summarized Zgen

:::info

For the `location`: refer [selection of files][17]

:::

```diff showLineNumbers
- zgen load <repo> [location] [branch]

+ zi ice ver"[branch]"
+ zi load <repo>
```

## Zplug Basics

```diff showLineNumbers
- zplug <repo/plugin>, tag1:<option1>, tag2:<option2>

+ zi ice tag1"<option1>" tag2"<option2>"
+ zi load <repo/plugin>
```

### Tag comparison

- `as` => `as`
- `use` => `pick`, `src`, `multisrc`
- `ignore` => None
- `from` => `from`
- `at` => `ver`
- `rename-to` => `mv`, `cp`
- `dir` => Selection(`pick`, …) with rename
- `if` => `if`
- `hook-build` => `atclone`, `atpull`
- `hook-load` => `atload`
- `frozen` => None
- `on` => None
- `defer` => `wait`
- `lazy` => `autoload`
- `depth` => `depth`

[1]: https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/clipboard.zsh
[2]: https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/termsupport.zsh
[3]: https://github.com/nicosantangelo/Alpharized
[4]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/gitfast
[5]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/osx
[6]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/docker
[7]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/fd
[8]: /docs/guides/customization#oh-my-zsh
[9]: https://github.com/sorin-ionescu/prezto/tree/master/modules/environment
[10]: https://github.com/sorin-ionescu/prezto/tree/master/modules/terminal
[11]: https://github.com/sorin-ionescu/prezto/tree/master/modules/docker
[12]: https://github.com/sorin-ionescu/prezto/tree/master/modules/git
[13]: https://github.com/sorin-ionescu/prezto/tree/master/modules/archive
[14]: https://github.com/sorin-ionescu/prezto/tree/master/modules/completion
[15]: http://zsh.sourceforge.net/Doc/Release/Zsh-Modules.html#The-zsh_002fzutil-Module
[16]: https://unix.stackexchange.com/questions/214657/what-does-zstyle-do
[17]: /docs/guides/syntax/ice#src-pick-multisrc
[18]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/history-substring-search
[19]: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/ag
