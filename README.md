# Fork de yt-x

Browse YouTube from your terminal.
Plus other sites yt-dlp supports.

## Features

-   **Interactive Menu**: Text-based UI using `fzf` or `rofi` for seamless navigation.
-   **YouTube-Specific Menus**: Access your feed, trending videos, playlists, watch later, subscriptions feed, liked videos, clips.
-   **Playback Support**: Play videos and audio via `mpv` or `vlc`.
-   **Search Functionality**: Search for videos, channels and playlists directly.
-   **Channel Exploration**: Explore channels, including their videos, streams, podcasts, shorts, and playlists.
-   **Saved Channels**: Bookmark your favorite channels for quick access, with support for importing existing subscriptions.
-   **Saved Videos**: Save videos to watch later.
-   **Mixes**: Generate and explore YouTube song mixes.
-   **Yt-x Shell:** Run custom yt-dlp and mpv commands for downloading and viewing videos and playlists
-   **Custom Playlists**: Save playlists for easier access.
-   **Download Management**: Download videos, audio, and playlists using `yt-dlp`.
-   **History & Recents**: Track your recent videos and search history.
-   **Configuration Management**: Customize and manage configurations for yt-x, mpv and yt-dlp with ease.
-   **Extensions:** Extend yt-x with your own custom ui and preview logic allowing more precise coverage of other sites that yt-dlp supports🥳
-   **Custom Commands:** Basically a simple way to achieve the same thing with extensions. A custom command is just a yt-dlp command that loads a playlist or playlist like json.
-   **Miscellaneous Features**:
    -   Shell completions for `bash`, `zsh`, and `fish`.
    -   Desktop entry generation for easy access.

## 📥 Installation

