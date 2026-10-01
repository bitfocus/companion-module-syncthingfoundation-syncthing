# companion-module-syncthingfoundation-syncthing

A [Bitfocus Companion](https://bitfocus.io/companion) module for [Syncthing](https://syncthing.net/).

It answers the question an operator usually has about Syncthing: is everything in sync? One
connection watches one Syncthing instance through its REST API and turns its state into buttons,
variables and feedbacks.

- **Sync state at a glance.** Whether this machine is up to date, and whether every other device
  has caught up too, as one variable and one feedback each.
- **Folders and devices.** State, completion, transfer activity and errors per folder and per
  device, following the instance's configuration as it changes.
- **Control.** Rescan, pause and resume folders and devices, override or revert local changes,
  restart Syncthing.
- **Immediate updates.** The module follows the Syncthing event stream, so changes reach buttons
  within milliseconds, with polling underneath as a safety net.
- **Easy setup.** Instances on the local network are found automatically, and the API key can be
  read from an instance whose web interface has no password.

See [HELP.md](./companion/HELP.md) for user documentation, [CHANGELOG.md](./CHANGELOG.md) for what
changed when, and [LICENSE](./LICENSE) for the license. Report problems in the
[issue tracker](https://github.com/bitfocus/companion-module-syncthingfoundation-syncthing/issues).

## Status

The module is in pre-release. Setting up a connection has been tested against real Syncthing
instances on Windows and in a virtual machine, over HTTP and HTTPS. The control actions and the
event stream under real transfer activity have only been checked against a simulated Syncthing so
far.

Everything below 1.0.0 is a pre-release, and each round of changes raises the patch number.
Version 1.0.0 will be the first published release.

## Getting started

The project uses Yarn 4 through Corepack. Enable Corepack once, so that `yarn` picks up the version
the project declares instead of a globally installed Yarn 1:

```
corepack enable
```

Running `yarn` then performs all the steps needed to develop the module. Build once with
`yarn build`, which is enough for Companion to load the module from this folder in developer mode.
While developing, `yarn dev` runs the compiler in watch mode and recompiles on change.

Check types and style with `yarn build` and `yarn lint`. Run `yarn test` after a build to exercise
the module against a simulated Syncthing server, from the REST client up to a full module instance
driven the way Companion drives it.

## Packaging and releases

Build a pre-release package with the flag that marks it as one:

```
yarn package --prerelease
```

Without the flag the build tool clears the pre-release marker in the manifest, whatever the file
says.

Releases are made from version tags, so only push a tag for a version that is meant to be
published. The tag has to match the version in `package.json`, for example `v1.0.0` for version
`1.0.0`, or the Bitfocus module check fails.

## Project layout

| File               | Contents                                                            |
| ------------------ | ------------------------------------------------------------------- |
| `src/main.ts`      | The instance class, polling loop and state publishing               |
| `src/api.ts`       | REST client for the Syncthing API, including error classification   |
| `src/types.ts`     | Types for the REST responses this module reads                      |
| `src/state.ts`     | Folder and device state, variable naming and completion maths       |
| `src/config.ts`    | Connection settings shown in the Companion web UI                   |
| `src/discover.ts`  | Reads the API key from an instance whose web interface has no login |
| `src/events.ts`    | Long-polling event stream, with reconnect and restart detection     |
| `src/lanscan.ts`   | Finds instances on the network and checks whether they answer       |
| `src/netbios.ts`   | Asks a machine its own name when reverse DNS has none               |
| `src/actions.ts`   | Actions                                                             |
| `src/feedbacks.ts` | Feedbacks                                                           |
| `src/variables.ts` | Variable definitions and the uptime formatter                       |
| `src/presets.ts`   | Ready-made buttons                                                  |
| `tests/`           | Dependency-free checks, run with `yarn test` after `yarn build`     |
| `scripts/`         | Diagnostics, for example `node scripts/discover-check.mjs`          |

## Roadmap

1. ~~Scaffold, connection, status and instance-wide variables~~ done
2. ~~Per-folder and per-device variables, feedbacks and presets, driven by the live configuration~~ done
3. ~~Per-folder and per-device actions: pause, resume, override, revert~~ done
4. ~~Event stream via `/rest/events` long polling, replacing most of the polling~~ done
5. ~~Network discovery and automatic API key reading for an easier setup~~ done
6. Real-world testing of the control actions and of the event stream under transfer activity,
   then the first published release, 1.0.0

Deliberately out of scope: actions that add a folder or add a remote device. Those are setup steps
that belong in the Syncthing web interface, where the device ID can be checked before confirming.
They may be reconsidered if users ask for them.

## References

- [Syncthing REST API](https://docs.syncthing.net/dev/rest.html)
- [Syncthing event API](https://docs.syncthing.net/dev/events.html)
- [Syncthing local discovery protocol](https://docs.syncthing.net/specs/localdisco-v4.html)
- [Companion module API](https://github.com/bitfocus/companion-module-base)
