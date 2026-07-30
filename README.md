# homebrew-tap

Homebrew formulae and Scoop manifests for the software published from this account.

One repository serves both. Homebrew requires a tap repository to be named
`homebrew-<something>`; Scoop imposes no naming rule at all. So the two can share a single
home, and there is one place to look when a published version is wrong.

## Install

**Homebrew** — macOS and Linux:

```bash
brew tap atakankizilyuce/tap
brew install leavesafe
```

**Scoop** — Windows:

```powershell
scoop bucket add atakankizilyuce https://github.com/atakankizilyuce/homebrew-tap
scoop install leavesafe
```

Yes — a Scoop bucket at a URL that says `homebrew`. See above for why.

## What is here

| Path | For |
|------|-----|
| `Formula/` | Homebrew formulae, one `.rb` per package |
| `bucket/` | Scoop manifests, one `.json` per package |

### Packages

| Package | Source | Platforms |
|---------|--------|-----------|
| `leavesafe` | [atakankizilyuce/LeaveSafe](https://github.com/atakankizilyuce/LeaveSafe) | macOS, Linux, Windows |

Windows users can also install LeaveSafe through winget. Those manifests live in
`microsoft/winget-pkgs`, not here:

```powershell
winget install LeaveSafe.LeaveSafe
```

## These files are generated

Every manifest here carries the SHA-256 of a published release binary, and a hand-written
checksum is a checksum that is wrong on the second release. **Nothing in `Formula/` or
`bucket/` should be edited by hand.** Such an edit would be overwritten by the next release
and, until then, would describe a file nobody can verify.

To change what a manifest says, edit the template in the source repository's `packaging/`
directory and cut a release.

## How a version lands here

A release in a source repository dispatches an event to this repository. A workflow here
regenerates the manifests — downloading each published asset and hashing what it actually
got — and opens a pull request. **Merging that pull request is the publish.** Until then the
new version exists on its releases page and nowhere else.

Nothing pushes to `main` directly, deliberately: a tag pushed by mistake should not become an
installed version, and a human reading a diff is the check that prevents it. Prereleases are
refused outright — a beta must never be what `brew install` produces.

## Adding another package

A source repository qualifies when it provides

```
packaging/generate.sh <tag> <output-dir>
```

emitting one `.rb` and one `.json` into the output directory, with checksums taken from the
assets it downloaded rather than from a local build. The workflow here checks that repository
out at the tag, runs the script, and files the results under `Formula/` and `bucket/`.

Users who have already tapped this repository get every package added later without doing
anything.
