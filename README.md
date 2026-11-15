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

Install the CeeSafe compiler (`ceesafec` 0.55.0 or later), then:

```sh
cd server && ceesafec build && ./out/chat_server 7700
```

Anything that speaks lines over TCP can talk to it, for example:

```sh
printf 'HELLO ada\nJOIN lobby\nSAY lobby hello\n' | nc 127.0.0.1 7700
```

## Running the client

The client needs `ceesafec` 0.55.0 or later: its dependencies are packages in subdirectories of the ClayWind repository
(CeeSafe's ADR 0152), it shows times in the local time zone (ADR 0153), and on Windows 0.55.0 fixes fetching git
dependencies and a first connection. Then, with a server running:

```sh
cd client && ceesafec build && ./out/claywind_chat
```

Choose a name and press Enter.

## Licence

MIT, see [LICENSE](LICENSE).
