# ClayWind Chat

A chat client and server written in [CeeSafe](https://github.com/CeeSafe/CeeSafe). The client is built on
[ClayWind](https://github.com/CeeSafe/Clay), a Tailwind-style UI toolkit, as an outside app would be: it depends on
ClayWind's released packages only, and nothing in the toolkit was changed for it. It is the gate of ClayWind's app
runtime phase: what a real desktop app needs, and whether ClayWind gives it.

- `server/`: rooms of people talking over TCP, in the line protocol of [PROTOCOL.md](PROTOCOL.md). One task per
  connection under an async task set; the rooms and each room's last 50 messages are shared behind an `AsyncMutex`.
- `client/`: the desktop app (coming next).

## Running the server

Install the CeeSafe compiler (`ceesafec` 0.54.0 or later), then:

```sh
cd server && ceesafec build && ./out/chat_server 7700
```

Anything that speaks lines over TCP can talk to it, for example:

```sh
printf 'HELLO ada\nJOIN lobby\nSAY lobby hello\n' | nc 127.0.0.1 7700
```

## Licence

MIT, see [LICENSE](LICENSE).
