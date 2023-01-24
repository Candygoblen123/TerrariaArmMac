# TerrariaArmMac
Precompiled files are available on the [releases](https://github.com/Candygoblen123/TerrariaArmMac/releases) page. This repo only contains code from terraria's open source dependencies, and contains no code copyrighted by Re-Logic.

# How to install

1. First things first, make a backup of your existing terraria install. You can find it by right clicking the game in Steam > Manage > Browse Local Files.

2. Download the latest release from the [Releases](https://github.com/Candygoblen123/TerrariaArmMac/releases) page

3. Unzip the Terraria Arm.zip file, there should be a folder inside called `Contents`

4. In your existing install of Terraria, right click `Terraria.app` and click `Show Package Contents`. You should see a folder called Contents, go into it.

5. The Directory structure of these 2 folders ahould corrilate. All you have to do is replace the existing files with the new files in the provided zip. (Note that you cannot merge the 2 folders, and that you cannot replace the entire folder. We still need all those other files!) For more detailed instructions, continue reading. 

6. Go into the MacOS folder, and replace `Terraria.bin.osx` and `TerrariaServer.bin.osx` with the versions provided in the Terraria Arm folder.

7. Go into the `osx/` folder, and replace `libFAudio.0.dylib`, `libSDL2-2.0.0.dylib`, `libtheorafile.dylib`, `libFNA3D.0.dylib`, `libSDL2_image-2.0.0.dylib` with the version provided in the Terraria Arm folder.

8. Go up back to the `Contents` Folder, go into the `Resources` folder, and replace `Mono.Posix.dll`, `System.Drawing.dll`, `System.Xml.Linq.dll`, `Mono.Security.dll`, `mscorlib.dll`, `System.Numerics.dll`, `System.Xml.dll`, `System.Configuration.dll`, `System.Runtime.Serialization.dll`, `System.dll`, `System.Core.dll`, `System.Security.dll`, `WindowsBase.dll`, `System.Data.dll`, `System.Windows.Forms.dll` with the version provided in the Terraria Arm folder.

9. You should now be able to open Terraria via steam, and it now should be running nativly on ARM. You can check via Activity Moniter

# How to uninstall
In steam, Right click terraria > Properties... > Local Files > Verify integrity of game files...

# Is there any benefit to doing this?
Yes! You'll see better frame rates throughout the game. In my experience the game stays locked at 60FPS most of the time, except during some late game boss fights.

# What has been tested
A playthough of the game via classic mode, multiplayer by joining a server, multiplayer by doing host and play.

# Does anything break?
Well, yes, but I would call these thingd pretty minor. There are 2 differences i've noticed: 
1. During Worldgen, desertification takes signifiantly longer running nativly than it does running on Rosetta. Upwards of a minute dedicated to desertification. No idea why. However, the rest of worldgen is much faster, so the time generating a world is about the same regardless of intel or arm. (Around 2 minutes for both when generating a large world)
2. When you close the game using the Exit button on the main menu, the game crashes. This doesn't occour when you close the game using the X button on the window. However, since the crash occurs when you want to exit the game anyway, I've elected to ignore it.

# So how is this possible?
Terraria uses a game framework called XNA, which means that it can be easily brought to new platforms using `mono` and the open source reimplementation of XNA, FNA. All I had to do to make the game run on arm is use an arm64 version of mono and recompile the native dependencies for arm64. As it turns out, the only closed source dependency that terraria has is `libsteam_api.dylib`. However, for some unknowable reason, Re-logic decided to include the arm64 version of that in the standard Terraria release. So on to the native dependencies:

As it turns out, Terraria only needs 6 of it's 17 native dependencies to run. Specifically, [libFAudio](https://github.com/FNA-XNA/FAudio), [libFNA3D](https://github.com/FNA-XNA/FNA3D/), [libSDL2](https://github.com/libsdl-org/SDL), [libSDL2-image](https://github.com/libsdl-org/SDL_image), [libtheorafile](https://github.com/FNA-XNA/Theorafile/), and libsteam_api. Compiling all of these was a breeze. I was able to quickly get terraria to run using my system install of mono.

However, requiring users to install mono wouldn't be the best UX in the world. If you open the `MacOS/Terraria` esecutable in a text editor, you'll find that it isn't a executable at all! It's actually a bash script from a project called [MonoKickstart](https://github.com/flibitijibibo/MonoKickstart), which is where the `Terraria.bin.osx` and `TerrariaServer.bin.osx` files come from. So i compiled that for arm64 as well, after compiling the latest release of mono using patches from [brew](https://github.com/Homebrew/homebrew-core/blob/master/Formula/mono.rb) to make it work.
