<p align="center">
  <img src="https://sncfoundation.github.io/logos/sheetlux-cd.svg" width="96" alt="Sheetlux CD logo">
</p>

<h1 align="center">Sheetlux CD</h1>

<p align="center"><b>GitOps for the spreadsheet cluster — our Argo CD.</b><br>
A <a href="https://sncfoundation.github.io">Sheet-Native Computing Foundation</a> project · analog of <b>Argo CD</b> / <b>Flux</b></p>

---

**Status:** 💾 Autosaving · a real, working component extracted from Sheeternetes.

## What it does

Sheetlux CD continuously reconciles a **directory of manifests** into a Sheeternetes cluster.
The declared state in Git is the source of truth: it pulls the repo, checksums the manifests,
and runs `skctl apply` on every change. Because `apply` is an idempotent upsert, the cluster
converges to Git — exactly like Argo CD or Flux, just smaller, funnier, and backed by a
spreadsheet.

## Requirements

- A running Sheeternetes cluster and **`skctl`** on your `PATH`
  (from [sncfoundation/sheeternetes](https://github.com/sncfoundation/sheeternetes)).
- `bash`, `git`, `shasum`.

## Usage

```bash
# reconcile ./gitops every 15s (Ctrl-C to stop)
./sheetlux

# a different directory / interval / skctl path
INTERVAL=30 ./sheetlux ~/my-manifests
SKCTL=/path/to/skctl ./sheetlux ./gitops
```

Point it at a Git working tree and it will `git pull --ff-only` before each sync, so a push to
your manifests repo rolls out to the cluster automatically. GitOps, spreadsheet-native.

## Example

`gitops/app.json`:

```json
{ "kind": "Deployment", "name": "hello", "image": "nginxdemos/hello", "replicas": 2, "cpu": 100, "memory": 64 }
```

Run `./sheetlux` and the `hello` Deployment converges to 2 replicas. Edit the file, and the
change is applied on the next tick. Delete a field, and it reconciles. It reconciles.

## Roadmap

- Prune (delete resources removed from Git).
- Health/sync status written back to a `Sheetlux` tab.
- Multi-source and per-directory apps.

Tracking & design: [sncfoundation/sheeternetes#32](https://github.com/sncfoundation/sheeternetes/issues/32) ·
[SNCF Landscape](https://sncfoundation.github.io/landscape.html)

---

<sub>Licensed under Apache-2.0. Do not run production on a spreadsheet. If you do, please film it.</sub>
