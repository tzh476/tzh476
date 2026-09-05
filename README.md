## Projects

- **[zvm](https://github.com/tzh476/zvm)** — a JVM implementation written in Java. 249 stars, 55 forks.
- **[innodb-viewer](https://github.com/tzh476/innodb-viewer)** — Java tooling for inspecting InnoDB data structures.
- **[allocguard](https://github.com/tzh476/allocguard)** — Go checker for capacity reserved from untrusted input before that input is validated.

## allocguard

Some Go parsers size a slice from a separator count taken straight off the wire, before
any element has been validated. A short, wholly invalid input then makes the parser
reserve memory it immediately discards.

`allocguard` reports only those sites. It is deliberately quiet: `make([]T, 0, n)` is
almost always fine, so a grep is useless — a site is reported only when the capacity is
counted from a value the function did not create and has not yet checked.

Two things worth knowing before you try it:

- **It suggests bounding the hint by the input length, not by a constant.** An earlier
  release recommended a constant ceiling and that was wrong: on a 1000-element list a
  cap of 64 costs 95,616 B against 40,576 B unclamped, so it makes the common case
  2.36x worse in order to fix the rare one.
- **The recursion check is off by default and is a lead generator, not a verdict.**
  Across 11 OSS-Fuzz projects it produced 55 findings and zero true positives.

Measured on real code: it reports both real `svcb.go` sites in `miekg/dns`, and finds
nothing in `sigstore/cosign`.

    go install github.com/tzh476/allocguard@latest
    allocguard ./...

Free and MIT: <https://github.com/tzh476/allocguard>. There is also a paid packaged
copy at <https://payhip.com/b/27A9r> (USD 19) if you would rather have a pinned
archive with install notes than track the repo — the tool itself is the same code.

## Recent upstream contributions

- [microsoft/qdk#3291](https://github.com/microsoft/qdk/pull/3291) — a public Python import API for `.qsc` circuits, backed by native Rust conversion.
- [visgl/react-google-maps#1023](https://github.com/visgl/react-google-maps/pull/1023) — keep non-finite Maps camera values out of React state.
- [moth-quantum/QuantumBrush#56](https://github.com/moth-quantum/QuantumBrush/pull/56) — preserve canvas sizing across project reload.
