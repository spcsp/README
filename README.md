# StrokesPlus.net - ClearScript Plugins

[StrokesPlus.net](https://www.strokesplus.net/) is an amazing piece of software that can automate//query/interacte with most parts of windows and applications. Included with the support for gestures, hotkeys, macros, floaters, is the a JS engine, [ClearScript](https://github.com/Microsoft/ClearScript). StrokesPlus uses ClearScript to embed a JS engine in the app, and provide a whole host of custom functionality, as well as access to a large portion of the .NET core libraries. (Things like _System.IO_ for example)

With some tinkering, we have leveraged [npmjs](https://npmjs.org) and the `npm` package manager to create a plugin ecosystem. They will be downloaded via npm, and installed locally. Part of the loading the plugin will be to load the plugins from disk, and make them avaiable to the scripting engine.

## SPPM - StrokesPlus Package Manager

First you will need the plugin to have some fun. [Download here.](https://github.com/spcsp/sppm)
Check the plugin's page for more detailed information, but let's use `SPPM` to install a plugin!

The root folder that houses all our plugins is located in StrokesPlus's `APPDATA` folder, located at `C:\Users\YOU\AppData\Roaming\StrokesPlus.net\`

> TIP: You can open the run box with <key>WIN</key>+<key>R</key> and type `shell:APPDATA` to open explorer right to `C:\Users\YOU\AppData\Roaming\`

```javascript
function sppm(command, noWindow = true, waitForExit = true) {
  this.APPDATA = StrokesPlus.OS.Shell.ExpandEnvironmentVariables("%APPDATA%");
  this.SP_APPDATA = System.IO.Path.Combine(this.APPDATA, "StrokesPlus.net");
  this.NODE_MODULES = System.IO.Path.Combine(this.SP_APPDATA, "node_modules");

  const NPM = "C:\\Program Files\\nodejs\\npm.cmd";
  const CMD = `cd "${this.SP_APPDATA}" && "${NPM}" ${command}`;

  return StrokesPlus.OS.Shell.RunProgram(
    "cmd.exe",
    "/C " + CMD,
    "",
    noWindow ? "hidden" : "normal",
    true,
    noWindow,
    waitForExit
  );
}
```

now we can leverage our new function to install some plugins!

### Official Plugins

To install a plugin into StrokesPlus

- [osd-toast](https://github.com/spcsp/osd-toast)
- [explore-settings-json](https://github.com/spcsp/explore-settings-json)
