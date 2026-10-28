# AvogadroDB: executables for the ClickBench entry

This repository publishes the executables that the AvogadroDB entry in
[ClickBench](https://benchmark.clickhouse.com/) downloads and runs. Nothing
else lives here: no source code, no data. The entry itself (the scripts the
ClickBench driver runs: `install`, `load`, `query`, `data-size`) is in the
`avogadrodb/` directory of
[ClickHouse/ClickBench](https://github.com/ClickHouse/ClickBench).

AvogadroDB is an embedded analytical database written in Rust: its own column
format and compression cascade on disk, with Apache DataFusion as the query
executor over that format. It ships as a Windows desktop application, a
command-line tool (`avogadrodb`, Windows and Linux) and a local server. The
ClickBench entry uses the command-line tool.

## Releases

Each release carries two Linux x86-64 executables, built inside
`manylinux_2_28` on the x86-64 baseline (no AVX2 or newer instruction assumed;
runs on any x86-64 CPU), needing glibc >= 2.28 and only libc, libm, libdl,
libpthread, libgcc_s and the loader:

| asset | what it is |
|---|---|
| `avogadrodb-<tag>-linux-x86_64` | the command-line tool: `load`, `query`, `molecularize`, `version` |
| `clickbench-harness-<tag>-linux-x86_64` | the measurement harness used during development (runs the 43 queries with DuckDB as the oracle over the same parquet file); not used by the ClickBench entry |
| `SHA256SUMS` | the sha256 of each asset |
| `glibc.txt` | the dynamic libraries and the highest GLIBC symbol each asset asks for |

The ClickBench entry pins one release tag and the sha256 of its `avogadrodb`
asset in its `install` script; a mismatch is a refusal.

## Reproducing the benchmark

Clone ClickHouse/ClickBench, enter `avogadrodb/`, run `./install` and then
`./benchmark.sh` on a VM like the ones the site uses (Ubuntu 24.04, 500 GB
disk). `install` downloads the pinned release from here and verifies it.

## License

The executables are published under the AvogadroDB Evaluation License
([LICENSE](LICENSE)): download and run them to evaluate AvogadroDB and to
reproduce the benchmark; no production use, no redistribution. The source code
is not public.

Contact: felipe@avogadrodb.com
