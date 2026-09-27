# ☕ decaf

A terminal tool that times your Mac's charge. Plug in, run `decaf`, and it
watches the battery climb to 100% (or any target you pick), then shows how long
it took with graphs of battery %, charging power and battery temperature, in
the terminal and as an HTML report.

It's called decaf because it runs your Mac on `caffeinate` (so it can't fall
asleep mid-charge) while it waits for the battery to fill up.

```
☕  73% [########............] → 100%  61.4 W  34.2 °C  24m 10s  (charging, ~0:41 left)
```

## Install

### Homebrew

```sh
brew install Cid-Emmerich/decaf/decaf
```

Upgrade later with `brew upgrade decaf`.

### From source

decaf is a single Bash script with no dependencies beyond what ships with
macOS:

```sh
git clone https://github.com/Cid-Emmerich/decaf.git
sudo install -m 755 decaf/decaf /usr/local/bin/decaf   # or anywhere on your PATH
```

## Usage

Plug in the charger, then:

```sh
decaf                 # time the charge to 100%
decaf --to 80         # stop at 80% (handy with Optimized Charging or a charge limit)
decaf -i 10           # sample every 10 seconds instead of every 30
decaf --report ~/Documents/Decaf/decaf_20260926_191809.csv   # redraw an old log
```

While it runs you get a live line with the battery %, a progress bar, charging
power, temperature, elapsed time and macOS's own time-remaining estimate. When
the target is reached decaf:

- draws the three graphs in the terminal,
- writes an HTML report next to the log and opens it in your browser,
- shows a notification with the total time.

**Press Ctrl+C at any time** to stop early and still get the graphs and report
for everything logged so far.

### Options

| Option | What it does |
| --- | --- |
| `-t`, `--to PCT` | Stop at this battery % (default 100) |
| `-i`, `--interval SEC` | Seconds between samples (default 30) |
| `-o`, `--out DIR` | Where logs and reports go (default `~/Documents/Decaf`, or `$DECAF_DIR`) |
| `--report FILE.csv` | Rebuild the graphs and report from an earlier log |
| `--no-open` | Don't open the HTML report in the browser |
| `--no-notify` | Don't show a notification when done |
| `-q`, `--quiet` | No live view, just the final summary |
| `-h`, `--help` | Show help |
| `-v`, `--version` | Show the version |

### Running it in the background

decaf is happy in its own terminal tab. To close the terminal and let it keep
going, run:

```sh
nohup decaf --quiet &
```

The notification and browser report still appear when it finishes.

## Notes

- **Optimized Battery Charging** (System Settings → Battery → ⓘ next to
  Battery Health) and charge limits can hold the battery at 80%. decaf would
  then wait forever for 100%, so either turn them off or use `--to 80`.
- If you run decaf at or above the target, it just tells you so and exits.
- Charging power is the power going **into the battery** (voltage × current),
  so it reads 0 W when unplugged, and less than the charger's rating while the
  Mac is also drawing power.
- Logs are plain CSV (`min,pct,watts,tempC`), so you can open them in any
  spreadsheet.

## License

[MIT](LICENSE)
