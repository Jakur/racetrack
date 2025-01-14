# Racetrack

Racetrack is a cli to play a match between two Tak engines, to see how they play and measure their relative strength.

Racetrack uses the text-based TEI (Tak Engine Interface) to communicate with the engine binaries, very similar to [UCI](https://en.wikipedia.org/wiki/Universal_Chess_Interface) for Chess. The engines communicate with Racetrack by simply sending string commands through stdin/stdout, see the TEI section for more.

## TEI

Tak Engine Interface is a protocol based on [Universal Chess Interface](https://ucichessengine.wordpress.com/2011/03/16/description-of-uci-protocol/), intended to be as similar as possible. The key difference are:
* The protocol uses `tei` everywhere `uci` would be used, like `teiok` and `teinewgame`
* Additionally, `teinewgame` *requires* the GUI to send a size parameter. So `teinewgame 5` for size 5.  
* Move and position notations are obviously different. [Portable Tak Notation](https://www.reddit.com/r/Tak/wiki/portable_tak_notation) and [Tak Position System](https://www.reddit.com/r/Tak/wiki/tak_positional_system) are used, respectively.

**Engines with TEI support:**
* [Tiltak](https://github.com/MortenLohne/tiltak)
* [Taktician](https://github.com/nelhage/taktician)
* [Takbag](https://github.com/Allybag/takbag) 
* [Ctak](https://git.sr.ht/~tslil/ctak) (Partial support)

## Build

Building the project from source requires the Rust compiler and Cargo (Rust's package manager) installed, both included in the [Rust downloads.](https://www.rust-lang.org/tools/install)

The minimum required Rust version is 1.51, released March 25th 2021.

To build and run:
```
cargo build --release
cargo run --release
```

This command will automatically fetch and build dependencies. The resulting binaries are written to `racetrack/target/release`.

## Usage

Run `racetrack --help` to see a full list of options.

To play two games between Tiltak and Taktician, with 60 seconds each for each game:

````
racetrack --engine tiltak taktician --engine2-args tei --games 2 --tc 60
````

## Notes for engine developers

* Use the `--log` argument to print a full log of TEI communications for debugging.
* Racetrack uses two non-standard rules: Games are adjudicated as drawn if the exact same position is reached three times (Identical to the rule in chess), and if a game's length exceeds 100 moves.
* If an engine plays an illegal move or crashes, the game is ruled as a loss, but the tournament continues.
* Engines are not ordinarily re-started between games, except for after crashes.
* stderr output from the engines is captured, and echoed to Racetrack's stderr. If you're getting weird output, that's probably why.