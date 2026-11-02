# Sayon's package index

Every version published to Sayon's package registry, part of [Sayon's website](https://github.com/sayon-lang/www.sayon-lang.org): what `sayon install` reads to choose versions and check what it downloads. The registry writes it as `sayon publish` adds versions, one commit for each; nobody edits it by hand.

The website is not operated yet, so the index has no packages, and `config.json` names the addresses it will have. Until then `sayon` uses a registry on your own computer, with an index of its own.

## What it holds

- `config.json`: where the registry's API is (`api`), for `sayon publish`, `login`, and `search`, and where each version's archive is (`download`, with `{name}` and `{version}` in it).
- A file for each package, placed by its name as crates.io-index places them: `1/a`, `2/ab`, `3/a/abc`, and `ab/cd/abcdef` for longer names, as `strings` is at `st/ri/strings`.
- In each file, a line of JSON for each version, oldest first, never changed once written:

```json
{"name":"words","version":"1.0.0","digest":"sha256:<64 hex digits>","description":"Words, louder","dependencies":[{"alias":"strings","package":"strings","requirement":"0.2","registry":null}],"published":"2026-10-28T09:30:00Z"}
```

`digest` is the SHA-256 of the version's archive, which `sayon` checks each download against, and which `sayon.lock` records. A dependency's `registry` is `null` when it comes from this registry. [PROTOCOL.md](https://github.com/sayon-lang/www.sayon-lang.org/blob/main/PROTOCOL.md) of the website describes it all.

## Reading it

`sayon` names a registry by its index's address and reads only the files it needs, over HTTP: `config.json`, then each package it installs. A registry's index is served at `<registry>/index`, as the one on this computer is at `http://127.0.0.1:4100/index`.
