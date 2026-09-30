# CrateCheck Desktop

A small helper for your Mac that lets the [CrateCheck](https://cratecheck.app) browser extension see your rekordbox library, so every track you already own gets a checkmark before you buy it again.

This repo only hosts releases. **To install, go to [cratecheck.app/desktop](https://cratecheck.app/desktop).**

## What it does, and doesn't

- **Reads your rekordbox library, read-only.** It opens rekordbox's `master.db` (rekordbox 6 or 7) with a fixed set of built-in queries and never writes to it.
- **Makes no network connections.** There's no networking code in it. It talks only to the CrateCheck extension, over Chrome's [native messaging](https://developer.chrome.com/docs/extensions/develop/concepts/native-messaging) (stdin/stdout), and only when the extension asks.
- **Runs only when needed.** Your browser starts it while CrateCheck checks tracks, and it quits when it's done. Nothing runs in the background, and it doesn't write logs.
- **Keeps its files in one folder,** `~/Library/Application Support/CrateCheck/`:
  - `bin/cratecheck`: the helper itself
  - `catalog.db`: an index of your owned tracks' titles and artists, so checks are instant
  - `key-cache.json`: your library's decryption key (rekordbox encrypts `master.db`), cached so opening it stays fast
  - `helper.json`: settings (only if you installed with a custom library path)

  It also adds one native-messaging manifest (`com.cratecheck.desktop.json`) per installed browser, in that browser's `NativeMessagingHosts` folder, so the browser can find it. Chrome, Chromium, Brave, Edge and Arc are supported.

## Uninstall

```sh
"$HOME/Library/Application Support/CrateCheck/bin/cratecheck" uninstall
```

This removes the helper, its folder and the browser manifests.

## Verify a download

Every release has a `SHA256SUMS` file. From the folder you downloaded into:

```sh
shasum -a 256 -c SHA256SUMS --ignore-missing
```

`install.sh` does this check for you before it installs anything.

## Release assets

The file names stay the same in every release, so `releases/latest/download/<name>` always gets the newest one.

| File                                | What it is                                                             |
| ----------------------------------- | ---------------------------------------------------------------------- |
| `install.sh`                        | The script behind `curl -fsSL https://cratecheck.app/install.sh \| sh` |
| `cratecheck-macos-universal.tar.gz` | The helper for Apple Silicon and Intel Macs                            |
| `SHA256SUMS`                        | Checksums for every file in the release                                |
| `THIRD_PARTY_NOTICES.txt`           | Licenses of the open-source components inside                          |

## Help

Email [contact@cratecheck.app](mailto:contact@cratecheck.app). Issues are off on this repo, so email is the fastest way to reach us.

## License

Free to use with CrateCheck. See [LICENSE](LICENSE). Open-source components are listed in each release's `THIRD_PARTY_NOTICES.txt`.
