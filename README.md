# Ikala Asset Editor

This is an editor and visualizer for assets used by the Ikala engine.
The Ikala engine and plugins that are used to create it are also open source. Please see their corresponding repositories for more information.

Binary files are not generally available as they are tracked through git LFS.

## Building

In order to build and run this project, some other repositories are going to be required. It is expected that this repo be installed in the same directory as the following:

* [Ikala-Core](https://github.com/Maceris/Ikala-Core)
* [Ikala-Plugins](https://github.com/Maceris/Ikala-Plugins)

If you wish to install these in another location, that location can be configured in gradle.properties.

To build the program, run `./gradlew build`. This will build all the dependent projects and install the jars in the plugins folder.

The `./gradlew clean` command will delete all the jars, but leave plugin config, scripts, and data alone.

## Running

Once the build has been run, `bin/openEditor` will open the editor. It works from any folder, and options after its name go to Java, such as `bin/openEditor -Dikala.headless=true`.

The editor is never written to while it runs. Its settings, logs and reports go in a Home folder, and caches (built level of detail cells, compiled shaders) in a separate cache folder, both named after `app.yml`:

| OS | Home | Cache |
| --- | --- | --- |
| Windows | `%APPDATA%\Ikala Asset Editor` | `%LOCALAPPDATA%\Ikala Asset Editor\cache` |
| Linux | `~/.local/share/ikala asset editor` | `~/.cache/ikala asset editor` |
| macOS | `~/Library/Application Support/Ikala Asset Editor` | `~/Library/Caches/Ikala Asset Editor` |

`-Dikala.home=<dir>` and `-Dikala.cache=<dir>` move them.
