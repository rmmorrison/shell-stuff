# shell-stuff

## What is this?

Over the past few years I've refined how I like to use a shell. I've discovered a number of useful additions from terminal  emulators to themes to supporting tools (and even tools that replace coreutils with improved functionality). This repository is intended to document all of this not just for myself in the future to rebuild my preferred environment but also to share with others as inspiration to explore and find ways to improve their workflows.

## Notes

* This setup is built around macOS and `zsh` (as the default shell on macOS), but should be very easily adaptable to Linux and even Windows (somewhat, if WSL2 is your thing). Paths to configuration files are documented according to their location on macOS, which _generally_ is the same location on Linux. If I'm aware of differences I will try to document them here, but Linux users should double-check the documentation to be sure.
* I originally intended to include dotfiles in this repository as well to share specific configuration files, but as it turns out I apply very little customization to all the tools I use day to day - a testament to sensible defaults. If there is any customization, I'll document it inline rather than a separate location.
* The [Honourable Mentions](#honourable-mentions) section is for tools that I think are interesting but haven't quite made it into my day-to-day workflow.

## Table of Contents

* [Terminal Emulator](#terminal-emulator)
  * [Ghostty](#ghostty)
* [Shell Extensions](#shell-extensions)
  * [oh-my-zsh](#oh-my-zsh)
  * [Powerlevel10k](#powerlevel10k)
* [Package Management](#package-management)
  * [Homebrew](#homebrew)
  * [mise-en-place](#mise-en-place)
* [Tools](#tools)
  * [tmux](#tmux)
  * [tpm](#tpm)
  * [dracula](#dracula)
  * [bat](#bat)
  * [ripgrep](#ripgrep)
  * [eza](#eza)
  * [television](#television)
  * [xh](#xh)
  * [delta](#delta)
  * [k9s](#k9s)
  * [cdebug](#cdebug)
  * [kubecolor](#kubecolor)
  * [kubectx](#kubectx)
  * [fzf](#fzf)
  * [uv](#uv)
  * [fx](#fx)
  * [jq](#jq)
  * [lazygit](#lazygit)
* [Honourable Mentions](#honourable-mentions)
  * [trurl](#trurl)
  * [runlike](#runlike)
  * [dive](#dive)
  * [helm-release-plugin](#helm-release-plugin)
  * [direnv](#direnv)
  * [posting](#posting)
  * [ctop](#ctop)
  * [lazydocker](#lazydocker)

## Terminal Emulator

### [Ghostty](http://ghostty.org)

> Ghostty is a fast, feature-rich, and cross-platform terminal emulator that uses platform-native UI and GPU acceleration.

I have used iTerm2 for the better part of a decade. It's served me well, and while I've experimented with others (alacritty, kitty) I've never been convinced to switch - until now. Ghostty was created by one of the co-founders of HashiCorp, is lightweight, has great configuration and documentation, and is very fast. If there was a complaint I had with iTerm2, it's that it wasn't difficult to bog it down with fast-scrolling text which was often an annoyance when trying to tail some debug logs, for instance. Ghostty has handled everything I've thrown at it with ease.

I like the Snazzy theme, which is set via Ghostty's text-based configuration:

> *~/.config/ghostty/config*
> ```
> theme = Snazzy
> ```

I also disable window save state. I find the feature works fairly inconsistently by default and I prefer to always start from a clean slate, anyway:

> *~/.config/ghostty/config*
> ```
> window-save-state = never
> ```

(This configuration is macOS only and has no effect on Linux.)

## Shell Extensions

### [oh-my-zsh](https://ohmyz.sh)

> Oh My Zsh is a delightful, open source, community-driven framework for managing your Zsh configuration. It comes bundled with thousands of helpful functions, helpers, plugins, themes, and a few things that make you shout...
>
> "Oh My ZSH!"

This is a configuration framework for `zsh` that's "batteries-included" and helpfully improves the `zsh` experience.

I use a number of built-in plugins to enhance my shell:

* `eza`: aliases that invoke the `eza` utility rather than `ls`
* `mise`: adds integration with mise (formerly rtx), a runtime executor compatible with npm, nodenv, pyenv, etc.
* `ssh`: provides host completion based off of your `~/.ssh/config` file, and adds some utility functions to work with SSH keys
* `ssh-agent`: automatically starts `ssh-agent` to set up and load whichever credentials you want for ssh connections
* `zsh-syntax-highlighting`: fish-style syntax highlighting as commands are being typed

> *~/.zshrc*
> ```
> plugins(eza mise ssh ssh-agent zsh-syntax-highlighting)
> ```

There are a *lot* of built-in plugins available for `oh-my-zsh`, and it's worth reading over the [entire list](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) to see if there are any others that would suit your environment. Note that plugins do slow down start performance, so it's best to run only what you need/is actually useful for you.

(`zsh-syntax-highlighting` isn't included with `oh-my-zsh` and is [available here](https://github.com/zsh-users/zsh-syntax-highlighting).)

If using the `ssh-agent` plugin, I also add the following to my `~/.zshrc` to silence its output (this must appear before `source $ZSH/oh-my-zsh.sh`):

> *~/.zshrc*
> ```
> zstyle :omz:plugins:ssh-agent quiet yes
> ```

### [Powerlevel10k](https://github.com/romkatv/powerlevel10k)

> Powerlevel10k is a theme for Zsh. It emphasizes speed, flexibility and out-of-the-box experience.

I like this theme for a couple of reasons: its wizard-like configuration that really helps you visually dial in your preferred look and feel to the theme, and the "instant prompt" feature which displays a prompt immediately while loading everything else in the background, asynchronously. The combination of Powerlevel10k and [Ghostty](#ghostty) makes for a very fast and responsive shell experience.

## Package Management

### [Homebrew](https://brew.sh)

> Homebrew installs the stuff you need that Apple (or your Linux system) didn’t.

The _standard_ package manager for macOS. Simple to use, and doesn't clutter your system (everything installs under `/opt/homebrew`). Makes installing virtually everything after this incredibly straightforward.

### [mise-en-place](https://mise.jdx.dev)

> mise is a tool that manages installations of programming language runtimes and other tools for local development. For example, it can be used to manage multiple versions of Node.js, Python, Ruby, Go, etc. on the same machine.

[Homebrew](#homebrew) is great but one of the cases it (and other package managers) doesn't handle well is the need for multiple versions of a package simultaneously. You can install multiple versions side-by-side, but which one gets used boils down to whichever one was installed last and had its shims put in place.

`mise` addresses this gap by not only handling multiple versions of tools but seamlessly allows you to switch between them, either manually via a `mise` command or by simply placing a `mise.toml` file with a specific tool and version where it's needed. `mise` hooks into the shell and will automatically ensure the correct tool and version is on your `PATH` as you navigate around the filesystem.

You can also include environment variables in `mise.toml` and `mise` will load/unload them as you move across directories, replacing the need for a separate utility like `direnv`.

(Note that if you're using `oh-my-zsh` and the `mise` plugin, the installation step to add `mise activate` to your `~/.zshrc` is unnecessary.)

## Tools

### [tmux](https://github.com/tmux/tmux)

> tmux is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal.

I enable a few small configuration tweaks:

> *~/.tmux.conf*
> ```
> # remap prefix from 'C-b' to 'C-a'
> unbind C-b
> set-option -g prefix C-a
> bind-key C-a send-prefix
> 
> # enable mouse support
> set -g mouse on
> 
> # split panes using | and -
> bind | split-window -h
> bind - split-window -v
> unbind '"'
> unbind %
> 
> # start window numbering at 1
> set -g base-index 1
> set -g pane-base-index 1
> set-window-option -g pane-base-index 1
> set-option -g renumber-windows 1
> ```

### [tpm](https://github.com/tmux-plugins/tpm)

> Installs and loads tmux plugins.

Does what it says on the box. A nice benefit is the installation instructions include the plugin `tmux-sensible`, which besides being some sane defaults for tmux automatically configures tmux for `xterm-256color` support (which [Powerlevel10k](#powerlevel10k) requires).

### [dracula](https://github.com/dracula/tmux)

> A dark theme for tmux

The default tmux colour scheme is...not pleasant. This theme is a nice improvement, and in my opinion meshes well with Ghostty's Snazzy theme. But it's a nice dark theme that should blend comfortably with most other terminal dark themes. Installable via `tpm`.

Despite being described as just a theme, it also includes a number of out-of-the-box plugins to customize your status bar. I prefer a clean look for my status bar so only enable powerline symbols support:

> *~/.tmux.conf*
> ```
> set -g @dracula-plugins 'powerline'
> set -g @dracula-show-powerline true
> ```

### [bat](https://github.com/sharkdp/bat)

> A cat(1) clone with syntax highlighting and Git integration.

It's `cat` on steroids. I like to wholesale replace `cat` with this via an alias:

> *~/.zshrc*
> ```
> alias cat='bat'
> ```

### [ripgrep](https://github.com/BurntSushi/ripgrep)

> ripgrep is a line-oriented search tool that recursively searches the current directory for a regex pattern. By default, ripgrep will respect gitignore rules and automatically skip hidden files/directories and binary files. (To disable all automatic filtering by default, use rg -uuu.) ripgrep has first class support on Windows, macOS and Linux, with binary downloads available for every release. ripgrep is similar to other popular search tools like The Silver Searcher, ack and grep.

### [eza](https://github.com/eza-community/eza)

> eza is a modern alternative for the venerable file-listing command-line program ls that ships with Unix and Linux operating systems, giving it more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git.

I like to completely replace `ls` with `eza`, since it's such a substantial improvement. If using `oh-my-zsh`, the `eza` plugin will handle this for you.

Otherwise, you can alias `ls` to `eza`:

> *~/.zshrc*
> ```
> alias ls='eza'
> ```

(I can't imagine going back to plain ol' `ls` now.)

### [television](https://github.com/alexpasmantier/television)

> Television is a fast and versatile fuzzy finder TUI. It lets you quickly search through any kind of data source (files, git repositories, environment variables, docker images, you name it) using a fuzzy matching algorithm and is designed to be easily extensible.

This is a recent replacement for `fzf` which I like a lot (especially for fuzzy history search via `CTRL+R`), but with some nice extra functionality including built-in preview support and easy extensibility via "channels".

I prefer the `television` theme (not to be confused with the out of the box `default` theme):

> *~/.config/television/config*
> ```
> theme = "television"
> ```

### [xh](https://github.com/ducaale/xh)

> xh is a friendly and fast tool for sending HTTP requests. It reimplements as much as possible of HTTPie's excellent design, with a focus on improved performance.

I like this for firing off quick HTTP requests from a terminal. I especially like its `--curl` flag, which prints the request translated to a `curl` command - great for sharing with others who may or may not have HTTPie/xh.

### [delta](https://github.com/dandavison/delta)

> A syntax-highlighting pager for git, diff, and grep output

I primarily work with `git` from the CLI and this tool makes reading diffs _so much better_.

### [k9s](https://github.com/derailed/k9s)

> K9s provides a terminal UI to interact with your Kubernetes clusters. The aim of this project is to make it easier to navigate, observe and manage your applications in the wild. K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.

There is no better way to interact with Kubernetes clusters than k9s. Does an excellent job visualizing the cluster and its resources, and is lightning quick to navigate around. I evangelize k9s to everyone who'll listen.

### [cdebug](https://github.com/iximiuz/cdebug)

> a swiss army knife of container debugging

This is a very useful tool for debugging containers, especially ones that lack an interactive shell but you _really_ need to get inside and poke around. Through a single command, you can launch a debugging sidecar that shares the same root filesystem as the container you wish to inspect plus have all the tools of the sidecar available. Great for situations like if you need to thread dump a JVM application but are only running atop a JRE, for example (use this to launch a JDK container to run `jstack`).

### [kubecolor](https://github.com/hidetatz/kubecolor)

> Colorize your kubectl output

I prefer to alias `kubecolor` to `kubectl` (it's basically a transparent proxy to add colour), primarily so I don't forget to use it:

> *~/.zshrc*
> ```
> alias kubectl='kubecolor'
> ```

This alias doesn't seem to affect any other tools that use `kubectl`.

### [kubectx](https://github.com/ahmetb/kubectx)

> kubectx is a tool to switch between contexts (clusters) on kubectl faster.

My `~/.kube/config` is primarily built on the output from `aws eks update-kubeconfig`, and so my contexts are named after ARNs which aren't simple to remember or type out each time I want to switch context. Using this with [fzf](#fzf) (below) is a must-have, as there's an automatic integration that enables fuzzy search on context names. This is a straight up timesaver for me.

### [fzf](https://github.com/junegunn/fzf)

> fzf is a general-purpose command-line fuzzy finder. It's an interactive filter program for any kind of list; files, command history, processes, hostnames, bookmarks, git commits, etc. It implements a "fuzzy" matching algorithm, so you can quickly type in patterns with omitted characters and still get the results you want.

Almost entirely replaced by [television](#television)...except for [kubectx](#kubectx), which integrates with `fzf` but not `television` (and is unlikely to do so, since the last release was in 2023). Just needs to be on the path for `kubectx`, no shell integration required.

### [uv](https://docs.astral.sh/uv/)

> A single tool to replace pip, pip-tools, pipx, poetry, pyenv, twine, virtualenv, and more.

Combines a number of Python tools into a single Rust-based one. Great in combination with [mise-en-place](#mise-en-place) since it's independent of a specific Python version/installation, so you can switch between multiple versions as necessary and `uv` will handle the rest. You _can_ use it to manage installations as well, although I stick with `mise` for that purpose.

### <a name="fx"></a>[f(x)](https://github.com/antonmedv/fx)

> Fx is a dual-purpose command-line tool tailored for JSON, providing both a terminal-based JSON viewer and a JSON processing utility.

Very useful when combined with other tools like `xh` to quickly process and visualize a JSON document. I've yet to use it as a processor, since I've normally relied on...

### [jq](https://github.com/jqlang/jq)

> jq is a lightweight and flexible command-line JSON processor akin to sed,awk,grep, and friends for JSON data. It's written in portable C and has zero runtime dependencies, allowing you to easily slice, filter, map, and transform structured data.

Makes processing JSON at the terminal or in a script a breeze. Ubiquitous enough that you can also just ask an LLM how to process a document and it'll spit out an accurate or close-enough `jq` command to do it.

### [lazygit](https://github.com/jesseduffield/lazygit)

I need to share its 'elevator pitch':

> Rant time: You've heard it before, git is powerful, but what good is that power when everything is so damn hard to do? Interactive rebasing requires you to edit a goddamn TODO file in your editor? Are you kidding me? To stage part of a file you need to use a command line program to step through each hunk and if a hunk can't be split down any further but contains code you don't want to stage, you have to edit an arcane patch file by hand? Are you KIDDING me?! Sometimes you get asked to stash your changes when switching branches only to realise that after you switch and unstash that there weren't even any conflicts and it would have been fine to just checkout the branch directly? YOU HAVE GOT TO BE KIDDING ME!
>
> If you're a mere mortal like me and you're tired of hearing how powerful git is when in your daily life it's a powerful pain in your ass, lazygit might be for you.

Pretty much the only git GUI I use that doesn't want me to tear my hair out doing anything beyond the usual branch/push/pull functions and send me back to the CLI anyway.

Beyond the default configuration, I enable [delta](#delta) as the default pager, enable hyperlinks and enable hyperlinks on line numbers opening in my configured `EDITOR`:

> *~/Library/Application \Support/lazygit/config.yml* (macOS)<br />
> *~/.config/lazygit/config.yml* (Linux)
> ```yaml
> git:
>   paging:
>     colorArg: always
>     pager: delta --paging=never --line-numbers --hyperlinks --hyperlinks-file-link-format="lazygit-edit://{path}:{line}"
> ```

## Honourable Mentions

*Items in this section are less frequently used, or I think are interesting but haven't quite made it into my day-to-day workflow.*

### [trurl](https://github.com/curl/trurl)

> Command line tool for URL parsing and manipulation

Part of the cURL project. Useful if you need to parse/manipulate URLs from a terminal or shell script.

### [runlike](https://github.com/lavie/runlike)

> You give it a docker container, it outputs the command line necessary to run another one just like it, along with all those pesky options (ports, links, volumes, ...). It's a real time saver for those that normally deploy their docker containers via some CM tool like Ansible/Chef and then find themselves needing to manually re-run some container.

### [dive](https://github.com/wagoodman/dive)

> A tool for exploring a docker image, layer contents, and discovering ways to shrink the size of your Docker/OCI image.

### [helm-release-plugin](https://github.com/JovianX/helm-release-plugin)

> helm-release is a Helm 3 plugin that allows running operations on Helm releases (deployed Helm charts).

The key feature I like of this plugin is the `helm release pull` command - like `runlike` but for Helm charts. Takes a deployed release and re-creates a Helm chart from it. I've needed to do this a couple of times - much easier than trying to work backwards yourself.

### [direnv](https://github.com/direnv/direnv)

> direnv is an extension for your shell. It augments existing shells with a new feature that can load and unload environment variables depending on the current directory.

[mise-en-place](#mise-en-place) also contains this functionality, so installing and using both aren't necessary.

I prefer to disable direnv's output as you navigate directories as it can be quite noisy:

> *~/.config/direnv/direnv.toml*
> ```toml
> [global]
> hide_env_diff = true
> log_format = '-'
> ```

### [posting](https://github.com/darrenburns/posting)

> Posting is an HTTP client, not unlike Postman and Insomnia. As a TUI application, it can be used over SSH and enables efficient keyboard-centric workflows. Your requests are stored locally in simple YAML files, so they're easy to read and version control.

This one is new to my toolbox and so I need to explore it some more before I can say if it's a keeper - but it's a nice looking TUI, supports OpenAPI specs (very nice!), importing cURL requests and environments/variables.

It also integrates with [`f(x)`](#fx) for visualizing JSON with a little configuration:

> *~/.config/posting/config.yaml*
> ```yaml
> pager_json: fx
> ```

### [ctop](https://github.com/bcicen/ctop)

> ctop provides a concise and condensed overview of real-time metrics for multiple containers

### [lazydocker](https://github.com/jesseduffield/lazydocker)

> A simple terminal UI for both docker and docker-compose, written in Go
