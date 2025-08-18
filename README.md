# Ikala Asset Editor

This is an editor and visualizer for assets used by the Ikala engine.
The Ikala engine and plugins that are used to create it are also open source. Please see their corresponding repositories for more information.

## Building

In order to build and run this project, some other repositories are going to be required. It is expected that this repo be installed in the same directory as the following:

* [Ikala-Core](https://github.com/Maceris/Ikala-Core)
* [Ikala-Plugins](https://github.com/Maceris/Ikala-Plugins)

If you wish to install these in another location, that location can be configured in gradle.properties.

To build the program, run `./gradlew build`. This will build all the dependent projects and install the jars in the plugins folder.

The `./gradlew clean` command will delete all the jars and the log folder, but leave plugin config, scripts, and data alone.

## Running

Once the build has been run, `./openEditor` will open the editor.