![Linux/BSD](https://img.shields.io/badge/-Linux/BSD-red.svg?style=for-the-badge&logo=linux) <a href="#arch-linux" target="_blank"> <img src="https://img.shields.io/badge/-Arch_Linux-black.svg?style=for-the-badge&logo=archlinux" alt="Arch Linux"> </a> ![MacOS](https://img.shields.io/badge/-MacOS-lightblue.svg?style=for-the-badge&logo=apple) ![Android](https://img.shields.io/badge/-Android-green.svg?style=for-the-badge&logo=android)

### ❄️ NixOS or Home Manager

### <samp>On NixOS, you can install packages using two main methods:</samp>

1. **Imperative/Direct installation**:

```bash
nix profile install github:Benexl/yt-x
```

#

2. **Declarative/Config-based**:

    2.1 Add the following to your `flake.nix`:

    ```nix
    inputs = {
      nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
      yt-x = {
        url = "github:Benexl/yt-x";
        inputs.nixpkgs.follows = "nixpkgs";
      };
      ...
    }
    ```

    2.2 Then, add Yt-x to your packages:

    > For system wide installation in _configuration.nix_

    ```nix
    environment.systemPackages = with pkgs; [
      inputs.yt-x.packages."${system}".default
    ];
    ```

    > For user level installation in _home.nix_

    ```nix
    home.packages = with pkgs; [
      inputs.yt-x.packages."${system}".default
    ];
    ```

### Arch Linux

![AUR Version](https://img.shields.io/aur/version/yt-x-git?style=for-the-badge&color=e64553&logo=arch-linux&label=%5BAUR%5D%20yt-x-git&logocolor=85e185&labelColor=000000)

**You can install [`yt-x`](https://aur.archlinux.org/packages/yt-x-git) from the Arch Linux [AUR](https://aur.archlinux.org/) repository.**

**To install, use your preferred package manager [`paru`](https://aur.archlinux.org/packages/paru-bin) or [`yay`](https://aur.archlinux.org/packages/yay-bin):**

```bash
# for paru users
paru -S yt-x-git

# for yay users
yay -S yt-x-git
```

### Cross-platform

```bash
# NOTE: ~/.local/bin should exist and be in path for this to work
curl -sL "https://raw.githubusercontent.com/Benexl/yt-x/refs/heads/master/yt-x" -o ~/.local/bin/yt-x && chmod +x ~/.local/bin/yt-x
```

## Dependencies

### Required

-   [jq](https://github.com/jqlang/jq) - JSON parsing.
-   [curl](https://curl.se/) - Download preview images.
-   [yt-dlp](https://github.com/yt-dlp/yt-dlp) - Fetch YouTube data.
-   [fzf](https://github.com/junegunn/fzf) - Main UI navigation.
-   [mpv](https://mpv.io/) - Video and audio playback.
-   [ffmpeg](https://www.ffmpeg.org/) - Proper HLS stream downloading.
-   [bash](https://www.gnu.org/software/bash/) - Script interpreter.
-   [nerdfont](https://www.nerdfonts.com/) - for the icons

### Optional

-   [gum](https://github.com/charmbracelet/gum) - Enhanced UI (highly recommended).
-   [rofi](https://github.com/davatorium/rofi) - Alternate UI.
-   **terminal image viewer:**
    -   [chafa](https://github.com/hpjansson/chafa) - Cross-terminal image rendering (recommended).
    -   [icat](https://sw.kovidgoyal.net/kitty/kittens/icat/) - recommended for kitty terminal and ghostty
    -   [imgcat](https://github.com/danielgatis/imgcat)
-   **terminal with image rendering support:**
    -   [kitty](https://sw.kovidgoyal.net/kitty/) - currently has the best image rendering capabilities (recommended)
    -   [wezterm](https://wezfurlong.org/wezterm/index.html)
    -   [ghostty](https://github.com/ghostty-org/ghostty)

---

## Usage

```bash
# Launch the UI
yt-x

# Edit configuration
yt-x -e

# load an extension
# extensions are located at ~/.config/yt-x/extensions
# the extension name is the name of a file in the extensions folder
yt-x -x <extension-name>

# Specify player at runtime
yt-x --player <mpv/vlc>

# Set selector at runtime
yt-x -s <fzf/rofi>

# Specify Rofi theme path
yt-x --rofi-theme <path>

# Enable/disable preview
yt-x --preview / yt-x --no-preview

# Print desktop entry
yt-x -E

# Print shell completions
yt-x completions --bash
yt-x completions --zsh
yt-x completions --fish

# Update the script
yt-x --update

# Display help
yt-x --help
```

---

## Tips

### Enabling Imports of Subscriptions & Private Playlists

Set your preferred browser in the configuration file:

```ini
PREFERRED_BROWSER: firefox
```

To enable `mpv` to access private playlists and videos, add something like this to `mpv.conf` (you can also use the ui to edit `mpv.conf`):

```ini
ytdl-raw-options=cookies-from-browser=firefox

# --- bonus mpv tips ---

# define the quality for mpv to use
ytdl-format="bestvideo[vcodec^=avc1][height=1080]+bestaudio/best[vcodec^=avc1][height=1080]/bestvideo[vcodec^=avc1][height=720]+bestaudio/best[vcodec^=avc1][height=720]/best"

# defines where screenshots will be saved
screenshot-directory=~/Pictures/mpv_screenshots/

# enable hardware accelaration
hwdec=auto
vo=gpu
```

To customise download options with yt-dlp you can add something like this to `yt-dlp.conf` (you can also use the ui to edit `yt-dlp.conf`)

```bash
-f bestvideo[vcodec^=avc1][height=1080]+bestaudio/best[vcodec^=avc1][height=1080]/bestvideo[vcodec^=avc1][height=720]+bestaudio/best[vcodec^=avc1][height=720]/best
--embed-chapters
--sponsorblock-mark all
--embed-metadata
--embed-thumbnail
--add-metadata
--embed-subs
--sub-lang en
--merge-output-format mkv
```

For additional enhancements, consider:

-   [uosc](https://github.com/tomasklaen/uosc) for a modern `mpv` UI.
-   [thumbfast](https://github.com/po5/thumbfast) for thumbnail timeline previews.

## Custom Playlists

Define custom playlists by editing `~/.config/yt-x/custom_playlists.json` (or use the UI):

```json
[
    {
        "name": "<playlist name>",
        "playlistUrl": "https://www.youtube.com/playlist?list=<playlist-id>",
        "playlistWatchUrl": "https://www.youtube.com/watch?list=<playlist-id>"
    }
]
```

## Theming

To change the default colorscheme, set `YT_X_FZF_OPTS` env var and give it custom fzf opts.

eg. (.bashrc)

```
#yt-x
export YT_X_FZF_OPTS=$FZF_DEFAULT_OPTS'
--color=fg:#e0def4,fg+:#e0def4,bg:#232136,bg+:#44415a
--color=hl:#3e8fb0,hl+:#9ccfd8,info:#f6c177,marker:#3e8fb0
--color=prompt:#eb6f92,spinner:#c4a7e7,pointer:#c4a7e7,header:#3e8fb0
--color=border:#44415a,label:#ea9a97,query:#f6c177
--border="rounded" --border-label="" --preview-window="border-rounded" --prompt="> "
--marker=">" --pointer="◆" --separator="─" --scrollbar="│"'
```
