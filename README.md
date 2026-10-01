this client is hacked lol kiss me you mother fucker
scull client is better use it the utilclient is prototype
A modular utility client for Minecraft 1.21.1 (Fabric). Modules are organized as Category > Module > Setting, with a ClickGUI, an HUD, and saved settings.
Source was syntax-checked but has not been compiled against Minecraft or run in-game by the author. See "Troubleshooting" if anything fails.
Requirements
Minecraft Java Edition 1.21.1
Fabric Loader 0.16.x
Fabric API 0.104.0+1.21.1 (or a newer 1.21.1 build)
JDK 21 (only needed to build)
Build the jar
Install JDK 21.
Open the scull-client folder in IntelliJ IDEA (it imports Gradle automatically), or use a terminal:
Code
Your jar is build/libs/scullclient-1.0.0.jar (not the -sources jar).
To test without installing: ./gradlew runClient.
Install
Install Fabric Loader for 1.21.1 and put Fabric API in .minecraft/mods.
Copy scullclient-1.0.0.jar into .minecraft/mods.
Launch the Fabric profile.
Controls
Key
Action
Right Shift
Open the ClickGUI
R
KillAura
J
ESP
X
XRay
B
Fullbright
G
Scaffold
V
Sprint
F
Fly
Z
Speed
In the ClickGUI: left-click a module to toggle it, right-click to expand its settings. Bool settings toggle on click, mode settings cycle, number settings go up with left-click and down with right-click. Toggling by key shows ON/OFF above the hotbar.
Modules
Combat
KillAura / TriggerBot: target priority (distance, health, angle), FOV and line-of-sight checks, attack on cooldown or CPS window, smoothed rotation. TriggerBot mode only hits what your crosshair is on.
AutoArmor: equips the best armor in your inventory by protection, toughness and enchantments.
AutoHeal: eats golden apples or stews when health drops below a threshold, then restores your hotbar slot.
Render
ESP: boxes or glow outlines, plus tracers.
XRay: hides every block not on the whitelist (ores, chests, portals, spawners by default).
Fullbright: gamma override or client-side night vision.
HUD: watermark and active-module list.
Movement / World
Scaffold: places blocks under your feet with edge detection, rotation toward the placed face, and randomized place delay.
Sprint, Fly (creative or velocity), Speed (capped multiplier).
Tip: XRay + Fullbright makes ores visible in dark caves.
Config
Keybinds and settings are saved to .minecraft/config/utilclient.json when you close the ClickGUI or quit the game. Delete the file to reset to defaults. Enabled/disabled state is not saved.
Stability design
Modules are created only after the client has fully started.
Every callback is wrapped. A module that throws is switched off with a red chat message instead of crashing the game.
Mixins use defaultRequire: 0 and required: false: if a target changed in your version, only that feature stops working and a warning is logged.
Only three mixins (XRay, ESP glow, Fullbright gamma).
ESP restores graphics state in a finally block.
Troubleshooting
Build error: a Minecraft method name differs in your version. Note the file and line; these are small fixes.
Game crash or a feature does nothing: check .minecraft/logs/latest.log (or crash-reports/). Lines starting with [UtilClient] name the module.
XRay shows nothing or glitches: toggle it off and on to rebuild chunks.
Mod not loading: confirm Minecraft 1.21.1, Java 21, Fabric Loader 0.16+, and Fabric API installed.
Project layout
Code
Add a module: extend Module, annotate with @ModuleInfo(name, category, key), add settings with add(...), subscribe with @Subscribe methods, and register it in ModuleManager.init().
Fair-play note
Meant for singleplayer, your own server, or servers that allow client modifications. Most public servers prohibit features like these and will ban accounts that use them.gradle wrapper --gradle-version 8.10    # one time, needs Gradle installed
./gradlew build                         # Windows: gradlew.bat buildcore/     Module, ModuleInfo, ModuleManager, Category, Config
setting/  Bool, Number, Mode, BlockList settings
event/    EventBus and Tick / RenderWorld / RenderHud / Packet events
module/   combat, render, movement, world
mixin/    BlockMixin (XRay), EntityMixin (ESP glow), SimpleOptionMixin (Fullbright)
ui/       ClickGuiScreen
