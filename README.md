# Gridfinity for Facet

Parametric [Gridfinity](https://gridfinity.xyz) bins and baseplates for the
[Facet](https://github.com/firstlayer-xyz) modeling language. Dimensions follow
Zack Freedman's Gridfinity spec (42 mm grid, 7 mm height units) and are designed
to interoperate with real-world Gridfinity prints.

## Use

```
var G = lib "github.com/firstlayer-xyz/gridfinity@PIN"

fn Main() Solid {
    return G.Bin(x: 2, y: 1, z: 3, divX: 2, scoop: true, label: true)
}
```

Pin `@PIN` to a commit SHA or release tag (e.g. `@v0.1.0`).

## API

### `Bin(x, y, z, divX=1, divY=1, lip=true, magnets=false, screws=false, scoop=false, label=false) Solid`

A storage bin. `x, y` are grid units; `z` is height units — total height is
`z × 7 mm` (the stacking lip is carved into the top, so it does not add height).

| param | meaning |
|---|---|
| `divX`, `divY` | split the interior into a grid of compartments |
| `lip` | stacking lip so bins stack on each other |
| `magnets`, `screws` | Ø6.5 mm magnet bores / Ø3 mm screw bores in the feet |
| `scoop` | curved scoop ramp at the front for scooping parts out |
| `label` | 45° label shelf at the back |

### `Baseplate(x, y, magnets=false, screws=false) Solid`

An `x × y` cell baseplate that bins snap into.

### Building blocks (exposed for composing custom parts)

`footSolid`, `BinBase`, `cellPockets`, `StackingLip`, `Compartments`,
`mountHoles`, `scoopRamp`, `labelTab`, `roundedPrism`.

### Constants

The spec dimensions are importable consts: `GRID` (42 mm), `HEIGHT_UNIT`
(7 mm), `GAP`, `BASE_HEIGHT` (4.75 mm), `FOOT_RADIUS`, `POCKET_RADIUS`, `WALL`,
`FLOOR`, `MAGNET_D`, `MAGNET_H`, `SCREW_D`, `SCREW_H`, `HOLE_INSET`,
`LABEL_DEPTH`. For example, to lay parts out on the grid:

```
var G = lib "github.com/firstlayer-xyz/gridfinity@PIN"
fn Main() Solid { return G.Bin(x: 1, y: 1, z: 6).Move(x: 2 * G.GRID) }
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
