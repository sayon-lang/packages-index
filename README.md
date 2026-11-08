# Sayon's package index

This repository lists every version ever published to Sayon's package registry, which is part of [Sayon's website](https://github.com/sayon-lang/www.sayon-lang.org). `sayon install` reads it to pick versions and to check what it downloads. The registry writes to it, one commit per `sayon publish`. Nobody edits it by hand.

The website isn't online yet, so the index is empty and `config.json` holds the addresses it will have. Until then, `sayon` uses a registry running on your own computer, with its own index.

## What's in it

- `config.json` says where the registry's API is (`api`), which `sayon publish`, `login`, and `search` use, and where each version's archive can be downloaded (`download`, with `{name}` and `{version}` filled in).
- Each package has a file, placed by its name the same way crates.io-index does it: `1/a`, `2/ab`, `3/a/abc`, and `ab/cd/abcdef` for longer names. So `strings` lives at `st/ri/strings`.
- Each file has one line of JSON per version, oldest first. A line never changes once it's written:

```json
{"name":"words","version":"1.0.0","digest":"sha256:<64 hex digits>","description":"Words, louder","dependencies":[{"alias":"strings","package":"strings","requirement":"0.2","registry":null}],"published":"2026-10-28T09:30:00Z"}
```

`digest` is the SHA-256 of the version's archive. `sayon` checks every download against it, and `sayon.lock` records it. A dependency's `registry` is `null` when it comes from this same registry. The website's [PROTOCOL.md](https://github.com/sayon-lang/www.sayon-lang.org/blob/main/PROTOCOL.md) describes all of it.

## How it's read

`sayon` knows a registry by its index's address, and fetches only the files it needs over HTTP: `config.json` first, then the file of each package it installs. A registry serves its index at `<registry>/index`, so the one on your own computer is at `http://127.0.0.1:4100/index`.
