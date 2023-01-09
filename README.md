# Fisher

> A plugin manager for [Fish](https://fishshell.com)—the friendly interactive shell. [Looking for plugins?](https://git.io/awesome.fish)

Manage functions, completions, bindings, and snippets from the command line. Extend your shell capabilities, change the look of your prompt and create repeatable configurations across different systems effortlessly.

- 100% _pure_-Fish—easy to contribute to or modify.
- Blazing fast concurrent plugin downloads.
- Zero configuration out of the box.
- Oh My Fish! plugin support.

> #### ☝️ [Upgrading from Fisher `3.x` or older?](https://github.com/jorgebucaran/fisher/issues/652)

## Installation

```console
curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher
```

## Quickstart

You can install, update, and remove plugins interactively with Fisher, taking advantage of Fish [tab completion](https://fishshell.com/docs/current/index.html#completion) and rich syntax highlighting.

### Installing plugins

Install plugins using the `install` command followed by the path to the repository on GitHub.

```console
fisher install jorgebucaran/nvm.fish
```

> To install from GitLab prepend `gitlab.com/` to the plugin path.

To get a specific version of a plugin add an `@` symbol after the plugin name followed by a tag, branch, or [commit](https://git-scm.com/docs/gitglossary#Documentation/gitglossary.txt-aiddefcommit-ishacommit-ishalsocommittish).

```console
fisher install IlanCosman/tide@v5
```

You can install plugins from a local directory too.

```console
fisher install ~/path/to/plugin
```

> Fisher expands plugins into your Fish configuration directory by default, overwriting existing files. If you wish to change this behavior, set `$fisher_path` to your preferred location and put it in your function path ([#640](https://github.com/jorgebucaran/fisher/issues/640)).

### Listing plugins

List all the plugins that are currently installed using the `list` command.

```console
$ fisher list
jorgebucaran/fisher
ilancosman/tide@v5
jorgebucaran/nvm.fish
/home/jb/path/to/plugin
```

The `list` command also accepts a regular expression to filter the output.

```console
$ fisher list \^/
/home/jb/path/to/plugin
```

### Updating plugins

The `update` command updates one or more plugins to their latest version.

```console
fisher update jorgebucaran/fisher
```

> Use just `fisher update` to update everything.

### Removing plugins

Remove installed plugins using the `remove` command.

```console
fisher remove jorgebucaran/nvm.fish
```

You may want to remove everything, including Fisher.

```console
fisher list | fisher remove
```

## Using your `fish_plugins` file

Whenever you install or remove a plugin from the command line, Fisher will write down all the installed plugins to `$__fish_config_dir/fish_plugins`. Adding this file to your dotfiles or version control is the easiest way to share your configuration across different systems.

You can also edit this file and run `fisher update` to commit changes:

```console
nano $__fish_config_dir/fish_plugins
```

```diff
jorgebucaran/fisher
ilancosman/tide@v5
jorgebucaran/nvm.fish
+ PatrickF1/fzf.fish
- /home/jb/path/to/plugin
```

```console
fisher update
```

That will install **PatrickF1**/**fzf.fish**, remove /**home**/**jb**/**path**/**to**/**plugin**, and update everything else.

## Creating a plugin

A plugin can be any number of files in a `functions`, `conf.d`, and `completions` directory. Most plugins consist of a single function, or [configuration snippet](https://fishshell.com/docs/current/index.html#configuration-files). This is what a typical plugin looks like.

<pre>
<b>ponyo</b>
├── <b>completions</b>
│   └── ponyo.fish
├── <b>conf.d</b>
│   └── ponyo.fish
└── <b>functions</b>
    └── ponyo.fish
</pre>

Non `.fish` files as well as directories inside those locations will be copied to `$fisher_path` under `functions`, `conf.d`, or `completions` respectively.

## Creating a theme

A theme is just like a regular Fish plugin, except it has a `.theme` file in the `themes` directory. Themes were introduced in [Fish 3.4](https://github.com/fish-shell/fish-shell/releases/tag/3.4.0) and work with the `fish_config` builtin. A theme can also have files in `functions`, `conf.d`, or `completions` if necessary. This is what a typical theme plugin might look like.

<pre>
<b>sosuke</b>
├── <b>conf.d</b>
│   └── sosuke.fish
└── <b>themes</b>
    └── sosuke.theme
</pre>

## Using `$fisher_path` with themes

If you customize `$fisher_path` to use a directory other than `$__fish_config_dir`, your themes won't be available via `fish_config`. That's because Fish expects your themes to be in `$__fish_config_dir/themes`, not `$fisher_path/themes`. This is not yet configurable in Fish, but there is [a request to add that feature](https://github.com/fish-shell/fish-shell/issues/9456).

This problem can easily be solved by symlinking Fisher's `themes` directory into your Fish config. First, backup any existing themes directory.

```console
mv $__fish_config_dir/themes $__fish_config_dir/themes.bak
```

Then, symlink Fisher's themes directory.

```console
ln -s $fisher_path/themes $__fish_config_dir/themes
```

If you want to use theme plugins and also maintain your own local themes, you can read more about how to do that here: [#708](https://github.com/jorgebucaran/fisher/issues/708).

### Event system

Plugins are notified as they are being installed, updated, or removed via Fish [events](https://fishshell.com/docs/current/cmds/emit.html).

> `--on-event` functions must already be loaded when their event is emitted. Thus, you should put your event handlers in the `conf.d` directory.

```fish
# Defined in ponyo/conf.d/ponyo.fish

function _ponyo_install --on-event ponyo_install
    # Set universal variables, create bindings, and other initialization logic.
end

function _ponyo_update --on-event ponyo_update
    # Migrate resources, print warnings, and other update logic.
end

function _ponyo_uninstall --on-event ponyo_uninstall
    # Erase "private" functions, variables, bindings, and other uninstall logic.
end
```

### Discoverability

Fisher doesn't make use of a central repository of plugins. However, that doesn't mean discovering new plugins should be hard. A great way to help people quickly find your project on GitHub is [by adding a topic to your repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics#adding-topics-to-your-repository). We recommend using [`fish-plugin`](https://github.com/topics/fish-plugin) for this purpose.

## Acknowledgments

Fisher started out in 2016 by [@jorgebucaran](https://github.com/jorgebucaran) as a shell configuration manager for Fish. We had a lot of help along the way. [Oh My Fish](https://github.com/oh-my-fish/oh-my-fish) laid the groundwork as the first popular Fish framework. [@jethrokuan](https://github.com/jethrokuan) was particularly helpful during the first years. [@PatrickF1](https://github.com/PatrickF1)'s candid feedback has been invaluable time and again. Bootstrapping Fisher was originally [@IlanCosman](https://github.com/IlanCosman)'s idea. Thank you to all our contributors! <3

## License

[MIT](LICENSE.md)
