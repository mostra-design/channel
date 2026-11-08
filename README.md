# mostra channel for Claude Code

Comments you post on your running app reach your Claude Code session as you post them, even when it's idle. Claude can start on them while you keep clicking around. This is a [Claude Code channel](https://code.claude.com/docs/en/channels): a small local program that connects [mostra](https://mostra.design) to the session.

## Set it up

You need Node 22 or newer, Claude Code signed in with a claude.ai account or a Console API key, and an API key from your project's Setup page in mostra. On a Team or Enterprise plan, an admin has to turn channels on first.

In your app's folder:

```sh
claude mcp add mostra-channel -e MOSTRA_API_KEY=mostra_sk_... -- npx -y @mostra-design/channel
claude --dangerously-load-development-channels server:mostra-channel
```

Channels are a research preview, so Claude Code loads only channels Anthropic has approved. The flag lets it load this one until it's approved. It asks you to confirm once.

To have Claude mark comments fixed once it's done, add mostra's MCP server too. The project's Setup page has the command.

## What arrives

- Comments and replies you post arrive whole, with the page and element they're on. Claude treats them as if you had typed them in the session.
- If you edit, close or delete one of them, Claude hears it, so it works from the corrected comment, not the old one.
- Comments from other people arrive as one line, "A reviewer posted #41.", with none of their words, not even their name. Anyone with your shared link can post, and a channel message reads as if you typed it, so their text stays out. Claude tells you and does nothing more unless you ask.
- "Not yet" on a fix arrives as a reopen, so Claude knows the fix didn't work.
- Agents' replies don't arrive, so Claude never answers itself.

If the connection drops, the channel reconnects and catches up on comments posted meanwhile that are still open. If mostra can't be reached or refuses the key, it says so once in the session.

Keep one Claude Code session open per key. Every session you start in this folder follows the project, even without the flag, and two sessions would both start on the same comment. A key allows at most five sessions at once.

## Settings

- `MOSTRA_API_KEY` (required): your project's API key. Comments from that project arrive, and "you" means whoever made the key.
- `MOSTRA_URL`: `https://mostra.design` by default. Plain `http` works only for an address on this machine, so the key is never sent unencrypted.

## Developing it

```sh
node --test
```

Against a mostra server on your machine, from a checkout:

```sh
claude mcp add mostra-channel -e MOSTRA_URL=http://localhost:4000 -e MOSTRA_API_KEY=mostra_sk_... -- node /path/to/channel/bin/mostra-channel.mjs
```

## Security

To report a security problem in the channel or on mostra.design, email security@mostra.design. Include steps to reproduce if you can. Please don't open a public issue or pull request.

## License

MIT
