# i-machine-things Flatpak Repository

A combined Flatpak remote for all i-machine-things apps.  
Each app is built and released individually; this repo aggregates the bundles weekly into a single OSTree remote.

## Add the remote

```bash
flatpak remote-add --if-not-exists i-machine-things \
  https://i-machine-things-org.github.io/flatpak-repo/repo
```

Or install the `.flatpakrepo` file:

```bash
flatpak remote-add --if-not-exists i-machine-things i-machine-things.flatpakrepo
```

## Apps

| App | App ID | Source |
|-----|--------|--------|
| JobDocs | `io.github.i_machine_things.JobDocs` | [i-machine-things-org/JobDocs](https://github.com/i-machine-things-org/JobDocs) |
| Honey Batchr | `com.honeybatchr.HoneyBatchr` | [i-machine-things/HoneyBatchr](https://github.com/i-machine-things/HoneyBatchr) |

## Install an app

```bash
flatpak install i-machine-things io.github.i_machine_things.JobDocs
flatpak install i-machine-things com.honeybatchr.HoneyBatchr
```

## How it works

The [publish workflow](.github/workflows/publish.yml) runs weekly (Monday 04:00 UTC) and on demand via `workflow_dispatch`.  It downloads the latest `.flatpak` bundle from each project's GitHub Releases, imports them into a local OSTree repo with `flatpak build-import-bundle`, and deploys the result to the `gh-pages` branch.

Individual project repos are not modified — they each continue to ship their own `.flatpak` release assets and can maintain their own per-project remotes for backwards compatibility.

## GPG signing

The repo is currently unsigned.  To add signing:

1. Generate a key: `gpg --gen-key`
2. Export it: `gpg --armor --export <KEY_ID>`
3. Add the armored key as a GitHub Actions secret named `FLATPAK_GPG_PRIVATE_KEY`
4. Pass `--gpg-sign=<KEY_ID>` to `flatpak build-import-bundle` in the workflow
5. Add `GPGKey=<base64-encoded-public-key>` to `i-machine-things.flatpakrepo`
