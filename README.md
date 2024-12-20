# JumpRoyale

![image](https://github.com/user-attachments/assets/df7ca981-c375-4c01-a6ac-258529acaa60)

A platformer controlled through Twitch chat to be played on my stream during short breaks.

-   [JumpRoyale](#jumproyale)
    -   [How to play](#how-to-play)
        -   [Tips](#tips)
    -   [Customizing your character](#customizing-your-character)
    -   [Extras](#extras)
    -   [Background](#background)
    -   [Building, running, testing, development](#building-running-testing-development)
        -   [Testing](#testing)
        -   [Development](#development)
        -   [Adding a new command](#adding-a-new-command)
    -   [Credits](#credits)

---

## How to play

The only way to play is for Adam to be streaming the game. At that point, you can use the following commands in his [Twitch chat](https://twitch.tv/AdamLearnsLive):

-   `join`: joins the game. Available at any time, although you'll only really stand a chance at winning if you join in the first ~45 seconds. 😉

To jump, you have to send one of the following commands in the chat: `l`, `u`, `r` (see full list aliases below). Optionally, you can adjust your angle and jump power.

Jump commands are currently accepted in the following format:

-   `<direction> [angle] [power]` - where default values for `angle` and `power` are `0` and `100` respectively
    -   `angle` can be from `-90` to `90`.
    -   `power` can be from `1` to `100`.

Both `angle` and `power` have a default value, so you don't have to type them in the chat. Omitting `angle` will make you jump up, omitting `power` will make you jump at maximum power. Commands were shortened to one character to make it easier to play.

Quick help:

-   `l` (aliases: `e`) - jump left, e.g. `l 45` or `e 45` to jump ↖
-   `r` (aliases: `j`, `i`) - jump right, e.g. `j 45` or `r 45` to jump ↗︎
-   `u` - jump up, **does not require `angle`**, but allows specifying `power` for weaker jumps, e.g. `u 50`

There are additional fixed-angle commands used as shortcuts:

-   `rrr`, `jjj`, or `iii`: same as `r 60` or `j 60`
-   `rr`, `jj`, or `ii`: same as `r 30` or `j 30`
-   `lll` or `eee`: same as `l 60`
-   `ll` or `ee`: same as `l 30`

| angle input    | -90 | -45 | 0   | 45  | 90  |
| -------------- | --- | --- | --- | --- | --- |
| jump direction | ←   | ↖   | ↑   | ↗︎  | →   |

> [!note]
> While `angle` is clamped between `-90` and `90`, that does not mean you have to put negative numbers in. This syntax was left in since `j` became an alias - you can use `j -30` to jump left or `j 30` to jump right! Sometimes it's more convenient to stay on the `j` key, so this might be a more preferred way to some players

To bypass duplicate message warning on Twitch, add some garbage letters after commands that you want to repeat:

-   `u`: jump up
-   `u a`: jump up again
-   `u bbbb`: jump up again

Jumping in the same direction also works with garbage letters:

-   `l 5`
-   `l 5 aaaa`
-   `l 5 bbb`

This also works if you add them right after the command name, but only without space:

-   `lk30` - interpreted as `l 30`
-   `rrr50` - interpreted as `rrr 50`

### Tips

> [!tip]
> Most important: you don't need to put `space` between the command and `angle`, you can simply send `l30` to jump 30 degrees to the left (mind the angle input above: `0` degrees means `up`)

Additional tips:

-   You can alternate between `l` and `r` commands to jump up in place, avoiding twitch duplicate message restriction. Jumping left or right with no angle specified allows you to jump up.
-   If you are using 7TV chat extension, it has its own duplicate message block prevention, so no additional garbage in the chat message is needed

---

## Customizing your character

Sending `char <choice>` in the chat allows you to change your character graphic. This is only for cosmetics and will be saved between the sessions.

> [!Note]
> Character customization is not saved during the Result Screen

-   `choice` is a number from `1` to `18`

![Characters](https://github.com/user-attachments/assets/48e7962f-ef9e-4093-8c45-fe65f2b09c89)

-   `glow [color]`: change your glow color. This is for subscribers, VIPs, and moderators only.
    -   If `color` isn't specified, it'll take the color of your Twitch name.
    -   `color` is in the form `RGB` or `RRGGBB`, e.g. `color f00` to set it to red, `color f0f` to set it to pink.
-   `namecolor <color>`: change your name color. This is for subscribers, VIPs, and moderators only.
    -   `Color names` are allowed, e.g. `red`, `yellow`, `tomato`. Refer to the [X11 Color Name Chart](https://en.wikipedia.org/wiki/X11_color_names#Color_name_chart) for color names.
    -   `Hexadecimal` values are also supported, e.g. `f0c`, `bd00cd`. Godot also supports `4`-`8` hex formats, but the `alpha` component is discarded, player name transparency is not allowed.
    -   `random` randomizes your name color.

---

## Extras

-   You can **revive** yourself using channel points. This isn't guaranteed to work, nor should you feel good about winning if you use this feature. 😛

---

## Background

-   Designed live (see [the design document](https://docs.google.com/document/d/1YoMtmxC9b5bVoKzm7LxIQ2DxAr3bq84uruarrXFOgQQ/edit))
-   Developed hastily in three days (although there'll probably be minor improvements made in the future... 👀)

---

## Building, running, testing, development

-   Prerequisites:
    -   Install [DotNet 8](https://dotnet.microsoft.com/en-us/download)
    -   Install [Godot ≥4.2](https://godotengine.org/download/windows/)
    -   Clone this repo
    -   Generate a twitch token for your twitch account: <https://twitchtokengenerator.com/>. You only need the scopes "chat:read" and "chat:edit" for now. Copy the **access token** and set it:
        -   `cd JumpRoyale` (i.e. your working directory should contain `JumpRoyale/JumpRoyale` and there should be `Jump Royale.csproj` in it)
        -   `dotnet user-secrets set twitch_access_token <your access token>`
        -   `dotnet user-secrets set twitch_channel_name <your channel name>`
        -   `dotnet user-secrets set twitch_channel_id <your channel id>`
    -   Ensure that you have a `GODOT4` environment variable:
        -   Windows: modify system properties to set the environment variable to something like `C:\myPath\Godot_vx.y.z-stable_mono_win64.exe`
        -   macOS: modify your shell's start-up script to add: `export GODOT4="/Applications/Godot_mono.app/Contents/MacOS/Godot"`
-   Building:
    -   Navigate to the root directory with `Jump Royale.sln`
    -   Run `dotnet restore`
    -   Rebuild solution:
        -   **VSCode**: Run `dotnet build --no-incremental` (or run `.NET: Rebuild` task if you have C# Dev Kit extension)
        -   **Visual Studio**: Build -> Rebuild Solution
    -   Open in Godot
    -   Click "Build" in Godot itself
-   Running:
    -   Same as "building", but click "Run" in Godot

> [!note]
> When developing with VSCode, if any changes were made in the `.editorconfig` file, you **need to** reload your window, because it will not pick up the changes immediately. Open `Command Palette` and run `Developer: Reload Window` task. This will most likely happen when switching to another branch that has different rules in `.editorconfig`, so you might see warnings from old cached config.
>
> On another note, you sometimes have to rebuild the solution for static analysis to detect the fixes (VSCode only).

### Testing

Refer to [Testing README](Testing.md)

### Development

With VSCode, we heavily recommend installing the following Microsoft extensions:

-   [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
-   Additionally, [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) is recommended for extra editor features: Testing, automatic Package Management and Project/Solution Explorer, but it is recommended to uninstall `IntelliCode for C# Dev Kit` from this pack; it only gets in the way.
-   Repository will recommend installing [CSharpier](https://marketplace.visualstudio.com/items?itemName=csharpier.csharpier-vscode) extension, we recommend installing this
    extension as a default formatter, because [the C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) only provides fixes for whitespaces and CSharpier
    can actually handle line breaks in a way that it makes sense. Don't install as a global tool (updates will cause conflicts and the extension will not know which version to use).

### Adding a new command

In `Arena.cs`:

-   In `HandleCommands`, make a new case, decide if it needs the `isPrivileged` argument (if the command is only for subs)
-   Create a new dictionary of aliases inside the provider
-   Make a new command-specific matcher inside the provider
-   Create a new method for the logic (no `defer`s needed!)

For example, assume there is a command that pushes a random player "left" or "right"
where the command format is: `push [direction] // ← random if null`

```cs
case string when CommandAliasProvider.MatchesPushCommand(command.Name):
    HandlePush(stringArguments[0]);
    break;
```

---

## Credits

-   Characters: <https://muchopixels.itch.io/character-animation-asset-pack>
-   World: <https://pixelfrog-assets.itch.io/pixel-adventure-1>
