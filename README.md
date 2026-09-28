# agentteams-element

The AgentTeams Element Web client as an OpenCharly candy.

Element Web is the browser chat client AgentTeams users open to watch and join
the Manager–Workers Rooms (Matrix chat spaces). This candy ships the static
Element Web files, served by a **rootless nginx** on `:8088`. The nginx config
is authored rootless (writable pid and temp paths under `/tmp`, no setuid), and
a charly-owned start script regenerates `config.json` from the deploy
environment before exec'ing nginx.

The service runs rootless as the image user (uid 1000).

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `agentteams-element` |
| Binary | Element Web static files (`/opt/element-web`) served by nginx |
| Service / port | `element-web` on `8088` |
| Package | `nginx` (Arch) |

Deploy-overridable `env_accept` vars select the Matrix server the browser
connects to, the browser-facing homeserver URL, and the brand name.

## How to use it

Compose the candy into a box (or the full AgentTeams stack, whose top
composition already includes it):

```yaml
my-agentteams:
  candy:
    base: cachyos.cachyos
    candy:
      - '@github.com/opencharly/pod-agentteams-element:<tag>'
```

Then build and deploy with the charly CLI:

```bash
charly box build my-agentteams
charly start my-agentteams
```

Open `http://<host>:<mapped-8088>/` to reach the client. See the owning skill
for the composition, ports, and both deploy substrates.

## Layout

- `charly.yml` — the `agentteams-element:` candy entity: the Element Web
  `extract`, the `nginx` package, `env_accept`, the port, the `element-web`
  service, and the plan that writes the rootless nginx config and start script.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-agentteams:agentteams` — the full stack composition.
- Sibling services: `/charly-agentteams:agentteams` (matrix, minio, higress, controller).
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
