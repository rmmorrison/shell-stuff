# shell-stuff

This repository (and README) file is a collection of terminal configuration, shell scripts and otherwise just useful
tools/tricks I've come across which I find useful in my day-to-day work.

## Terminal Emulator

### [Ghostty](http://ghostty.org)

> Ghostty is a fast, feature-rich, and cross-platform terminal emulator that uses platform-native UI and GPU acceleration.

Great, performant terminal emulator by one of the co-founders of HashiCorp. It's the terminal emulator that's finally dethroned iTerm2 for me. It features sane defaults (I have zero configuration outside of a theme) and handles difficult workloads with ease. I used to bog down iTerm2 tailing busy container logs with k9s, but Ghostty handles it without breaking a sweat.

I like the Snazzy theme, which is set via file-based configuration:

> **~/.config/ghostty/config**
> ```
> theme = Snazzy
> ```

## Shell Extensions

### [Oh My Zsh](https://ohmyz.sh)

> Oh My Zsh is a delightful, open source, community-driven framework for managing your Zsh configuration. It comes bundled with thousands of helpful functions, helpers, plugins, themes, and a few things that make you shout...
>
> "Oh My ZSH!"

Batteries-included zsh configuration framework. Great now that the default shell on macOS is zsh. I don't use its themes (see below) but I really like some of the out-of-the-box plugins for autocomplete support in some of my most used tooling:

> **~/.zshrc**
> ```
> plugins(git aws docker vscode zsh-syntax-highlighting)
> ```

Honestly, I need to revisit this plugin list, as I don't really take full advantage of it. There's other out-of-the-box plugins that support other tools I like which I just don't use here. I should fix that.

(`zsh-syntax-highlighting` isn't out of the box and is [available here](https://github.com/zsh-users/zsh-syntax-highlighting). It adds fish-style syntax highlighting as commands are being typed. I like the visuals.)

### [Powerlevel10k](https://github.com/romkatv/powerlevel10k)

> Powerlevel10k is a theme for Zsh. It emphasizes speed, flexibility and out-of-the-box experience.

I like this theme because it's a) highly customizable, b) ships with an interactive-driven configuration wizard that allows you to explore the customization visually and c) includes an "Instant prompt" implementation that makes shell startup time _blazing fast_. Combined with Ghostty I can drop into shells and new tabs at lightning speed. Can be installed as an `oh-my-zsh` plugin.

## Package Management

### [Homebrew](https://brew.sh)

> Homebrew installs the stuff you need that Apple (or your Linux system) didn’t.

The _standard_ package manager for macOS. Simple to use, and doesn't clutter your system (everything installs under `/opt/homebrew`). Makes installing virtually everything after this incredibly straightforward.

### [mise-en-place](https://mise.jdx.dev)

> mise is a tool that manages installations of programming language runtimes and other tools for local development. For example, it can be used to manage multiple versions of Node.js, Python, Ruby, Go, etc. on the same machine.

Homebrew doesn't handle use cases where multiple versions of a language or tool are required. mise-en-place is an excellent compliment to handle this gap. Allows installation of multiple versions, switching to them via a `mise` command or (even better!) dotfiles in project directories, which specify required tools and versions. mise-en-place reads these files as you traverse directories and automatically switches as needed. Integrates with JetBrains tooling quite well (it's how I like to install JDKs now).

## Tools

### [tmux](https://github.com/tmux/tmux)

> tmux is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal.

Incredibly useful if your terminal emulator lacks tab support, or you're working on remote hosts over ssh (using a multiplexer like tmux is much better than a whole bunch of ssh connections). Great even as a safety net if you disconnect from a remote host - just reattach to your session and you'll be back where you left off.

### [tpm](https://github.com/tmux-plugins/tpm)

> Installs and loads tmux plugins.

Does what it says on the box. A nice benefit is the installation instructions include the plugin `tmux-sensible`, which besides being some sane defaults for tmux automatically configures tmux for `xterm-256color` support (which `powerlevel10k` requires).

### [dracula](https://github.com/dracula/tmux)

> A dark theme for tmux

The default tmux colour scheme is...not pleasant. This theme is a nice improvement, and in my opinion meshes well with Ghostty's Snazzy theme. But it's a nice dark theme that should blend comfortably with most dark themes. Installable via `tpm`.

Despite being described as just a theme, it also includes a number of out-of-the-box plugins to customize your status bar. I prefer a clean look for my status bar so only enable powerline symbols support:

> **~/.tmux.conf**
> ```
> set -g @dracula-plugins 'powerline'
> set -g @dracula-show-powerline true
> ```

