---
layout: post
title: "The Ultimate Terminal Setup for macOS: Tips, Tricks, and Customizations"
tags: macOS terminal iTerm 1Password
---

## Intro

The terminal, also known as the _command line interface_ (CLI), is a powerful tool that can greatly enhance our experience on macOS. One of the main advantages of using the terminal is that it gives us access to a wide range of powerful command-line tools and utilities. Additionally, we can increase our efficiency and speed of navigation and management of files and directories. The terminal also allows for scripting and automation of tasks through the use of shell scripts and gives us access to advanced features and settings that may not be available through the _graphical user interface_ (GUI). By mastering the terminal, we can greatly increase our productivity and efficiency as power users.

In this article, I will guide you through customizing and optimizing your terminal experience on macOS. The goal is that by the end you should have an advanced terminal configuration at your disposal.

![iTerm2 end result](/assets/images/terminal-setup-on-macos/iterm_terminal_example.png)

_I am not affiliated with any of the apps or programs that I may mention or recommend. Any views or opinions expressed are solely my own and do not represent the views or opinions of any other organization or entity._

## Steps

1. `Homebrew`
2. `iTerm2`
3. `Oh My Zsh`
4. `rbenv` and `pyenv`
5. `powerline-shell`
6. Enable Touch ID for `sudo`
7. `1Password CLI`

## Homebrew

