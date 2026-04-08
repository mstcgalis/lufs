# lufs

LUFS analysis and track comparison for `.wav` files. Powered by ffmpeg.

```
$ lufs
(5 analyzed, 0 cached)

TRACK          INT      TP       ST MAX   LRA
───────────────────────────────────────────────
intro          -14.2    -0.3     -12.1    4.2
verse          -13.8    -0.5     -11.9    3.8
chorus         -12.1    -0.1     -10.5    2.9
bridge         -14.5    -0.4     -12.8    5.1
outro          -15.0    -0.6     -13.2    3.5

Integrated range: 2.9 LU  |  ST max range: 2.7 LU  |  TP ceiling: -0.1 dBFS

TP warnings (> -0.1 dBFS):
  chorus  -0.1 dBFS

Loudness outliers (> 1 LU from median):
  chorus  +1.7 LU
  outro   -1.2 LU
```

## Install

### Homebrew

```sh
brew install mstcgalis/tap/lufs
```

### Manual

```sh
curl -o /usr/local/bin/lufs https://raw.githubusercontent.com/mstcgalis/lufs/main/lufs
chmod +x /usr/local/bin/lufs
```

## Usage

```
lufs [-s integrated|peak|st|lra] [--no-cache] [-i|--info] [files...]
```

| Flag | Description |
|------|-------------|
| `-s, --sort COL` | Sort by column: `integrated`, `peak`, `st`, `lra` |
| `-i, --info` | Show bit depth and sample rate |
| `--no-cache` | Force re-analysis (ignore `.lufs_cache`) |
| `-v, --version` | Show version |
| `-h, --help` | Show help |

With no file arguments, analyzes all `.wav` files in the current directory.

## Features

- **Caching** — results are cached per-directory in `.lufs_cache`, keyed by file modification time. Only new or changed files are re-analyzed.
- **Sorting** — sort the table by any loudness column.
- **TP warnings** — flags tracks with true peak above -0.1 dBFS.
- **LRA outliers** — flags tracks whose loudness range deviates >3 LU from the median.
- **Loudness outliers** — flags tracks whose integrated loudness deviates >1 LU from the median.
- **Mono detection** — flags mono files with a `[MONO]` tag.
- **Format info** — `-i` shows bit depth and sample rate per track.

## Requirements

- [ffmpeg](https://ffmpeg.org/) (with ebur128 filter)
- zsh

## License

MIT
