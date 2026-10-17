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

### Language support and the next release

The 25-language update is being developed in the
[LeaveSafe source repository](https://github.com/atakankizilyuce/LeaveSafe).
The manifests currently committed here still install **1.6.1**; this documentation
does not make the development translations available in that existing binary.
The Flutter mobile and desktop application's translations are maintained in
[leavesafe-app](https://github.com/atakankizilyuce/leavesafe-app) and ship separately.

The daemon's planned language codes are:

`en`, `zh-Hans`, `hi`, `es`, `ar`, `fr`, `bn`, `pt`, `id`, `ur`, `ru`, `de`,
`ja`, `pcm`, `arz`, `mr`, `vi`, `te`, `sw`, `ha`, `tr`, `pnb-Arab`, `tl`,
`ta`, `yue-Hant`.

Once a reviewed release containing them is published, its setup language choice
applies to terminal instructions and its browser panel has its own language
selector. Both read bundled catalogs; translating the interface does not require
an online translation service. OS-generated diagnostic output and literal shell
commands remain in their original form. Terminal glyph support depends on the
terminal's installed fonts.

For that release, maintainers should verify the source commit's full language and
independent-review gates, successful platform tests, and Sonar evidence: no new
issues or security problems and at least 80% measured code coverage. Then publish
the approved source tag, inspect the generated manifest PR here, and verify the
downloaded assets and their hashes before merging it. Do not replace the current
1.6.1 checksums with development builds or invent a release version.

This repository contains generated package metadata rather than the application
translation catalogs. A documentation-only change here has no executable lines
on which to measure code coverage; the coverage requirement is checked on the
application and daemon code that is actually shipped. Package-manager descriptions
and store listings are a separate localization stage after application work.

## What is here

| Path | For |
|------|-----|
| `Formula/` | Homebrew formulae, one `.rb` per package |
| `bucket/` | Scoop manifests, one `.json` per package |
| `winget/<version>/` | Exactly what was sent to `microsoft/winget-pkgs`, kept so a reviewer sees it in the same diff |

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

A release in a source repository dispatches a `release-published` event here.
[`publish.yml`](.github/workflows/publish.yml) regenerates the manifests — downloading each
published asset and hashing what it actually got — and opens a pull request.

**Merging that pull request is the publish.** Homebrew and Scoop users have the new version
at that moment, and the merge starts the winget submission
([`winget.yml`](.github/workflows/winget.yml)), which Microsoft then reviews on their own
schedule. Until the merge, the new version exists on its releases page and nowhere else.

Nothing pushes to `main` directly, and `main` is protected so nothing can: a tag pushed by
mistake should not become an installed version, and a human reading a diff is the check that
prevents it. Prereleases are refused outright — a beta must never be what `brew install`
produces.

`publish.yml` can also be run by hand with a tag, for when a dispatch was missed or a
manifest needs regenerating. It takes a `dry_run` option that generates and diffs without
opening anything, which is the way to rehearse a change to the pipeline itself.

## Adding another package

A source repository qualifies when it provides

```
packaging/generate.sh <tag> <output-dir>
```

emitting one `.rb` and one `.json` into the output directory, with checksums taken from the
assets it downloaded rather than from a local build. Any `.yaml` files it writes are treated
as winget manifests. The workflow here checks that repository out at the tag, runs the
script, and files the results.

It dispatches with a payload naming itself:

```json
{ "app": "leavesafe", "repo": "atakankizilyuce/LeaveSafe", "tag": "v1.3.0" }
```

Users who have already tapped this repository get every package added later without doing
anything.
