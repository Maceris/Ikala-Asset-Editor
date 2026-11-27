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

## Check Scripts

`plugins/Asset-Converter/scripts/checks/` holds scripts that drive the editor as a person would and log each step as `Script: ...`. Run one at startup with `-Dikala.test.script=<file>`, its arguments in `-Dikala.test.scriptArgs=<text>`, and `-Dikala.test.exitAfterScript=true` to end the run with the script: the editor shuts down when it finishes, exiting with 1 if a UI check in it failed, else 0. Without that flag the editor stays open afterwards.

```
java -Dikala.test.exitAfterScript=true -Dikala.test.script=plugins/Asset-Converter/scripts/checks/workbench_subgraph.iks -Dikala.test.scriptArgs=<scratch folder> -jar Ikala-Core-0.6.0.jar
```

## Converting Assets

`bin/convertAsset` converts assets between the binary `.ika` form games load and text, without opening the editor. Materials only for now: `stone.ika` becomes `stone.material.yml` beside it, and back. Give several files to convert each, or `-o <file>` to name the output of one:

```
bin/convertAsset stone.material.yml
bin/convertAsset stone.ika -o text/stone.material.yml
```

The text form is described in Ikala-Graphics' `MaterialText`.

## Building Textures

`bin/buildTextures` builds a game's textures from their sources: every texture graph and instance under the sources folder bakes its PNG maps, then every texture's PNG maps there, baked or painted, become one `.ika` under the shipped folder. Files whose pixels wouldn't change are left alone. `--uses` bakes only the graphs that read a shared parameter file:

```
bin/buildTextures ../Lotomation/art/textures ../Lotomation/plugins/Lotomation/data/textures
bin/buildTextures ../Lotomation/art/textures ../Lotomation/plugins/Lotomation/data/textures --uses ../Lotomation/art/textures/block/wear.texparams.yml
```

## Baking Textures

`bin/bakeTextures` bakes texture graphs (`*.texgraph.yml`, made in the workbench's Graph panel) to the PNG maps the game reads, without opening the editor. Give it graphs or folders; each graph bakes to the content folder it names, or to the one `--data` gives. Maps whose pixels wouldn't change are left alone:

```
bin/bakeTextures ../Lotomation/art/textures/block
bin/bakeTextures --data /tmp/data stone.texgraph.yml
```
