# NestOpt benchmark DXF datasets

These archives contain the input geometry and settings for the NestOpt benchmark. They are organized by case so that the files can be imported into a nesting application and tested with the same quantities and constraints.

| Archive | Contents |
| --- | --- |
| `sheet-metal-benchmark-dxf.zip` | 20 sheet-metal cases: 10 standard, 5 scale and 5 featured cases. |
| `apparel-benchmark-dxf.zip` | 15 apparel cases: 10 comparison and 5 featured cases. |

## Run a case

1. Extract the archive and open `sheet-metal/cases.csv` or `apparel/cases.csv`. Find the case you want to test.
2. Import that case's `parts.dxf` as a **part library**. Each `PARTnnn` identifies one part type. In sheet-metal files it is the DXF layer name; in apparel files it is the block and piece name. Inner cut contours are included with their part.
3. Open the case's `parts.csv`. Set the **quantity**, permitted rotation angles and mirroring for each matching `PARTnnn`. DXF alone does not reliably communicate these settings to every application. The DXF contains one outline per row, not the requested number of copies.
4. Apply the stock or fabric width, length or length limit, spacing, margins, mode and other settings from `cases.csv`. Use the values in its `units` column consistently. A length limit for an apparel case is the maximum search length, not the length of the final layout.
5. Run for **60 seconds using 8 logical CPUs**, then compare sheet count for sheet-metal cases or material utilization for apparel cases. `normal` mode corresponds to the benchmark's standard mode; `fast` is fast mode. Some settings are application-specific.

For example, `sheet-metal/scale/scale-1000/parts.dxf` contains the part types for the 1,000-piece case. Its adjacent `parts.csv` specifies the quantity of each part, while `sheet-metal/cases.csv` gives the sheet dimensions and other case settings. The large-batch cases use the same convention, so their DXFs stay compact.

## Reading the files

- `cases.csv` is the index of cases and their test settings. `width` and `length_or_limit` are sheet width and sheet length for metal, and fabric width and search-length limit for apparel. Empty fields mean that a setting was not specified in that input.
- Each `parts.csv` lists quantities and per-part constraints. `allowed_rotations_degrees` lists absolute permitted angles relative to the DXF outline. A value such as `0..359 step 1` means every listed whole-degree angle in that range. `rotation_offset_degrees` and `direction_mode` preserve the source settings for software that uses them; do not apply the offset a second time to an already specified angle list.
- The apparel comparison cases use the original **input units** shown in `cases.csv`; their DXFs are marked unitless. Keep the geometry, width, spacing and precision in the same numeric units. The featured apparel and all sheet-metal cases use millimetres.
- Apparel DXF blocks share an origin because the files are pattern libraries. They may overlap in a CAD preview; this is expected and does not represent a finished layout. Sheet-metal outlines are spaced apart for easy identification.
- `apparel/featured/apparel-defects/` also contains `defects.dxf` and `defects.csv`. These files describe the two exclusion regions in fabric coordinates: X is fabric width and Y is fabric length. Set them as defects or keep-out zones; do not import them as parts.
- Results can vary between runs and hardware. In particular, the `flanges` case can require two or three sheets.

## Sources

- Sheet-metal standard and scale reference cases: [Lapas nesting benchmark v1.1](https://lapas.io/benchmarks/), Rokas Slaboševičius / Lapas (Pro Sistemos, MB). The benchmark's published result data are available under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- Apparel comparison instances: [ESICUP data sets](https://www.euro-online.org/websites/esicup/data-sets/).
- Featured cases and NestOpt test settings come from the inputs used for the NestOpt benchmark.
