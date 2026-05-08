# Setting Up a Rojo Project

> **Prerequisites:** This guide assumes you've already installed Visual Studio Code, Rokit, the Visual Studio Code extensions, the Rojo plugins, and Git. If not, follow [this video](https://www.youtube.com/watch?v=IJDg6tRJmHo) first.

## 1. Initialize Rokit and Rojo

```bash
rokit init
rokit add rojo
rojo init
```

Remove everything under `Workspace` inside `default.project.json` unless you want Rojo to control instances.

### Generate Sourcemap

If `sourcemap.json` wasn't created after `rojo init`, generate it manually:

```bash
rojo sourcemap default.project.json --output sourcemap.json
```

## 2. Project Structure

Everything under `src/` is where you write code. File extensions determine script type:

| Extension      | Script Type           |
| -------------- | --------------------- |
| `.client.luau` | LocalScript           |
| `.server.luau` | Script (ServerScript) |
| `.luau`        | ModuleScript          |

## 3. Set Up Wally (Package Manager)

```bash
rokit add wally
rokit add wally-package-types
wally init
```

This creates a `wally.toml` file where you define your packages.

### Adding Packages

1. Browse the [Wally website](https://wally.run) and copy a package's install line
2. Paste it under the appropriate section in `wally.toml`:
   - `[dependencies]` — shared packages (synced to ReplicatedStorage, accessible by both client and server)
   - `[server-dependencies]` — server-only packages (synced to ServerScriptService)
3. Optionally use PascalCase for package names if you prefer
4. Run `wally install`

Example `wally.toml`:

```toml
[dependencies]
Networker = "leifstout/networker@0.3.1"

[server-dependencies]
ProfileService = "firebird702/profileservice@1.1.0"
```

### Syncing Packages with Rojo

Add the package folders to `default.project.json` so Rojo can recognize and sync them. Place them alongside their respective script locations:

```diff
 "ReplicatedStorage": {
   "Shared": {
     "$path": "src/shared"
   }
+  "Packages": {
+    "$path": "Packages"
+  }
 }
```

```diff
 "ServerScriptService": {
   "Server": {
     "$path": "src/server"
   }
+  "ServerPackages": {
+    "$path": "ServerPackages"
+  }
 }
```

### Generating Type Exports

Wally doesn't automatically export types from packages. After installing, run the command on each package folder that exists:

```bash
wally-package-types -s sourcemap.json Packages/
wally-package-types -s sourcemap.json ServerPackages/
```

## 4. Git Ignore

Add all package directories, sourcemap, and wally lockfile to your `.gitignore`:

```
Packages/
# any additional package directories (e.g. ServerPackages/)
sourcemap.json
wally.lock
```

## Sources

- [Wally — Package Manager for Roblox](https://wally.run/)
- [Wally GitHub Repository](https://github.com/UpliftGames/wally)
- [Rojo Documentation](https://rojo.space/)
- [Rokit GitHub Repository](https://github.com/rojo-rbx/rokit)

---

This boilerplate guide is licensed under [MIT](https://opensource.org/licenses/MIT). Feel free to suggest changes or improvements.
