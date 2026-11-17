# ClayWind Chat

A chat client and server written in [CeeSafe](https://github.com/CeeSafe/CeeSafe). The client is built on
[ClayWind](https://github.com/CeeSafe/Clay), a Tailwind-style UI toolkit, as an outside app would be: it depends on
ClayWind's released packages only, and nothing in the toolkit was changed for it. It is the gate of ClayWind's app
runtime phase: what a real desktop app needs, and whether ClayWind gives it.

- `server/`: rooms of people talking over TCP, in the line protocol of [PROTOCOL.md](PROTOCOL.md). One task per
  connection under an async task set; the rooms and each room's last 50 messages are shared behind an `AsyncMutex`.
- `client/`: the desktop app. It connects to a server, joins `#lobby`, and shows each room you join beside the
  others; a message arriving while the window is idle wakes it, and the window uses no CPU while nothing happens.
  It depends on ClayWind's `claywind`, `claywind_desktop`, `claywind_fonts` and `claywind_tasks` packages, as git
  dependencies from the ClayWind repository's subdirectories.

## Running the server

Install the CeeSafe compiler (`ceesafec` 0.62.1 or later), then:

```sh
cd server && ceesafec build && ./out/chat_server 7700
```

Anything that speaks lines over TCP can talk to it, for example:

```sh
printf 'HELLO ada\nJOIN lobby\nSAY lobby hello\n' | nc 127.0.0.1 7700
```

## Running the client

The client needs `ceesafec` 0.62.1 or later (0.55.0 for what follows, 0.62.1 for a phone): its dependencies are packages in subdirectories of the ClayWind repository
(CeeSafe's ADR 0152), it shows times in the local time zone (ADR 0153), and on Windows 0.55.0 fixes fetching git
dependencies and a first connection. Then, with a server running:

```sh
cd client && ceesafec build && ./out/claywind_chat
```

Choose a name and press Enter.

## On a phone

The same client runs on an iPhone and an Android phone (ClayWind's mobile phase's gate). On a narrow screen the rooms
are a row above the room; the content keeps clear of the system's bars and ends above the soft keyboard; and the app
saves where it is each time it leaves the screen, so when the system ends it in the background it comes back with
your server, name and draft, and connects again.

ClayWind's `claywind-pack` makes the app, from the `client` directory (`--network` gives Android's network
permission and the sentence iOS shows before the first connection to a device on the local network):

```sh
claywind_pack android --name "ClayWind Chat" --id com.example.chat --network "Connects to the chat server you name"
adb install "out/pack/ClayWind Chat.apk"
claywind_pack ios --name "ClayWind Chat" --id com.example.chat --network "Connects to the chat server you name"
xcrun devicectl device install app --device <device> "out/pack/ios/ClayWind Chat.app"
```

On the phone, give the server as the address of the machine it runs on (`192.168.1.20:7700`), not `127.0.0.1`.

## Licence

MIT, see [LICENSE](LICENSE).
