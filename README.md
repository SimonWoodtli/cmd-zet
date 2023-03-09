# zet

This is `zet`, a command line tool for managing your digital Zettelkasten
(notebox). Many commands use fzf for easier management of a Zettelkasten
repository. `zet` is a bash script that also uses `rg` for faster grep searches
and `fd` for better filename search performance.

> 🧐 I strongly recommend using two Zettelkasten git repositories. A public one
with a remote git repository on github.com and a private one that exists only
as a local git repository.

Quick links:

* [How to use]
* [Install]

## Features

* intuitive syntax: `zet [OPTIONS] [<subcommand>] [<pattern>]...`
* uses  fzf to fuzzy find Zettels (notes).
* in vim `!!zet <subcommand>` works with fzf
* uses `rg` and `fd` for grep and find replacements for better performance.
* supports parallel command execution: Create or edit Zettels in different tmux
  or terminal windows at the same time.
* smart case: searching within `fzf` or with patterns is case-insensitive by
  default.

## Demo

[screencast.webm](https://user-images.githubusercontent.com/66033447/209848299-2d3f7c8b-7ee6-4044-b7ef-61a54c78d10b.webm)

## How to use

~~~bash
COMMANDS:

    zet init
    zet current
    zet last
    zet delete
    zet edit
    zet get
    zet body
    zet cat
    zet count
    zet help
    zet version
    zet usage
    zet screenshot <pattern>
    zet use <pattern>
    zet create <pattern>
    zet link <pattern>
    zet query <pattern>
~~~

## Installation

Since people have too many different systems with different configurations,
I didn't want to write a universal setup script. However, the installation of
`zet` is quite simple.

1. fetch the latest version and make it executable:

```bash
curl -LJ https://raw.githubusercontent.com/SimonWoodtli/cmd-zet/main/zet -o $HOME/.local/bin/zet
chmod u+x $HOME/.local/bin/zet
```

2. install [dependencies]
3. create a github.com public repo called `zet` 
4. clone your public repo to your local machine
5. decide whether you want to create another github repo for your private notes
   or just a local git repo also name it `zet`
6. adjust some settings in the `zet` command: `vi $HOME/.local/bin/zet`
    1. Look for the \_configData() function and adjust the settings to your
       needs.
    1. zetPublicDir: the directory path to where you cloned your public `zet`
       repo
    1. zetPrivateDir: the directory path to where you created your private
       `zet` repo
    1. editor: the editor that `zet` will use
    1. screenshotDir: the location where your screenshots are being taken.
       Change this only if you want to use a different screenshot tool. Or if
       you want to use a specific folder. Both require changes to the
       \_\_createScreenshot() function.
    1. gituser: the github.com username that belongs to you
7. enable tab completion: add `complete -C zet zet` to your bashrc

### Dependencies

* Bash version 4 or higher is required
* [fzf]
* [fd]
* [rg]
* [sed]
* [gawk]
* [imagemagick]
* [xclip]
* [git]
* [gnome-screenshot]

## 🙏 Thanks to 

> 📝 My old `zet` command simply created a file and filled it with a markdown
template. It had an isosec stamp at the beginning of the title. I also just
used it as a local private git repository. But ever since rwxrob developed his
`zet` command, I've been using it and loving it. Back when I was super hyped
about the Zettelkasten system but couldn't transfer the analog system to
a digital version myself. I tried to inspire/persuade him to develop a command
on how to implement and design a digital Zettelkasten, and he did. That meant
I could abandon my own first attempt. That one failed largely because my bash
and general programming skills just weren't up to it at the time.

A big thanks to [rwxrob] and his idea on how to manage a digital Zettelkasten
with isometric seconds in Coordinated Universal Time UTC.
I also took the liberty of copying a few snippets here and there into my own
`zet` command. I tried to keep this to a minimum. But I liked the overall
design of rwxrob's command, so the core ideas and design are the same. I just
implemented 95% of the code base in my own way. I've been using rwxrobs' `zet`
command since he created it, but a lot of the features and things he
implemented I didn't need. So I'm glad I now have a command that gets rid of
all the clutter and only does what I need.

Here's a list of some of the things and ideas I adopted. Some I had to modify
quite a bit to make them work in my command.

- Design decisions: It is a good idea to name folders with an isosec timestamp
  and create README.md files in them. Especially if they are used on a public
  git hosting service like github.com. So I decided to incorporate this into my
  current `zet` command. I also welcome the idea of having two separate zet
  repos, one public and one private. It is a great idea to share technical
  knowledge with the Internet. Knowledge wants to flow freely, and the more
  people who embrace this idea, the better for all of us. Also, if you have
  your Zettels (notes) hosted on github.com, you can use github's search
  feature, which makes it easier to find things. However, for more personal
  notes, I still prefer to have a second Zettelkasten that is private.
- Tab completion/main loops: I had to modify the main loop that executes
  commands depending on user input, and the tab completion loop. I used
  associative arrays, which is the main difference, and a case that checks if
  \$1 user input is a flag/option. My version differs quite a bit. However, the
  idea of how to implement tab completion in a script is definitely not mine.
  I might have come up with something, but I doubt it would look like this
  implementation.
- \_newest() this function gets the last created file from a folder. I created
  \_\_editLastModifiedReadme() which works similarly by retrieving the last
  readme file, but requires two loops and adds a lot more overhead. I created
  a command yesterday that was just as simple as Rob's idea. But unfortunately
  I didn't commit the changes and at some point I deleted it again. Needless to
  say, I can't remember how I did it.
- \_isoSec() using isosec is a core design decision of my current `zet`
  command. It helps a lot to manage a digital Zettelkasten (notebox). I also
  had the idea of capturing a timestamp when I started playing around with my
  very first version of the Zettelkasten a couple of years ago. However, I put
  a timestamp at the beginning of a title and mine wasn't a UTC timestamp
  either.  
- \_\_create() This function is very simple, and even if I had written it
  myself without ever looking at Rob's command, the result would have been
  something very similar. There just aren't that many ways to get user input,
  create variables, and print them to a file.

## Challenges

* integration of fzf and how to let fzf search not only for titles but also for
  tags at the same time
* make zet commands that require fzf work in vim with !!. E.g. `!!zet cat`.
  Inside a tmux session it was easy, but it was a challenge to do this without
  tmux running.
* creating a command to edit the last modified README.md `zet last`
* creating a command that looks for titles and tags and prints them `zet link
  <pattern>`
* changing code snippets from rwxrob to make it work in my script. The most
  difficult part was to change rwxrob's main command to execute user input and
  tab completion.
* manage flow and organize a larger script

## License

Copyright 2022 Simon D. Woodtli <simonwoodtli@posteo.net>  
Released under Apache-2.0 License  

## Stats

![Alt](https://repobeats.axiom.co/api/embed/87f4865318b3d00fa3ff94a8bc9dce89fffbbc96.svg "Repobeats analytics image")

[rwxrob]: <https://github.com/rwxrob/>
[Install]: <#installation>
[How to use]: <#how-to-use>
[dependencies]: <#dependencies>
[fzf]: <https://github.com/junegunn/fzf>
[fd]: <https://github.com/sharkdp/fd>
[rg]: <https://github.com/BurntSushi/ripgrep>
[sed]: <https://pkgs.org/search/?q=sed>
[gawk]: <https://pkgs.org/search/?q=gawk>
[imagemagick]: <https://pkgs.org/search/?q=imagemagick>
[xclip]: <https://pkgs.org/search/?q=xclip>
[git]: <https://pkgs.org/search/?q=git>
[gnome-screenshot]: <https://pkgs.org/search/?q=gnome-screenshot>
