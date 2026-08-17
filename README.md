# station-icons

Instrument logos served over jsDelivr's GitHub CDN.

```
https://cdn.jsdelivr.net/gh/kubakukla/ticker_icons@v3/icons/<key>.png
```

`<key>` is the XTB ticker, lowercased, with `.` replaced by `_` — `CDR.PL`
becomes `cdr_pl`. Every icon is PNG whatever the upstream source served, so
the URL follows from the ticker with no index lookup. Coverage is partial by
design — 7418 of 9730 tickers — and a miss returns 404, which the frontend
turns into a generated initials avatar.

The exchange suffix is deliberately kept: bare symbols
collide across markets (GPW's `CDR` is CD Projekt, NYSE's is Cedar Realty),
and resolving to the wrong company's logo is the exact bug this repo exists
to fix.

Pin a tag rather than `@main`. jsDelivr caches `@main` for about a week with
no automatic invalidation, so a tag per release is what makes an update
actually reach browsers.

## The 50 MB limit

**jsDelivr refuses to serve a repository over 50 MB** — every request comes
back `403 Package size exceeded the configured limit of 50 MB`, whether or
not the individual file is small. The first version of this set was 70 MB
and was entirely unservable.

Icons are therefore capped at 96px and encoded as PNG-8 where the colours
allow it, which is ample for the 26-40px the avatar renders at and brings
the set to ~26 MB. `cmd/fetchlogos` in the station repo does this on the way
in; regenerate with

```sh
go run ./cmd/fetchlogos -groups all -out ../ticker_icons
```

and check the total stays well under 50 MB before tagging. Note that the
limit is on the whole package, so headroom shrinks as coverage grows.

## Provenance and licensing

`manifest.json` records, for every stored file, the source, the exact source
URL, the licence, and a SHA-256 of the bytes. That file is the audit trail —
keep it in sync with `icons/`.

The logos are third-party trademarks. They are reproduced here to identify
the financial instrument each one refers to, which is nominative use and is
standard practice among brokers and portfolio trackers. They are not covered
by this repository's licence, no affiliation or endorsement is implied, and
each mark remains the property of its owner.

Entries with `"source": "wikimedia-commons"` carry a per-file licence in the
manifest. Several are CC-BY-SA and **require attribution** wherever they are
displayed — check the manifest before assuming a file is unconditioned.

To have a logo removed, open an issue.
