# Gridfinity for Facet

Parametric [Gridfinity](https://gridfinity.xyz) bins and baseplates for the
[Facet](https://github.com/firstlayer-xyz) modeling language. Dimensions follow
Zack Freedman's Gridfinity spec (42 mm grid, 7 mm height units) and are designed
to interoperate with real-world Gridfinity prints.

## Use

```
var G = lib "github.com/firstlayer-xyz/gridfinity@v0.3.0"

fn Main() Solid {
    return G.Bin{x: 2, y: 1, z: 3, divX: 2, scoop: true, text: "M3"}.Solid()
}
```

Pin `@v0.3.0` to a commit SHA or release tag (e.g. `@v0.3.0`).

## API

### `Bin{...}.Solid() Solid`

A storage bin, built as a struct and rendered with `.Solid()`. `x, y` are grid
units (required); `z` is height units — the body is `z × 7 mm` tall and the
stacking lip adds `LIP_HEIGHT` (4.4 mm) on top, which a stacked bin's feet drop
into. Every other field has a default.

```
G.Bin{x: 3, y: 1, z: 4, text: "M3",
      bodyColor: Color(hex: "#333"), labelColor: Color(hex: "#fff")}.Solid()
```

| field | meaning |
|---|---|
| `x`, `y`, `z` | size: `x × y` grid cells, `z` height units (required) |
| `divX`, `divY` | split the interior into a grid of compartments |
| `lip` | stacking lip so bins stack on each other (default `true`) |
| `magnets`, `screws` | Ø6.5 mm magnet bores / Ø3 mm screw bores in the feet |
| `scoop` | curved scoop ramp at the front for scooping parts out |
| `label` | 45° label shelf at the back |
| `text` | text on the label shelf, auto-sized to fit (a non-empty `text` adds the shelf) |
| `emboss` | signed relief for `text`: `< 0` engraves (default `-0.6 mm`, flush with the rim), `> 0` raises it, `0` leaves the shelf flat |
| `font` | optional `.ttf`/`.otf` path for `text` (defaults to the built-in font) |
| `bodyColor`, `footColor`, `lipColor`, `labelColor` | optional `Color` per part; unset (nil) leaves a part its default material |

`labelColor` colours only the text's top readable layer (the raised cap, the
engraved floor, or the flush inlay, depending on `emboss`); the relief that
connects it to the bin keeps `bodyColor`.

### `Baseplate(x, y, magnets=false, screws=false) Solid`

An `x × y` cell baseplate that bins snap into. Plain, it is a thin "lite" frame.
Enabling `magnets`/`screws` raises the receptacles onto a solid deck so the
bores have material to hold the hardware: Ø6.5 mm magnet pockets open at the
mating floor (under each seated foot) and Ø3 mm screw bores pass through.

### Building blocks (exposed for composing custom parts)

`footSolid`, `BinBase`, `cellPockets`, `StackingLip`, `Compartments`,
`mountHoles`, `baseplateHoles`, `scoopRamp`, `labelTab`, `labelGlyphs`,
`roundedPrism`, `colorize`.

### Constants

The spec dimensions are importable consts: `GRID` (42 mm), `HEIGHT_UNIT`
(7 mm), `GAP`, `BASE_HEIGHT` (4.75 mm), `FOOT_RADIUS`, `POCKET_RADIUS`, `WALL`,
`FLOOR`, `MAGNET_D`, `MAGNET_H`, `SCREW_D`, `SCREW_H`, `HOLE_INSET`,
`LABEL_DEPTH`, `LABEL_TEXT_MARGIN`, `LABEL_TEXT_LAYER`. For example, to lay
parts out on the grid:

```
var G = lib "github.com/firstlayer-xyz/gridfinity@v0.3.0"
fn Main() Solid { return G.Bin{x: 1, y: 1, z: 6}.Solid().Move(x: 2 * G.GRID) }
```

## Dimensions

| | value |
|---|---|
| grid pitch | 42 mm |
| height unit | 7 mm |
| base height | 4.75 mm (0.8 + 1.8 + 2.15) |
| bin↔baseplate clearance | 0.5 mm total |
| foot outer radius | 3.75 mm |

Values cross-checked against `kennetek/gridfinity-rebuilt-openscad`.

## License

MIT (see [LICENSE](LICENSE)). Gridfinity is a system by Zack Freedman.
