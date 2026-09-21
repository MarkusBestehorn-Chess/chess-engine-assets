# chess-engine-assets

Produced engine artefacts for the `chess` project: binary files this project **converts or builds** rather
than downloading, published here so that every installed build can obtain them by an ordinary anonymous
download.

**This repository holds artefacts, not source.** It carries no build system, no tests and no application
code. The producing code, the declarations that pin these bytes, and the reasoning behind each artefact live
in the `chess` project.

## Why a separate repository

Every consumer of a declared engine artefact URL is unauthenticated by design: the provisioning script
performs a plain `fetch(url)` with no credential, and reading a CI runner's token is forbidden by that
project's own configuration rules. A **private** repository's `/releases/download/<tag>/<name>` path answers
`404 Not Found` to an unauthenticated client, so a produced artefact published there reaches nothing. This
repository is public precisely so that path serves.

## How the bytes are pinned

Each artefact is declared in the consuming project's engine manifest with its `sha256`, and that digest is
the integrity gate. A GitHub release asset is immutable only by **convention** — it can be replaced under a
fixed tag — so the digest, not the URL, is what establishes that the bytes are the intended ones. The
provisioning script verifies the digest after download and refuses a mismatch.

## Licensing

Each release describes its own assets: the upstream they derive from, the immutable commit of that upstream,
the tool and version that produced them, any modification made, and the licence they are conveyed under.
Read the release notes of the tag you are downloading from.

Artefacts derived from GPL-licensed upstreams are conveyed under those upstreams' licences, with a
Corresponding-Source pointer to the exact upstream commit and — where this project modified the work — a
prominent modification notice with its date, as GPL-3.0 §5(a) requires. `LICENSE` in this repository is the
verbatim GNU General Public License version 3, the licence the artefacts published here so far are conveyed
under.

## Tags

Asset tags are deliberately **not** semantic-version shaped. A `v*.*.*` tag is the consuming project's
product-release namespace and triggers its release pipeline; an artefact publish must never do that.
