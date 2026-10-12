# CryptoCrack

A Rust rebuild of the CryptoCrack 4.0 classical cipher solver for Linux, with a native GTK desktop, a command-line engine and a local browser interface.

- 88 cipher types with known-key decryption, 86 with blind key search, 57 with dictionary keyword search
- Cipher identification, frequency/IoC/Kasiski/n-gram statistics, word spacing, anagram and pattern search
- Cryptarithm and Sudoku solvers

## Install

```sh
git clone https://github.com/YOUR-NAME/cryptocrack.git
cd cryptocrack
sh install.sh
```

Or download the ZIP from the green **Code** button, extract it, open a terminal in the folder and run `sh install.sh`.

On a desktop this opens the setup wizard: choose who the install is for, the program and data folders, and whether to add a menu entry and desktop icon. It installs:

- **CryptoCrack** in your applications menu, or `cryptocrack` in a terminal
- `cryptocrack-cli`, the command-line engine
- sample ciphers in `~/CryptoCrack/Ciphers`

Other options:

- `sh install.sh --yes` installs for your user with no questions (also works over SSH or without a desktop)
- `sh install.sh --uninstall` removes it; your data folder is kept
- Run `sh install.sh` again to update

On x86-64 the installer in `installer/` includes everything, GTK too, so nothing is downloaded. On other processors (ARM, Raspberry Pi) `install.sh` builds from source; it asks before installing Rust and the GTK build packages.

## Build

Needs Rust and GTK 3 development files (`libgtk-3-dev` on Debian/Ubuntu/Kali).

```sh
cd rust
cargo build --release --features desktop --bins
cargo test --release --features desktop
./target/release/cryptocrack-desktop          # window
./target/release/cryptocrack-linux --help     # command line
```

`./run-linux.sh` and `./run-browser.sh` build on first use.

Rebuild the installer in `installer/` (also needs `cargo-zigbuild`, `zig` and `zip`):

```sh
python3 packaging/build_setup.py installer
```

## Command line

```sh
cryptocrack-cli --list
cryptocrack-cli decrypt vigenere --text LXFOPVEFRNHR --key LEMON
cryptocrack-cli solve vigenere --file cipher.txt
cryptocrack-cli solve playfair --file cipher.txt
cryptocrack-cli solve polybius --file cipher.txt --rows ABCDE --columns ABCDE
cryptocrack-cli solve substitution --file cipher.txt --crib SECRET
cryptocrack-cli identify --file cipher.txt
cryptocrack-cli words --text THISISASECRETMESSAGE
```

`--period` sets key length, block period or rails; `--restarts`, `--iterations` and `--seed` control the search budget. Blind searches are heuristic: run again or raise the budget if a result is not readable.

## Layout

| Path | Contents |
| --- | --- |
| `rust/` | engine, CLI, GTK desktop, tests |
| `desktop/main.ui` | desktop layout |
| `original/EnglishData/` | English scoring tables and dictionaries from CryptoCrack 4.0 |
| `validation/` | reference vectors used by the tests |
| `install.sh` | installer entry point |
| `installer/` | prebuilt Linux x86-64 installer (setup wizard + bundled GTK runtime) |
| `packaging/` | install wizard source, text installer, bundle build script, sample ciphers |

The English data files come from the original CryptoCrack 4.0 and remain the property of its author.