### [bat](https://github.com/sharkdp/bat)

> A cat(1) clone with syntax highlighting and Git integration.

I like to wholesale replace `cat` with this via an alias:

> **~/.zshrc**
> ```
> alias cat='bat'
> ```

### [ripgrep](https://github.com/BurntSushi/ripgrep)

> ripgrep is a line-oriented search tool that recursively searches the current directory for a regex pattern. By default, ripgrep will respect gitignore rules and automatically skip hidden files/directories and binary files. (To disable all automatic filtering by default, use rg -uuu.) ripgrep has first class support on Windows, macOS and Linux, with binary downloads available for every release. ripgrep is similar to other popular search tools like The Silver Searcher, ack and grep.

Much faster than GNU grep, and respects `.gitignore` files by default. I use this as my default search tool.

### [eza](https://github.com/eza-community/eza)

> eza is a modern alternative for the venerable file-listing command-line program ls that ships with Unix and Linux operating systems, giving it more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git.

I like to wholesale replace `ls` with this via an alias:

> **~/.zshrc**
> ```
> alias ls='eza'
> ```

(I can't imagine going back to plain ol' `ls` now.)

### [fzf](https://github.com/junegunn/fzf)

> fzf is a general-purpose command-line fuzzy finder. It's an interactive filter program for any kind of list; files, command history, processes, hostnames, bookmarks, git commits, etc. It implements a "fuzzy" matching algorithm, so you can quickly type in patterns with omitted characters and still get the results you want.

Lots of versatility here but frankly I primarily use this for its much better fuzzy history search (`CTRL+R`).

### [xh](https://github.com/ducaale/xh)

> xh is a friendly and fast tool for sending HTTP requests. It reimplements as much as possible of HTTPie's excellent design, with a focus on improved performance.

I like this for firing off quick HTTP requests from a terminal. I especially like its `--curl` flag, which prints the request translated to a `curl` command - great for sharing with others who may or may not have HTTPie/xh.

### [delta](https://github.com/dandavison/delta)

> A syntax-highlighting pager for git, diff, and grep output

I've always worked with git directly from the terminal (I've yet to find a UI that I like) and this tool makes reading diffs _so much better_.

### [k9s](https://github.com/derailed/k9s)

> K9s provides a terminal UI to interact with your Kubernetes clusters. The aim of this project is to make it easier to navigate, observe and manage your applications in the wild. K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.

There is no better way to interact with Kubernetes clusters than k9s. Does an excellent job visualizing the cluster and its resources, and is lightning quick to navigate around. I evangelize k9s to everyone who'll listen.

### [cdebug](https://github.com/iximiuz/cdebug)

> a swiss army knife of container debugging

This is a very useful tool for debugging containers, especially ones that lack an interactive shell but you _really_ need to get inside and poke around. Through a single command, you can launch a debugging sidecar that shares the same root filesystem as the container you wish to inspect plus have all the tools of the sidecar available. Great for situations like if you need to thread dump a JVM application but are only running atop a JRE, for example (use this to launch a JDK container to run `jstack`).

### [kubecolor](https://github.com/hidetatz/kubecolor)

> Colorize your kubectl output

