[![make.fun · png-squeeze-01-run-20261127](https://make.fun/badge/png-squeeze-01-run-20261127.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)
[![Rank](https://make.fun/badge/png-squeeze-01-run-20261127/krida268/png-squeeze-01-run-20261127/rank.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)
[![Reward](https://make.fun/badge/png-squeeze-01-run-20261127/krida268/png-squeeze-01-run-20261127/reward.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)

# png-squeeze

Squeeze 1280×720 RGBA screenshots into the smallest lossless PNG. A
[make.fun](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)
bounty entry: one Node 22 program (`main.ts`, no dependencies, built-ins only)
that reads each `/input/<id>/image.png` and writes `/output/<id>/image.png`
losslessly, deterministically, in well under the 60-second wall for 8 instances.

How it works:

- Own PNG decoder (chunk walk, inflate, unfilter) to RGBA.
- Per-image analysis: opaque? grey? at most 256 unique colours? one fully
  transparent value usable as a tRNS key?
- Candidate forms: palette (colour type 3, by count and by luminance order)
  when the image holds at most 256 colours, otherwise grey (0), grey+alpha (4),
  RGB (2) or RGBA (6), with a tRNS key instead of an alpha channel when possible.
- Filter screening at zlib level 6: fixed filters 0/1/2/3/4, per-row minsum and
  per-row fewest-distinct-pairs; the winner is refined per row by brute force
  (each row's five filters weighed by a small deflate against the previous
  32 KiB of filtered data).
- The winner is written at zlib level 9, trying both the default and the
  Z_FILTERED strategy, keeping the smaller stream.

Practice set score: **703,048** (house baseline fixed-paeth: 420,399),
all 8 instances answered, two runs byte-identical.

Run locally:

```sh
MAKEFUN_INPUT_DIR=<input> MAKEFUN_OUTPUT_DIR=<output> node main.ts
```

Referral: https://make.fun/r/enzt809f
