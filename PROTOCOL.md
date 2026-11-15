# The chat protocol

One TCP connection per client, UTF-8 text, one message per line (`\n`; a `\r` before it is ignored). A line is a
command word, a space, and its arguments; the last argument runs to the end of the line, so it may hold spaces. A
line longer than 4,096 bytes is refused, as is anything before `HELLO`.

## Client to server

| Line | Meaning |
|---|---|
| `HELLO <name>` | Join the server as `<name>` (1 to 24 letters, digits, `-` or `_`). Answered by `WELCOME` or `ERR`. |
| `JOIN <room>` | Join a room (the same rules as a name), made if new. Answered by `HISTORY` lines, then `JOINED` to everyone in it. |
| `LEAVE <room>` | Leave a room. `LEFT` goes to everyone still in it. |
| `SAY <room> <text>` | Send `<text>` to a room you are in. `MSG` goes to everyone in it, you included. |
| `ROOMS` | Ask for the rooms. Answered by one `ROOM` line each, then `ROOMS-END`. |
| `QUIT` | Leave the server. |

## Server to client

| Line | Meaning |
|---|---|
| `WELCOME <name>` | You are in, as `<name>`. |
| `ERR <reason>` | Your last line was refused: the reason in words. |
| `JOINED <room> <name>` | `<name>` joined `<room>` (you, after your `JOIN`). |
| `LEFT <room> <name>` | `<name>` left `<room>`, or the server. |
| `MSG <room> <name> <time> <text>` | `<name>` said `<text>` in `<room>`; `<time>` is the server's Unix time in seconds. |
| `HISTORY <room> <name> <time> <text>` | One of the room's last 50 messages, sent on `JOIN` before `JOINED`. |
| `ROOM <room> <members>` | A room and how many are in it. |
| `ROOMS-END` | The end of the `ROOM` lines. |