Self-explanatory. Works best if you alias it to `kubectl` (it's basically a transparent proxy to add colour):

> **~/.zshrc**
> ```
> alias kubectl='kubecolor'
> ```

### [kubectx](https://github.com/ahmetb/kubectx)

> kubectx is a tool to switch between contexts (clusters) on kubectl faster.

My `~/.kube/config` is primarily built on the output from `aws eks update-kubeconfig`, and so my contexts are named after ARNs which aren't simple to remember or type out each time I want to switch context. Using this with `fzf` (above) is a must-have, as there's an automatic integration that enables fuzzy search on context names. This is a straight up timesaver for me.

## Goodies

### tmux cheat sheet

Getting used to `tmux` can be a bit of a learning curve. Having a cheat sheet handy like [this one](https://tmuxcheatsheet.com) is pretty helpful while you're getting used to the ropes.

### Editor environment hook

I like to use the `EDITOR` environment hook to use my editor of choice (Visual Studio Code). Plenty of people are happy to use `vi` or `nano` but I prefer a more familiar workspace especially when editing complex resources in Kubernetes, for example.

> **~/.zshrc**
> ```
> export EDITOR='code -w'
> ```

(The `-w` flag is important - it tells Visual Studio Code to wait for the file to be closed before returning. Without it, commands waiting for the editor would proceed immediately without waiting for the actual edits.)

### Clean up local git branches

It's not difficult to accumulate a lot of local git branches over time, especially if working on a fairly large project. This can become a bit of a pain when trying to switch branches as you might have to sift through dozens of branches if you _can't_ quite remember the name of the one you were working on last week.

Here's a little git alias that will delete all local branches that have been merged into `master` (adjust for your mainline branch as necessary):

> **~/.gitconfig**
> ```
> [alias]
>     br-delete-useless = "!f(){\git branch | grep -v "master" | grep -v ^* | xargs git branch -d;\}; f"
> ```

You can also create a second variant that switches out the `git branch -d` for `git branch -D` to force delete all branches besides the current one and `master` (again, adjust for your mainline branch as necessary):

> **~/.gitconfig**
> ```
> [alias]
>     br-delete-useless-force = "!f(){\git branch | grep -v "master" | grep -v ^* | xargs git branch -D;\}; f"
> ```

## Honourable Mentions

*Items in this section are less frequently used, or I think are interesting but haven't quite made it into my day-to-day workflow.*

### [trurl](https://github.com/curl/trurl)

> Command line tool for URL parsing and manipulation

Part of the cURL project. Useful if you need to parse/manipulate URLs from a terminal or shell script.

### [runlike](https://github.com/lavie/runlike)

> You give it a docker container, it outputs the command line necessary to run another one just like it, along with all those pesky options (ports, links, volumes, ...). It's a real time saver for those that normally deploy their docker containers via some CM tool like Ansible/Chef and then find themselves needing to manually re-run some container.

Haven't had the need for this one yet, but I've been keeping it in my back pocket in case I ever need to.

### [dive](https://github.com/wagoodman/dive)

> A tool for exploring a docker image, layer contents, and discovering ways to shrink the size of your Docker/OCI image.

Great for 'taking apart' a Docker image and seeing whats inside, layer by layer - without even needing to run it.

### [helm-release-plugin](https://github.com/JovianX/helm-release-plugin)

> helm-release is a Helm 3 plugin that allows running operations on Helm releases (deployed Helm charts).

The key feature I like of this plugin is the `helm release pull` command - like `runlike` but for Helm charts. Takes a deployed release and re-creates a Helm chart from it. I've needed to do this a couple of times - much easier than trying to work backwards yourself.

### [direnv](https://github.com/direnv/direnv)

> direnv is an extension for your shell. It augments existing shells with a new feature that can load and unload environment variables depending on the current directory.

Useful for projects that need specific environment variables to run but you don't want to clutter your global environment with them, or they conflict with other projects/tools.

### [posting](https://github.com/darrenburns/posting)

> Posting is an HTTP client, not unlike Postman and Insomnia. As a TUI application, it can be used over SSH and enables efficient keyboard-centric workflows. Your requests are stored locally in simple YAML files, so they're easy to read and version control.

This one is new to my toolbox and so I need to explore it some more before I can say if it's a keeper - but it's a nice looking TUI, supports OpenAPI specs (very nice!), importing cURL requests and environments/variables. I need to see how easy it is to copy response bodies/headers from it, though - as that's something I do frequently enough where it'd be a pain without it.

### [ctop](https://github.com/bcicen/ctop)

> ctop provides a concise and condensed overview of real-time metrics for multiple containers

It's `top`, but for containers. I've found that `k9s` displays enough about metrics to be sufficient for my everyday work, but I do like the visualization here.

### [lazygit](https://github.com/jesseduffield/lazygit)

I need to share its 'elevator pitch':

> Rant time: You've heard it before, git is powerful, but what good is that power when everything is so damn hard to do? Interactive rebasing requires you to edit a goddamn TODO file in your editor? Are you kidding me? To stage part of a file you need to use a command line program to step through each hunk and if a hunk can't be split down any further but contains code you don't want to stage, you have to edit an arcane patch file by hand? Are you KIDDING me?! Sometimes you get asked to stash your changes when switching branches only to realise that after you switch and unstash that there weren't even any conflicts and it would have been fine to just checkout the branch directly? YOU HAVE GOT TO BE KIDDING ME!
>
> If you're a mere mortal like me and you're tired of hearing how powerful git is when in your daily life it's a powerful pain in your ass, lazygit might be for you.

I've yet to find any sort of UI I would rather use over plain ol' git commands but I think this is an interesting candidate striking the balance between taking some of the pain away from git while avoiding oversimplifying it too much.

### [lazydocker](https://github.com/jesseduffield/lazydocker)

> A simple terminal UI for both docker and docker-compose, written in Go

From the same author as `lazygit`. I don't work with Docker containers directly enough these days to make much use of this tool, but I can see this being incredibly useful for those who do (or home labs...)
