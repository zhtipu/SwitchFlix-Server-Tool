# SwitchFlix Server Tool

A free Windows program that reads your ISP's media server, tests it the way the SwitchFlix app uses it, and writes a small file for your Switch's SD card. No typing long addresses on the Switch.

Companion to [SwitchFlix](https://github.com/zhtipu/switchflix).

## Install

Download `SwitchFlix-Server-Tool.zip` from the latest release, unzip it anywhere and open `SwitchFlix-Server-Tool.exe` inside the folder. Keep the files next to it (the `_internal` folder is part of the program). It starts faster than a single-file program and updates itself from the About tab.

## What it does

1. Type the server's address (and a user name and password if it asks for them) and press **Analyze**.
2. The tool works out what kind of server it is and tests what the app needs: listing titles, posters, and opening a stream.
3. Press **Write to SD card** and pick the root of the card. It creates `switch/SwitchFlix/servers/<name>.json`.
4. Put the card in the Switch and open SwitchFlix. The server appears by itself. (Settings, Sources, **Load servers from the SD card** reloads it later.)

## Servers it understands

| Kind | What the tool checks |
|---|---|
| Emby | version, sign in (finds an account without a password), libraries, titles, posters, a stream, the HTTPS certificate |
| Ovoo movie portal | movie and TV lists, genre filter, a watch page, the stream (HLS: video versions, audio tracks), subtitles |
| Folder index (Apache, nginx, h5ai) | folders and video files, that a file opens, that seeking works |
| WebDAV | folder listing |

Movie portals that are none of these are **worked out from the site itself**: the tool looks for the repeated title cards, the paging, the title page, the video address, episodes and search, writes them as a *profile*, then reads the site with that profile (the same reader the app has) to prove it: lists, a title page, a video that really opens. A server counts as ready only after that proof. The profile travels inside the server file, and the app keeps it in `/switch/SwitchFlix/profiles/`. This covers sites with server-built pages, a plain video address per title and no login. Sites that build pages only with scripts, need a login, or hide the video still go to the problem report.

Anything else cannot be used yet. Press **Save problem report**: the tool saves one small zip file with the structure of the site (page markup, the parts of the scripts that talk to the server, answers of a few addresses, the start of a stream playlist). User names, passwords, tokens, cookies and e-mail addresses are never put in it, and the server's domain name is replaced unless **Include the server address** is ticked. Open `diagnostic.json` inside the zip to see exactly what it holds. Attach the zip to a new issue at [github.com/zhtipu/switchflix/issues](https://github.com/zhtipu/switchflix/issues) and support for that kind of server can be added.

## Notes

- A server is only called **Ready for SwitchFlix** when a video really opens: titles and folders alone are not enough. If the tool lists titles or folders but cannot reach a video, it says so and offers the problem report.
- The tool accepts expired or self-made HTTPS certificates, like the app does.
- A server file is read once by the app and renamed to `.json.done`, so a server you remove in the app does not come back.
- **Save server file** writes the same file anywhere, to copy over by hand to `/switch/SwitchFlix/servers/`.
