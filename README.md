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
