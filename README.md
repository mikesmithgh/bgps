# 📍 git-prompt-string

git-prompt-string is a shell agnostic git prompt written in Go. git-prompt-string provides
information about the current git branch and is inspired by
[git-prompt.sh](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh).

[![go](https://img.shields.io/static/v1?style=flat-square&label=&message=v1.22.0&logo=go&labelColor=282828&logoColor=9dbad4&color=9dbad4)](https://go.dev/)
[![semantic-release: angular](https://img.shields.io/static/v1?style=flat-square&label=semantic-release&message=angular&logo=semantic-release&labelColor=282828&logoColor=d8869b&color=8f3f71)](https://github.com/semantic-release/semantic-release)

> [!WARNING]\
> 03/25/2024: git-prompt-string (previously bgps) is actively undergoing a major rewrite 
>
> This is a breaking change that will simplify and improve maintainability of git-prompt-string
>
> If you prefer to keep using legacy bgps, then use the tag [v0.0.1](https://github.com/mikesmithgh/git-prompt-string/tree/v0.0.1)

## 📦 Installation

### homebrew tap
```sh
brew install mikesmithgh/homebrew-git-prompt-string/git-prompt-string
```

### go install

```sh
go install github.com/mikesmithgh/git-prompt-string@latest 
```

## 🛠️ Setup

### Prompt configuration

Add git-prompt-string to your prompt. For example,

#### bash
```sh
PROMPT_COMMAND='PS1="\[\n \e[0;33m\w\e[0m$(git-prompt-string)\n \e[0;32m\u@local \e[0;36m\$\e[0m \]"'
```

### git-prompt-string configuration

#### Nerd Font

By default, the powerline icon `` is used as a prefix in the prompt. It is recommended to use a [Nerd Font](https://www.nerdfonts.com/)
to properly display the `` (nf-pl-branch) icon. See https://www.nerdfonts.com/ to download a Nerd Font. If you
do not want this symbol, replace the prompt prefix with " ". For example, add the following to you git-prompt-string
configuration.

```toml
prompt_prefix = ' '
```

#### Configuration file

git-prompt-string will first check if the `--config` option was passed as an argument. If 
`--config` is set, the filepath defined in the value will be used as the configuration
file.

If `--config` is not set, then git-prompt-string will check if the environment variable 
`$GIT_PROMPT_STRING_CONFIG` is set. If `$GIT_PROMPT_STRING_CONFIG` is set, the 
filepath defined in the value will be used as the configuration file.

If `$GIT_PROMPT_STRING_CONFIG` is not set, then git-prompt-string will check if the environment
variable `$XDG_CONFIG_HOME` is set. If `$XDG_CONFIG_HOME` is set, the directory defined in then
value will be used as the base directory for git-prompt-string configurations. See 
[XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)
for more information on XDG environment variables.

If `$XDG_CONFIG_HOME` is not set, then `~/.config` and `~/AppData/Local` will be used as the
base directory for Unix and Windows, respectively.

The file defined at `git-prompt-string/config.toml` in the base directory will be used to configure
git-prompt-string.

| OS      | Base directory  | Configuration file                            |
| :------ | :-------------- | :-------------------------------------------- |
| Unix    | ~/.config       | ~/.config/git-prompt-string/config.toml       |
| Windows | ~/AppData/Local | ~/AppData/Local/git-prompt-string/config.toml |

If the configuration filepath is set to the special value of `NONE`, then all user configurations will
be ignored. For example, `git-prompt-string --config=NONE` or `GIT_PROMPT_STRING_CONFIG=NONE git-prompt-string`
will use the default configuration values defined by git-prompt-string.

#### Configuration options

The following configuration options are available in either as a command-line argument or TOML key.

```text
--ahead-format or ahead_format
      The format used to indicate the number of commits ahead of the
      remote branch. The %v verb represents the number of commits
      ahead. One %v verb is required. (default "↑[%v]")

--behind-format or behind_format
      The format used to indicate the number of commits behind the
      remote branch. The %v verb represents the number of commits
      behind. One %v verb is required. (default "↓[%v]")

--color-clean or color_clean
      The color of the prompt when the working directory is clean.
      (default "green")

--color-delta or color_delta
      The color of the prompt when the local branch is ahead, behind,
      or has diverged from the remote branch. (default "yellow")

--color-dirty or color_dirty
      The color of the prompt when the working directory has changes
      that have not yet been committed. (default "red")

--color-disabled or color_disabled
      Disable all colors in the color-disabled

--color-merging or color_merging
      The color of the prompt during a merge, rebase, cherry-pick,
      revert, or bisect. (default "blue")

--color-no-upstream or color_no_upstream
      The color of the prompt when there is no remote upstream branch.
      (default "bright-black")

--color-untracked or color_untracked
      The color of the prompt when there are untracked files in the
      working directory. (default "magenta")

--diverged-format or diverged_format
      The format used to indicate the number of commits diverged
      from the remote branch. The first %v verb represents the number
      of commits ahead of the remote branch. The second %v verb
      represents the number of commits behind the remote branch. Two
      %v verbs are required. (default "↕ ↑[%v] ↓[%v]")

--no-upstream-remote-format or no_upstream_remote_format
      The format used to indicate when there is no remote upstream,
      but there is still a remote branch configured. The first %v
      represents the remote repository. The second %v represents the
      remote branch. Two %v are required. (default " → %v/%v")

--prompt-prefix or prompt_prefix
      A prefix that is added to the beginning of the prompt. The
      powerline icon  is used be default. It is recommended to
      use a Nerd Font to properly display the  (nf-pl-branch) icon.
      See https://www.nerdfonts.com/ to download a Nerd Font. If you
      do not want this symbol, replace the prompt prefix with " ".
      \ue0a0 is the unicode representation of . (default " \ue0a0 ")

--prompt-suffix or prompt_suffix
      A suffix that is added to the end of the prompt.
```

#### Specifying colors

A color value in the configuration must be either a single color or multiple colors
separated by white space.

Valid formats for a color are:
- `color`
- fg:`color`
- bg:`color`
- #ffffff
- #fg:ffffff
- #bg:ffffff
- `reset`

The value `reset` will clear all text formatting and reset the color to the default value.
Colors starting with `bg` or `#bg` are background colors. All other formats are considered
foreground colors. i.e., `red` is equivalent to `fg:reg`.

Colors starting with `#` are considered a hex color code and must have 6 digits.

Valid colors are defined in the following table.

| Color          | Code |
| :------------- | :--  |
| black          | 0    |
| red            | 1    |
| green          | 2    |
| yellow         | 3    |
| blue           | 4    |
| magenta        | 5    |
| cyan           | 6    |
| white          | 7    |
| bright-black   | 8    |
| bright-red     | 9    |
| bright-green   | 10   |
| bright-yellow  | 11   |
| bright-blue    | 12   |
| bright-magenta | 13   |
| bright-cyan    | 14   |
| bright-white   | 15   |

The following are examples of valid color configurations:

```toml
color_clean='#e5ee04'
color_no_upstream="reset fg:black bg:white"
color_dirty="bg:#b30559"
color_delta="fg:#fcb728"
color_untracked="fg:#ff0000 bg:#16f2aa"
color_merging="bg:#ccccff magenta"
```

#### Default configuration

```toml
prompt_prefix = '  '
prompt_suffix = ''
ahead_format = '↑[%v]'
behind_format = '↓[%v]'
diverged_format = '↕ ↑[%v] ↓[%v]'
no_upstream_remote_format = ' → %v/%v'
color_disabled = false
color_clean = 'green'
color_delta = 'yellow'
color_dirty = 'red'
color_untracked = 'magenta'
color_no_upstream = 'bright-black'
color_merging = 'blue'
```

## 📌 Alternatives
- [git-prompt.sh](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) - bash/zsh git prompt support
- [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt) - An informative and fancy bash prompt for Git users
- [zsh-git-prompt](https://github.com/olivierverdier/zsh-git-prompt) - Informative git prompt for zsh
- [starship](https://starship.rs/) - The minimal, blazing-fast, and infinitely customizable prompt for any shell!

