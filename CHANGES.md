# Change Log / Release Notes

## 0.13 (Sometime Early 2017)

### Breaking Changes

  * When restarting xmonad, resume state is no longer passed to the
    next process via the command line.  Instead, a temporary state
    file is created and xmonad's state is serialized to that file.

    When upgrading to 0.13 from a previous version, the `--resume`
    command line option will automatically migrate to a state file.

    This fixes issue #12.

### Enhancements

  * You can now control which directory xmonad uses for finding your
    configuration file and which one is used for storing the compiled
    version of your configuration.  In order of preference:

      1. New environment variables.  If you want to use these ensure
         you set the correct environment variable and also create the
         directory it references:

         - `XMONAD_CONFIG_DIR`
         - `XMONAD_CACHE_DIR`
         - `XMONAD_DATA_DIR`

      2. XDG Base Directory Specification directories, if they exist:

         - `XDG_CONFIG_HOME/xmonad`
         - `XDG_CACHE_HOME/xmonad`
         - `XDG_DATA_HOME/xmonad`

      3. The `~/.xmonad` directory.

    If none of these directories exist then an appropriate directory
    will be created.  If the relevant environment variable mentioned
    in step (1) above is set, the referent directory will be created
    and used.  Otherwise the relevant XDG directory from (2) will be
    used.

    Note: If the environment variables mentioned in (1) above are not
    set, the directories in (2) don't exist, and `~/.xmonad` does
    exist, the traditional behavior of using `~/.xmonad` for
    everything will be used.

    This fixes a few issues, notably #7 and #56.

  * A custom build script can be used when xmonad is given the
    `--recompile` command line option.  If an executable named `build`
    exists in the xmonad configuration directory it will be called
    instead of `ghc`.  It takes one argument, the name of the
    executable binary it must produce.

    Note: the build script is called whenever `ghc` would have been
    called.  That means the `xmonad.hs` file (or any files in `lib`
    need to have newer time stamps than the generated executable in
    order for the build script to be called.

    This fixes #8.  (One of two possible custom build solutions.  See
    the next entry for another solution.)

  * For users who build their xmonad configuration using tools such as
    cabal or stack, there is another option for executing xmonad.

    Instead of running the `xmonad` executable directly, arrange to
    have your login manager run your configuration binary instead.
    Then, in your binary, use the new `launch` command instead of
    `xmonad`.

    This will keep xmonad from using its configuration file
    checking/compiling code and directly start the window manager
    without `exec`ing any other binary.

    See the documentation for the `launch` function in `XMonad.Main`
    for more details.

    Fixes #8.  (Second way to have a custom build environment for
    XMonad.  See previous entry for another solution.)

## 0.12 (December 14, 2015)

  * Compiles with GHC 7.10.2, 7.8.4, and 7.6.3

  * Use of [data-default][] allows using `def` where previously you
    had to write `defaultConfig`, `defaultXPConfig`, etc.

  * The [setlocale][] package is now used instead of a binding shipped
    with xmonad proper allowing the use of `Main.hs` instead of
    `Main.hsc`

  * No longer encodes paths for `spawnPID`

  * The default `manageHook` no longer floats Gimp windows

  * Doesn't crash when there are fewer workspaces than screens

  * `Query` is now an instance of `Applicative`

  * Various improvements to the example configuration file

[data-default]: http://hackage.haskell.org/package/data-default
[setlocale]: https://hackage.haskell.org/package/setlocale