Make sure you have [`Homebrew`](https://brew.sh/) installed. We will use it to install tools and applications.

## iTerm2

First, let's install [**`iTerm2`**](https://iterm2.com/index.html).

```sh
brew install --cask iterm2
```

Configuring the basic behaviours of iTerm2 is pretty straightforward as it already has great presets. If you want to customize it further I recommend starting with the [official documentation](https://iterm2.com/documentation.html). Usually, I only configure the following:

### Changing the colours

Colours are always a question of personal preference. My favourite one so far is the **Solarized Dark** as it's easy on my eyes. If you don't like dark colours consider using **Solarized Light**. Experiment with the other presets as well.

Go to `Settings (Cmd + ,)` -> `Profiles` -> `Default` -> `Colors` -> **`Color Presets...`** -> **`Solarized Dark`**

You can find a lot more in the [Online Gallery](https://iterm2colorschemes.com/).

### Unlimited scrollback

Today's Macs have so much RAM available we can safely increase or even completely remove the cap on history (scrollback). If you are a developer and compile or release something, the output can be extremely verbose. To be able to see everything properly it can be a good idea to turn off the limit on the scrollback buffer. If you experience a performance hit then you should fine-tune the _Scrollback lines_ settings instead.

Go to `Settings` -> `Profiles` -> `Default` -> `Terminal` -> **`Unlimited scrollback`**

### Natural Text Editing

The last setting is for convenience. If you want to edit a command before executing and would like to use the same shortcuts you know and love everywhere in macOS, like jumping left and right by word (`Option + Left` / `Option + Right`) or going to the beginning (`Cmd + Left`) or the end of the line (`Cmd + Right`), you should set the Key Mapping to **Natural Text Editing**.

Go to `Settings` -> `Profiles` -> `Default` -> `Keys` -> `Key Mappings` -> `Presets...` -> **`Natural Text Editing`**

I think it's easier than [defining escape sequences](https://superuser.com/a/641751) to all our shortcuts.

## Oh My Zsh

What is [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) and why do want it?

> Oh My Zsh is an open source, community-driven framework for managing your **zsh** configuration.

Alright, but what is **zsh**?

> ZSH, also called the Z shell, is an extended version of the Bourne Shell (sh), with plenty of new features, and support for plugins and themes. [Source](https://www.howtogeek.com/362409/what-is-zsh-and-why-should-you-use-it-instead-of-bash/)

Makes sense, except what is a **shell**?

> A shell is a software program used to interpret commands that are input via a command-line interface, enabling users to interact with a computer by giving it instructions. [Source](https://www.computerhope.com/jargon/s/shell.htm)

Apple has already replaced the default shell from bash to **zsh** in macOS Catalina. In my eyes, we are just improving it further with Oh My Zsh which comes with tons of features. Let's [install](https://github.com/ohmyzsh/ohmyzsh#basic-installation) it!

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

After the successful installation, we can start customizing it by editing our `~/.zshrc` file. The contents of this file are executed every time we create a new session.

This is my main config. (Will be extended in the upcoming steps.)

```
ZSH_THEME="robbyrussell"

zstyle ':omz:update' mode auto
zstyle ':omz:update' frequency 1

plugins=(bundler git macos xcode)

export LANG=en_US.UTF-8
export EDITOR='vim'
```

First, we can set up our theme. If you don't like this one, there is [a lot more to choose from](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes).

The next two lines just make sure that our Oh My Zsh is automatically updated every day.

Following that we can customize the plugins we use. There is a [myriad of available plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins), but in my experience, a large portion of them just adds _aliases_ that are pretty hard to remember when I can simply memorize the actual command itself. No need for indirections. For example, the `brew` plugin creates an _alias_ `bubc` for `brew upgrade && brew cleanup`. Which one seems easier to remember? For me, it's usually the latter. Don't get me wrong, these aliases can be useful, using the `bundler` plugin saved me a lot of time. Instead of writing manually `bundle install` 50+ times a day, I just say `bi` and we are ready to go. Experiment with what works for you.

What's an **alias**?
The alias command lets us create shortcuts for commands that we may need to enter frequently, making them easier to remember and use.

The last two lines set up the language environment and the preferred editor. If you are not familiar with the basics of `vim` I suggest choosing something else, like `nano`.

## rbenv and pyenv

As a developer or even a power user, sooner or later you'll have to use Python and/or Ruby. If you are like me, working on multiple projects that use different versions of Python and Ruby you have to figure out how to handle multiple versions at the same time. I found [`rbenv`](https://github.com/rbenv/rbenv) and [`pyenv`](https://github.com/pyenv/pyenv) reliable and easy to use with similar commands. Both can be installed with Homebrew.

```sh
brew install rbenv ruby-build
brew install pyenv
```

At the end of the installation `brew` will tell us about the initialization of these environments. The point is that when we start our terminal session we have to initialize the Ruby and Python environments. We can do this by putting the following commands into our `~/.zsrc`.

```sh
# Ruby
if command -v rbenv 1>/dev/null 2>&1; then
  eval "$(rbenv init -)"
fi

# Python
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

After we've edited our `~/.zshrc` we have to reload the configuration by either closing the terminal and opening a new session, or executing the following command.

```sh
source ~/.zshrc
```

Finally, we can install our Ruby and Python.

```sh
# List the latest stable versions and choose one (at the time of writing it's 3.2.0)
rbenv install -l

# Install Ruby 3.2.0
rbenv install 3.2.0

# Make Ruby 3.2.0 the default Ruby on the computer (first in PATH)
rbenv global 3.2.0
```

```sh
# List the latest stable versions and choose one (at the time of writing it's 3.11.1)
pyenv install -l

# Install Python 3.11.1
pyenv install 3.11.1

# Make Python 3.11.1 the default Python on the computer (first in PATH)
pyenv global 3.2.0
```

## powerline-shell

The Open Source [`powerline-shell`](https://github.com/b-ryan/powerline-shell) can make our terminal look even better. Installation is pretty easy after we have a Python installed.

```sh
pip install powerline-shell
```

After the successful installation let's create the config file for it and place it here: `~/.config/powerline-shell/config.json`. The configuration is further explained [here](https://github.com/b-ryan/powerline-shell#config-file). Customize for your needs.

```json
{
  "segments": ["virtual_env", "ssh", "cwd", "git", "hg", "jobs", "root"]
}
```

Finally, we have to edit our `~/.zshrc` once again to initialize it in every session.

```sh
# Powerline
function powerline_precmd() {
    PS1="$(powerline-shell --shell zsh $?)"
}

function install_powerline_precmd() {
  for s in "${precmd_functions[@]}"; do
    if [ "$s" = "powerline_precmd" ]; then
      return
    fi
  done
  precmd_functions+=(powerline_precmd)
}

if [ "$TERM" != "linux" ]; then
    install_powerline_precmd
fi
```

Once you start a new session (or `source` the zsh config), you will notice these weird-looking characters. Let's fix those by installing a proper font.

### Meslo Powerline Font

1. Download the font from here: [Meslo Powerline Font](https://github.com/powerline/fonts/raw/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf).
2. Open it and install it via macOS's Font Book.
3. In `iTerm2` change the font: go to `Settings` -> `Profiles` -> `Default` -> `Text` -> `Font` and select **`Meslo LG M for Powerline`**. Optionally you can also adjust the size of the text. I prefer 14pt.

## Enable Touch ID for `sudo`

Up until now, most of our commands ran without having to type in our password.

The Unix and Linux operating systems' `sudo` command stands for **superuser do**. A user can run a command with the superuser's (also known as the **root** user's) privileges, enabling them to carry out operations that the system would otherwise restrict. You will be prompted for your password when you use `sudo` to run a command. This is a safety precaution to guarantee that only users with permission to act as superusers can. If you have a Touch ID on your Mac or Magic Keyboard you can set it up to be able to use it instead of typing in the password.

A word of advice: **DO NOT MAKE A MISTAKE EDITING THE FOLLOWING FILE!**

Open the sudo file:

```sh
sudo vim /etc/pam.d/sudo
```

Then insert the following line to the top:

```sh
auth sufficient pam_tid.so
```

Save the file and close it. Open a new session (after typing in your password you will be able to sudo for a little while) and test your setting, e.g.: `sudo ls`. It should prompt you to the system's Touch ID screen.

![Sudoing with Touch ID](/assets/images/terminal-setup-on-macos/sudo_touch_id.png)

## 1Password CLI

I love [`1Password`](https://1password.com/). It really feels like I'm using a first-party application. Recently they've come up with something even more amazing: the [**`1Password CLI`**](https://developer.1password.com/docs/cli). There are tons of possibilities with it, I'm still playing with it but it's already useful for a bunch of things. Let me show you some.

If you have a `1Password` subscription and the app is installed already, you can install the `1Password CLI` via brew:

```sh
brew install --cask 1password/tap/1password-cli
```

You can also verify if the installation was successful:

```sh
op --version
```

To set it up just enable Touch ID and connect the two programs and finally log in with the CLI.

- Go to `Settings` -> `Security` -> turn on **`Touch ID`**, then go to `Settings` -> `Developer` -> turn on **`Connect with 1Password CLI`**. (Later we will also turn on the `Use the SSH agent`.)
  ![Sudoing with Touch ID](/assets/images/terminal-setup-on-macos/1password_settings.png)
- Sign in to the 1Password CLI by issuing any command, like `op vault ls`.

A huge problem even for experienced, senior software engineers is how they are storing their passwords and secrets. Sometimes they just put them in an environment variable or a configuration file. This is really bad. With 1Password CLI we can do better without sacrificing convenience. Currently, I'm mainly using it for two things:

- Store and load secrets securely
- Authenticate with SSH automatically

### Store and load secrets securly

> With 1Password CLI, you can use secret references to securely load secrets saved in 1Password in environment variables, configuration files, and anywhere else you might need them, without putting any plaintext secrets in code. At runtime, secret references are automatically replaced with the actual secrets they refer to. [Source](https://developer.1password.com/docs/cli/secret-references)

Let's see an example. We are using [Danger](https://danger.systems/js/) to automate some parts of our [code review](https://smartbear.com/learn/code-review/what-is-code-review/) process. If we want to connect it to Bitbucket Cloud, we need the UUID, the username and an app password. I stored these in 1Password and then created environment variables in my `~/.zshrc`.

```sh
export DANGER_BITBUCKETCLOUD_UUID="op://Work/Atlassian/Info/UUID"
export DANGER_BITBUCKETCLOUD_USERNAME="op://Work/Atlassian/Info/username"
export DANGER_BITBUCKETCLOUD_PASSWORD="op://Work/Atlassian/AppPasswords/Danger"
```

Before executing the `danger` command locally, I just have to prefix it with `op run --` and the environment variables will be replaced with the actual passwords. It's amazing!

```sh
op run -- bundle exec danger pr https://bitbucket.org/...`
```

Note: running this command prompts for your Touch ID.

### Authenticate with SSH automatically

I have a lot of [SSH keys](https://www.ssh.com/academy/ssh-keys), usually using a different one for each and every Git provider and client we are working for. Managing these, especially if you have multiple Bitbucket accounts for example can be a pain. Writing complex ssh configurations. `1Password CLI` comes to our rescue here as well.
First, we can store our SSH keys in `1Password` **vaults**, which is already better than the default `~/.ssh` directory. The real kicker, however, is that we can automatically authenticate with our Touch ID when we perform `git` commands. It's like magic. 🎉

To set this up follow the [guide](https://developer.1password.com/docs/ssh). The main steps are:

- Turning on `Settings` -> `Developer` -> **`Use the SSH agent`**
- Add the proper config to your SSH config (`~/.ssh/config`). `1Password` settings will show it.

```sh
Host *
	IdentityAgent "~/Library/Group Containers/XXXXXXXXXX.com.1password/t/agent.sock"
```

- Add your SSH keys to your Vault
- Done ✅

### Other ideas

By browsing the [documentation](https://developer.1password.com/docs/cli) we can see that there are other possibilities, like

- [Connecting to Servers](https://developer.1password.com/docs/connect)
- [Integrate 1Password to CI/CD solutions](https://developer.1password.com/docs/ci-cd)
- [Tinker with web development](https://developer.1password.com/docs/web/add-1password-button-website)
- [Better integrate your workflow into VS Code](https://developer.1password.com/docs/vscode)
- etc.

## Acknowledgement

Years ago I've started my terminal customization journey thanks to Felix Krause, the original creator of [fastlane.tools](https://fastlane.tools/). To this day I still use a large part of his [ideas](https://github.com/KrauseFx/what-terminal-is-felix-using).

## Summary

If you've reached this point you should have a customized terminal environment setup. Congratulations! As a developer or power user, setting up our terminal can significantly increase our productivity and efficiency. We can improve our command-line experience by streamlining our workflow, adding useful command-line tools, and customizing our prompt. Additionally, we can manage our code changes and collaborate with others by setting up our terminal to work with version control systems like Git. With 1Password we can also mitigate some of the security issues, but don't forget to keep your command-line and terminal tools up to date.

## Versions

- macOS Ventura 13.1 (22C65)
- iTerm Build 3.4.19
- rbenv 1.2.0
  - Ruby 3.2.0
- pyenv 2.3.10
  - Python 3.11.1
- 1Password for Mac 8.9.13 (80913040)
  - 1Password CLI 2.12.0
