# Snake State Space Calculator

An interactive, browser-based tool that counts every possible state of a game of Snake on an *n*×*n* board — every placement of a snake of every length, together with one piece of food — and shows how that count grows with the board's area.

## Method

A *state* is a snake (an ordered, self-avoiding path of orthogonally adjacent squares, head to tail) plus one food item on any square the snake doesn't occupy. Total states = Σ<sub>*k*=1…*n*²−1</sub> snakes(*k*) · (*n*² − *k*).

Left out: heading direction (only ambiguous for a length-1 snake), score, timing, and reachability — these are configurations, not necessarily states a real game could reach from its start position.

Brute-force path enumeration becomes infeasible around *n* = 7, so the calculator uses a frontier (transfer-matrix) dynamic program: it sweeps the board square by square, tracking only which edges cross the boundary between processed and unprocessed squares and how their loose ends pair up, carrying a length-indexed polynomial of exact integer counts per boundary pattern. All arithmetic uses BigInt, so totals are exact, not floating-point approximations. Results were checked against brute-force enumeration for *n* ≤ 6.

## Controls

| Setting | Options | Effect |
|---|---|---|
| Board size *n* | 2–10 (slider) | Grid to compute; area is *n*² |

| Button | Function |
|---|---|
| Calculate | Runs the frontier DP for every size from 2 up to *n* (reusing cached sizes), then renders the tables and chart |
| Draw another example | Redraws the handheld display with a new random snake-and-food state, without recomputing |

*n* up to 8 is instant; 9 takes a few seconds; 10 can take about a minute. Computation runs in the browser with periodic yields, tracked by a progress bar, so the page stays responsive.

## Display

The handheld readout shows one randomly drawn example state — the outlined square is the snake's head — alongside the exact total state count for the current *n*, abbreviated to fit the LCD.

Two tables and a chart summarize the results:

- **Scaling with grid area** — one row per board size up to *n*, with the total state count, its order of magnitude (log₁₀), and growth ratios against *n* = 2 and against the previous size. The chart plots log₁₀(states) against grid area *n*², which trends toward a straight line if growth is exponential in the number of squares.
- **Breakdown by snake length** — for each possible length *k*: the number of snake placements, the free squares left for food (*n*² − *k*), the resulting state count, and that length's share of the total, shown as a bar.

## Usage

A live version is available at [tmillhouua.github.io/Snake_State_Space_Calculator](https://tmillhouua.github.io/Snake_State_Space_Calculator/).

Alternatively, clone or download the repository and open `index.html` directly in a browser. The calculator runs entirely in the browser with no build step or server required.

## Dependencies

None. Computation and rendering are plain HTML, CSS, and vanilla JavaScript. The only external resources are the Atkinson Hyperlegible and VT323 fonts, loaded from Google Fonts; without an internet connection the page falls back to system fonts and otherwise works fully offline.
