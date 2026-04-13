<p align=center>
<br>
<a href="http://makeapullrequest.com"><img src="https://img.shields.io/badge/PRs-welcome-darkorange.svg"></a>
<img src="https://img.shields.io/badge/os-linux-darkorange">
<img src="https://img.shields.io/badge/os-windows-darkorange">
<br>
</p>

<h1 align="center">ani-skip<h1>

<p align="center">
<img src="https://media.tenor.com/CHVEROnz6hMAAAAC/asta-black-clover.gif">
</p>

<h3 align="center">
A script to automatically skip anime opening and ending sequences, making it easier to watch your favorite shows without having to manually skip the intros and outros each time.
</h3>

**Important:** There's a chance `ani-skip` may not recognize the anime you're watching. It leverages the [aniskip API](https://api.aniskip.com/api-docs). If an anime's episode(s) are missing, you can contribute or request its inclusion on their [discord server](https://discord.com/invite/UqT55CbrbE).

## Troubleshooting Errors

Should you run into problems, first ensure you're using the most recent version:

- For Linux:
  ```bash
  sudo ani-skip -U
  ```

- For Windows:
  Open Git Bash as an administrator and enter:
  ```bash
  ani-skip -U
  ```

If the issue remains unresolved, please create a new issue.

---

## Usage

```sh
ani-skip -h
```
```
    Usage:
    ani-skip [OPTIONS]

    Options:
      -q, --query
        Search query for anime title
      -i, --id
        Direct source ID (MyAnimeList or AllAnime ID)
      -e, --episode
        Specify the episode number
      -s, --source
        Source for ID/query resolution (myanimelist, allanime). 
        [default: myanimelist]
      -f, --filter
        Regex to filter search results for disambiguation (used with -q)
      --always
        Always skip 
        [default: skip once per session]
      --toggle
        Enable keybind in mpv to toggle skipping on/off 
        [default: 'a', set ANI_SKIP_TOGGLE_KEY to customize]
      --offset <seconds>
        End skip N seconds early to avoid missing episode content
        [max: 5, default: 0]
      -V, --version
        Show the version of the script
      -h, --help
        Show this help message and exit
      -U, --update
        Update the script

    Either -q or -i is required.

    Some example usages:
      ani-skip -i 52299 -e 5 # MAL ID directly
      ani-skip -i "ReooPAxPMsHM4KPMY" -s allanime -e 12 # AllAnime ID directly
      ani-skip -q "Solo Leveling" -e 3 # Search MAL (default)
      ani-skip -q "one piece" -s allanime -f "^1P$" -e 12 # Search AllAnime with filter
      ani-skip -q "Solo Leveling" -s allanime -f "Season 2" -e 1 # Season disambiguation
```

### Search MAL (default, backwards compatible)

```sh
ani-skip -q "Solo Leveling" -e 3
```
```
--chapters-file=/tmp/tempfile --script-opts=skip-op_start=130.531,skip-op_end=220.531,skip-ed_start=1326.58,skip-ed_end=1416.58
```

### Search AllAnime

AllAnime stores `malId` for each show, so querying it directly avoids the title mismatch issues that occur when anime have non-standard display names (e.g. "1P" for One Piece).

```sh
ani-skip -q "one piece" -s allanime -f "^1P$" -e 12
```

Use `-f` with a regex to disambiguate results (exact match, season selection, etc).

### Direct ID input

Pass a MAL or AllAnime ID directly to skip the search step entirely. This is the ideal path for [ani-cli](https://github.com/pystardust/ani-cli) integration since it already has the AllAnime `_id`.

```sh
# AllAnime ID
ani-skip -i "ReooPAxPMsHM4KPMY" -s allanime -e 12

# MAL ID
ani-skip -i 52299 -e 5
```

### Skip behavior

By default, openings and endings are skipped once — you can seek back to watch them again. Use `--always` to skip every time, `--toggle` to enable the `a` keybind in mpv for toggling skipping on/off, and `--offset` to end the skip a few seconds early. Set `ANI_SKIP_TOGGLE_KEY` to customize the keybind.

```sh
# Skip always, with toggle keybind, ending skip 3 seconds early
ani-skip -i 52299 -e 3 --always --toggle --offset 3
```

### Fetch MAL ID only (no episode)

Omit `-e` to just resolve and print the MAL ID.

```sh
ani-skip -q "Solo Leveling"
```
```
52299
```


## Install

- Linux
  > For Arch linux, ani-skip is available in the AUR as [ani-skip-git](https://aur.archlinux.org/packages/ani-skip-git).
  ```sh
  git clone https://github.com/synacktraa/ani-skip.git
  sudo apt install mpv fzf  
  sudo cp ani-skip/ani-skip /usr/local/bin
  mkdir -p ~/.config/mpv/scripts && cp ani-skip/skip.lua ~/.config/mpv/scripts
  ```
  
- Windows
  > Make sure [scoop](https://scoop.sh/) is installed.
  - Open powershell and run:
    ```powershell
    scoop install mpv fzf git
    ```
  - Open git bash
    ```sh
    git clone https://github.com/synacktraa/ani-skip.git
    cp ani-skip/ani-skip /usr/bin
    mkdir -p ~/scoop/apps/mpv/current/portable_config/scripts
    cp ani-skip/skip.lua ~/scoop/apps/mpv/current/portable_config/scripts
    ```

## Dependencies
- grep
- sed
- curl
- fzf
- mpv - Video Player

## Checklist

- [x] MPV support
- [x] MyAnimeList ID resolution
- [x] AllAnime ID resolution
- [ ] VLC support
- [ ] Create packages for Windows, Linux and Termux
- [ ] Test it on Android termux and Mac
