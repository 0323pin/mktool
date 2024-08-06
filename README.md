# mktool

This is intended to be a collection of utilities to replace parts of
[pkgsrc](https://github.com/NetBSD/pkgsrc/)'s mk infrastructure.

Many targets under `mk/` are implemented using a combination of shell and
awk, and can suffer from a lack of performance, especially when input sizes
grow.

For example, with the profligation of Go modules used in newer Go software,
www/grafana now has over 5,000 distfiles.  This exposes various issues in
the current pkgsrc checksum scripts that are hard to work around.  This
tool implements replacements with the following performance improvements
when running in www/grafana on a 32-core SmartOS host:

|          Command | Existing pkgsrc scripts |      mktool |  Speedup |
|-----------------:|------------------------:|------------:|---------:|
| `bmake checksum` |              10 seconds |   2 seconds |   **5x** |
| `bmake distinfo` |   3 minutes, 30 seconds |   2 seconds | **100x** |

As pkgsrc strives to be as portable as possible, at no point will any of the
commands implemented by `mktool` become mandatory.  This tool simply exists
for those who are able to run Rust software to dramatically improve pkgsrc
performance.

## Installation

Install using `cargo`:

```shell
cargo install mktool
```

At some point in the future it will hopefully be possible to enable `mktool`
by simply setting:

```make
TOOLS_PLATFORM.mktool=  /path/to/mktool
```

in your `mk.conf`, and any supported commands will be automatically enabled.

## Commands

These are the commands currently implemented.

### checksum

A replacement for
[pkgsrc/mk/checksum/checksum.awk](https://github.com/NetBSD/pkgsrc/blob/trunk/mk/checksum/checksum.awk)

### distinfo

A replacement for
[pkgsrc/mk/checksum/distinfo.awk](https://github.com/NetBSD/pkgsrc/blob/trunk/mk/checksum/distinfo.awk)
