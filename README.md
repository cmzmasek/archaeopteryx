# Archaeopteryx

**The desktop viewer for phylogenetic figures worth publishing.**

Archaeopteryx is an offline application for visualizing, annotating and analyzing
phylogenetic trees, built for publication-quality figures. It reads phyloXML,
Newick / New Hampshire (NH/NHX) and Nexus — including annotated
[**BEAST / BEAST X** output](#beast-and-beast-x-output) and the trees written by
[**MrBayes, TreeTime and Nextstrain**](#mrbayes-treetime-and-nextstrain-nexus) — and
brings together taxonomy and sequence annotation, protein-domain architectures,
calendar and geologic time axes, and WYSIWYG vector (PDF / SVG / EPS) export.

**→ [cmzmasek.github.io/archaeopteryx](https://cmzmasek.github.io/archaeopteryx)** ·
**[Download the latest release](https://github.com/cmzmasek/archaeopteryx/releases/latest)**

> **Prefer the browser?** **[Archaeopteryx.js](https://cmzmasek.github.io/archaeopteryx-js/)**
> is the online version: nothing to install. It shares this viewer's file formats,
> "Color by" rules and node-data card, so a tree looks the same in both.

---

## Highlights

- **Integrated annotation** — UniProt / NCBI taxonomy, sequence data and
  protein-domain architectures, straight onto the tree.
- **Calendar & deep time** — tip-dated calendar axes for molecular epidemiology, and
  the full ICS geologic time scale for the fossil record.
- **Publication vector export** — WYSIWYG PDF / SVG / EPS from the renderer that draws
  the screen.
- **Undo & provenance** — every edit is undoable, and every tree-changing operation
  records what it did.
- **Large trees, five layouts** — rectangular (three orientations), circular and
  unrooted, with tip-aligned annotation columns that become rings in circular.
- **Reads what you have** — Newick, NHX, Nexus, phyloXML, Nextstrain / Auspice JSON
  and Nexus, tip-dated labels, and BEAST, MrBayes and TreeTime annotations.

**File → Demo Trees** has a pre-configured example of each.

---

## Install

Download the installer for your platform from
**<https://github.com/cmzmasek/archaeopteryx/releases/latest>**. Each bundles its own
Java 21 runtime — nothing else to install.

> The apps are not code-signed or notarized yet, so each platform shows a
> first-launch security prompt (steps below). Getting past it once is enough.

### macOS

Take `Archaeopteryx-<version>-apple-silicon.dmg` for Apple Silicon or
`Archaeopteryx-<version>-intel.dmg` for Intel (Apple menu → **About This Mac**: a
**Chip** line means Apple Silicon, a **Processor** line Intel). Open it and drag
**Archaeopteryx** into **Applications**. On the first launch, **right-click** the app
and choose **Open**, then **Open** again; after that, launch it normally.

### Windows

Run `Archaeopteryx-<version>.msi` (it adds a Start-menu entry, optionally a desktop
shortcut, and lets you choose the folder). If SmartScreen warns, click **More info →
Run anyway**.

### Linux (Debian / Ubuntu)

```
sudo apt install ./archaeopteryx_<version>_amd64.deb
```

(or `sudo dpkg -i …`), then launch it from the applications menu or as `archaeopteryx`.

### Staying up to date

**Archaeopteryx has no server, and never will.** It does not phone home and collects
nothing. **Settings → Application → Check for Updates at Launch** (off unless you turn
it on) reads the public releases page of its repository once, after starting; if a
newer version exists, the **Help** menu gains a first line, *New version available:
x.y.z*, linking to it. No network or a failed check: nothing happens. Nothing about
you, your trees or your machine is sent — the request is a plain read of
<https://github.com/cmzmasek/archaeopteryx/releases>.

### If a tree feels slow to draw

**Settings → Application → Diagnostics → Show paint-time / FPS counter** (remembered
once set) shows in the top-right corner how long the last frames took to paint. It
measures the cost of a frame, not how often the window redraws, and never appears in
an exported figure. Use it to tell an expensive tree from a slow machine: switching a
track off shows at once what it cost.

### Run from the jar

With no installer for your platform, or for a single self-contained file, run the jar
with **Java 21 or newer** (`java -version`; otherwise install a free OpenJDK such as
[Eclipse Temurin](https://adoptium.net/temurin/releases/?version=21): `brew install
--cask temurin@21`, `winget install EclipseAdoptium.Temurin.21.JDK`, or `sudo apt
install openjdk-21-jre`). The jar bundles every library it needs:

```
curl -L -o forester.jar https://github.com/cmzmasek/forester/raw/master/forester/java/forester.jar
java -jar forester.jar                 # or open a tree directly:
java -jar forester.jar mytree.xml
java -Xmx4g -jar forester.jar big.xml  # more memory for a very large tree
```

---

## Try it: demo trees

**File → Demo Trees** opens example trees, each pre-configured to show a capability
(the demos are bundled in the jar; the same trees and more are in
[`forester/demo/`](https://github.com/cmzmasek/forester/tree/master/forester/demo)):

- **Color Tips by Metadata** — colored by a categorical property
- **Annotation Columns** — tip-aligned color strips and a numeric heat map
- **Symbol Columns** — tip-aligned marks: a present value filled, an explicit
  "no"/absent value hollow, a missing value nothing; a categorical field in distinct
  colors; the glyph (circle / square / diamond / triangle) chosen per column
- **Properties in Labels** — six properties per tip: two in the label, four as
  columns. One field, one role
- **Stacked Bar Columns** — ten microbiome samples with three read counts
  (*Firmicutes*, *Bacteroidetes*, *Proteobacteria*) as one segmented bar per tip: its
  length the total, its segments the composition. **Normalize stacked bars to 100%**
  (**Tools → Annotation Fields…**) compares composition alone
- **Pie Columns** — the same samples as one pie per tip
- **Pangenome Presence/Absence (Clustergram)** — 100 strains × 40 genes, a table of
  presence certainty (0–4; blank = not assessed) shown as a clustergram. Set **View →
  Order Matrix Columns → Same as Table** and the gene classes read as bands (see
  [A heat-map matrix and its column order](#a-heat-map-matrix-and-its-column-order))
- **Sparse Accessory Genome (Two Clustered Orders)** — 50 strains × 18 genes whose
  eight rare genes share no strain. **Clustered (co-occurrence)** clumps them, as
  Euclidean distance counts shared absence as agreement; **Clustered (ignoring shared
  absence)** puts each prophage beside its lineage's capsule locus
- **Protein Domain Architectures** — domains drawn to scale as rounded, colored boxes,
  labelled on the boxes or in a draggable, E-value-aware legend (**Settings → Layout →
  Domain labels**); in circular and unrooted views they ride each tip's spoke as a ring
  when **Radial Labels** are on. A domain whose `to` is not greater than its `from`, or
  that lacks a coordinate, is skipped, and the count skipped is reported on opening
- **Alignment next to Tree** — six vertebrate proteins drawn beside the tree; hover a
  residue for its column, position and properties (see [Sequence alignment](#sequence-alignment))
- **Bat Phylogeny (Taxonomy by Rank)** — 34 species with common and scientific names
  and synonyms, every clade rank-annotated, colored by family (offline)
- **Animal Tree of Life (Nested Clade Levels)** — 25 animals with order, class and
  phylum annotated at once as nested bars in shades of their phylum
- **GTDB Taxonomy (Genome-based)** — Bacteria and Archaea with a GTDB-Tk
  classification, colored by phylum (offline)
- **Ancestral State Pies** — a discrete geographic trait as posterior pies
- **Node Age Spindles** — divergence-time uncertainty (point age + 95% HPD)
- **Tree Properties** — a gene family whose **Tree Properties** window (View menu, ⌘I)
  fills every section: name, description, metadata, statistics with histograms, and
  which tips carry which data
- **Break Long Branches** — a fast-evolving outgroup drawn shortened with a break mark
- **SARS-CoV-2 Time Tree** — tip-dated, on a calendar-year axis
- **Phylodynamics (Nextstrain JSON)** — an Auspice v2 dataset with geographic
  ancestral-state pies
- **Filoviridae (Ebola & Marburg)** — a real filovirus phylogeny colored by species,
  with host / country / year metadata and per-protein accessions
- **Dinosaur Time Tree** — archosaurs (with *Archaeopteryx*!) on the geologic scale
- **Late Cretaceous Dinosaurs** — narrow enough that the axis bands the *stages*
- **Lagomorph Time Tree** — 18 living rabbits, hares and pikas back to the Eocene
- **Ammonite Time Tree** — an all-extinct fossil clade with FAD/LAD range bars
- **Tree of Life (Deep Time)** — back to LUCA (~3.8 Ga)
- **Tanglegram** — gophers and their lice, side by side

---

## Display controls

Every button in the **control panel** on the left is a drawn icon — one click each, no
dialog. **Help → Control Panel Cheat Sheet** lists every control currently on screen
beside its icon and tooltip; it is built from the panel itself, so it cannot fall out
of step, and shows only what *this* tree has. **Save as PNG…** prints it as a one-page
reference.

**Theme.** The **sun / moon** button switches light and dark themes, showing the theme
it will switch *to*. The canvas follows, and the choice is remembered.

**Layout — the five display types.** One row, one exclusive group:

| button | layout |
| --- | --- |
| tree pointing right | rectangular, **root at left** (the classic view) |
| tree hanging down | rectangular, **root at top** |
| tree growing up | rectangular, **root at bottom** |
| open ring with spokes | **circular** |
| free-form star | **unrooted** |

Any layout is one click from any other, and all five are first-class: annotation
columns, clade bands, time axes, HPD and range bars, tip images and vector export work
in each.

**Tree share — how much of the width the tree keeps.** Tip labels, the domain track,
the alignment, annotation columns and the legend column all compete for the width.
The tree takes its share **first** and the tracks divide the rest, so a heavily
annotated tree stays a tree. The **Tree share** slider (under *Node size*) sets it from
25% to 80% (40% by default, remembered). In rectangular layouts it divides the depth
**width**, in circular the tip-ring **radius**, in unrooted the **fan spread**.

- The alignment gives way first — it scrolls, so a narrower window shows fewer columns.
  The domain zoom (**d-** / **d+**) trades against it. Clade bands and the legend column
  are never shaved.
- Tip labels first shrink their font; only at the smallest readable size are they
  **shortened with an ellipsis**, and only if that widens the tree by at least 1% (the
  slider's step). No demo tree is shortened at the default share.
- The share is a target, not a floor: on a 527-tip tree with domains *and* an
  alignment, asking for 80% gives the tree about 55%. It is never left nothing.
- With nothing competing for the width, the slider greys out and says **(no effect)**.

**Phylogram / cladogram.** Three buttons, each drawn with its tip labels, because that
is where the difference shows:

| button | what it draws |
| --- | --- |
| ragged branches, ragged labels | **phylogram** — branch lengths to scale, tips end ragged |
| ragged branches, labels in a column | **aligned phylogram** — the same, labels carried to a common column |
| flush branches, labels in a column | **cladogram** — topology only, all tips flush |

Each radial layout draws only one phylogram and greys the other: **unrooted** has no
column to align to, so the aligned phylogram is greyed; **circular** always carries its
labels to the outer ring on dotted leaders, so the *plain* phylogram is greyed
(Archaeopteryx.js does the same). Your choice is kept per tab and returns in a
rectangular layout. A tree without branch lengths can only be a cladogram.

**Rectangular style** (**Settings → Layout → Rectangular style**): **Square** (the
default), **Euro Type** (slanted corner), **Rounded** or **Triangular** (clades as
triangles). Set while a radial tree is shown, it takes effect in the next rectangular
layout.

**Zoom, fit and navigate:**

| button | what it does |
| --- | --- |
| **Y+ / Y−** | zoom in / out vertically |
| **X− / X+** | zoom in / out horizontally |
| label rows pushed apart | **expand** along the label axis until labels stop overlapping (`Alt+E`) |
| square frame with arrows | **fit** the whole tree (`Alt+C`, `Home` or `Esc`) |
| landscape frame with arrows | **fit to the window width**, keeping the vertical zoom (`Alt+W`) |
| small ladderized tree | **ladderize**; click again to flip (`Alt+O`) |
| arrow into a bar | back to the **complete tree** from a sub-tree (`Alt+Shift+R`) |
| plain left arrow | **up one level** (`Alt+R`) |
| triangle with tip lines | **uncollapse all** (`Alt+U`) |

The buttons follow the layout. **Root at top / bottom:** fit-to-width becomes
*fit to height*, and the expand glyph turns. **Circular and unrooted:** **X− / X+**
become **rotate counter-clockwise / clockwise** (also `A` / `S`, or `Shift`+wheel);
fit-to-width becomes the **node-label direction** toggle — along the spoke (the
default, where ring neighbours never meet) or flat — showing the state it switches
*to*; expand is greyed. A button that cannot act right now fades rather than vanishing,
so the row never changes shape.

**Collapsed clades** are drawn as a **wedge** from the node to its nearest and
farthest tips, so the shape still shows how uneven the clade is (one step deep in a
cladogram). A wedge takes more room for a bigger clade, never more than two and a half
rows, and is filled in the colour most of its tips wear under **Color by**. Its label
stands where a tip label would and names the clade: the node's name; else a Color-by
value at least 95% of its tips share ("Cow · 12 tips"); else the tips' shared name
prefix; always with the tip count. A search hit inside outlines the wedge in the search
colour and adds `[found/total]`; all tips hit fills it and bolds the label. A clade
holding a hit stays bright under **Dim Non-Matches**. Archaeopteryx.js draws collapsed
clades the same way.

**Auto-hide Labels** (*Display Data*, on by default) hides a mark only where something
already drawn is really in its way: a tip or clade label over another label (tip names
first, then clade names, larger clades first; a search hit's label always drawn); a
**support or branch-length number** over another number, a label, or — in circular and
unrooted — another branch; **support symbols** once rows are closer than the symbols.
So nothing is dropped that could have been read, and a lone zero-length branch keeps
its support value. Zoom in and marks come back; untick it to draw everything. **Expand**
is the alternative — worth doing before an export, since a figure exported while marks
are hidden lacks them (the export report says so).

**Which node am I on?** Pointing at a node gives it a soft **glow** — in every **Click
on Node to:** mode, collapsed wedges included — and pausing brings up the
[node card](#viewing-and-editing-node-data). The glow's colour says what a click will do:

| glow | meaning |
| --- | --- |
| the node's own colour, else a neutral accent | you are on this node (any non-selection mode) |
| the found colour | in **Select Node(s)**, a click will **add** this node |
| muted grey | in **Select Node(s)**, a click will **remove** it |

"The node's own colour" is whatever it is drawn in — Color by, a node style, an event
colour, a colorized clade. In Select Node(s), pointing at a *branch* glows its clade
root and marks the tips a click would take; over a collapsed wedge the glow stands for
the whole clade. The glow never appears in an exported figure.

### Long tip labels ("Shorten Labels")

**Shorten Labels** (*Display Data*, on by default for long names) makes database-style
names readable without touching the data. It drops the **prefix every tip shares** —
counted as shared when at least 95% of tips carry it; the rest keep their full names —
then, if the rest is still long, keeps the **first and last eight characters**:

```
Influenza A virus (A/mallard/Sweden/1/2010)   ->   mallard/../1/2010)
```

Both ends stay because neighbouring strains usually differ at both. It is display
only: search, export and accession parsing see the full name. Archaeopteryx.js shortens
the same way.

---

## Support values in Newick and Nexus files

Newick and Nexus carry branch support as a bare internal label — `)100:0.05` — with
nothing to say whether `100` is a support value or a clade name. **When every internal
label looks like a support value** (bootstrap percentages, posterior probabilities, a
0–1000 scale), they are read as confidence values, and everything that works on
support works: colouring by it, support symbols, collapsing weak branches. A tree whose
labels are names, or that mixes names and numbers, is left alone — a genuine name is
never turned into a number.

**Settings → Files → "Treat internal labels as confidence values"**: **Auto** (the
default: only when they *all* look like support), **Always** (every numeric label — the
only choice that helps a mixed tree), **Never**. (The old **"Internal Node Names are
Confidence Values"** checkbox is gone; its equivalent is **Always**.)

**MAD values are not support.** **Tools → MAD-Root** (Tria, Landan & Dagan 2017) gives
every internal branch its minimal ancestor deviation — how far from a clock the tree
would be if rooted there, *lower is better*. **MAD Confidence Values** shows them before
any support value (`0.12/90`); phyloXML keeps them (`type="MAD"`), and rerooting any
other way removes them. Support symbols, colouring and collapsing by support, the
Support / Confidence search and the support statistics all ignore them, and Newick /
Nexus write the branch's real support.

### What gets written back out

**Support values are saved by default**, in square brackets — `)0.3[95]` — and read back
the same way. (Before 0.11.157 a plain *Save As* dropped them.) **Settings → Files →
Newick / Nexus Saving** can write them as **internal node names** instead, or leave
them out.

**Nexus files carry their sequences** as a `Characters` block, and opening the file
brings them back. No matrix is written — and a comment in the file says why — when the
sequences differ in length (not an alignment) or two tips share a name. A tip without a
sequence gets a row of `?` (missing, not a gap).

**A tip with nothing to name it** — no name, taxonomy or sequence — is written as
`node1`, `node2`, … by position, so the file stays readable and the Nexus taxon list
complete. Internal nodes without labels stay unlabelled.

---

## Coloring tips by their data ("Color by")

**Color by:** (left panel) colors every tip by one field — taxonomy (code, scientific
name, common name), sequence (name, symbol, gene name), or any node property (`host`,
`country`, `year`, …). A category gets one color per value; a number individual colors
or a gradient. [Archaeopteryx.js](https://github.com/cmzmasek/archaeopteryx-js) uses the
same rules, ids and labels, so a tree colors identically in both.

**Which fields are offered**, best first: clean categories; numeric fields; very wide
categories (more than 20 values); *In-Group* / *Out-Group* fields; sparse fields (on
fewer than two thirds of the tips); categories whose values barely repeat. Within each
group, wider and more even coverage first. **Never offered:** a single-valued field; a
text field with a different value on every tip (an identifier); a field some tip
carries twice; and fields about the *record* rather than the organism — ids,
accessions, taxon ids, authors, sets, data-use terms, embargo (*restricted until*)
dates.

**A tree opens already colored** by the first offered field that is not a very wide
category. A figure saved with the tree keeps its coloring, and a field you chose is
never overridden. **Settings → Labels & Colors → Auto-color a newly opened tree** turns
this off.

**The legend** is a draggable card (double-click sends it home). Each row shows a color
and its tip count; `[by count]` / `[A-Z]` flips the order; long legends show the top 20
with *show all*. Tips with **no value** are counted in a dashed-circle row pinned last
(they draw no dot). Click a row to give that value your own color; *Use Automatic
Color* undoes it.

**The legend gets a column of its own.** Its home corner is where a root-left tree puts
its top tips and a matrix its headers, so a strip at the right is reserved for it and
the tree laid out in the rest — the window then shows what **Export as PDF** and
`aptx_render` draw. A legend dragged elsewhere gives the column back, and the column is
never taken when it would cost more than 40% of the width. Off: **Settings → Layout →
Legend in Its Own Column**.

**Numeric fields.** A field is numeric only when **every** value is a plain decimal
(`12`, `-0.5`, `1e3`); `0x1A` or `Infinity` make it a category. Spellings of one number
(`1`, `1.0`, `+1`) are one value. Up to ten distinct numbers are treated as codes (H5N1
vs H5N2) with individual colors; more become a **gradient**. Up to twenty, a
`[colors]` / `[gradient]` legend control flips between the two.

**Values are grouped for coloring** — spelling variants (`Human` / `human` /
`homo_sapiens`), `host` qualifiers after `;`, `country` subdivisions after `:`
(`USA:CA` = `USA:IL`), and unambiguous animal synonyms (`swine` / `porcine` / `Sus
scrofa` → **Pig**; `bovine` / `cattle` → **Cow**; `Homo sapiens` → **Human**; …). A value
of only underscores, or only a qualifier, is no value. Display only: stored values,
search, exports and the node window keep them verbatim. Taxonomy and sequence fields are
never grouped.

**Colors are stable.** A value keeps its color through subtrees, collapses and deletions
— the legend follows what is on screen, nothing recolors. A subtree never changes the
offered fields or the chosen one, and edits or undo keep your field while any tip
carries it. (A gradient always spans the visible range.) Switching the palette
(**Settings → Labels & Colors**: Default or Colorblind-friendly) or **Reset to Defaults**
reassigns colors.

**Size by** scales each tip symbol by a numeric field, so one symbol can encode two
fields.

---

## Annotation fields: columns or labels

Tips often carry a host, a country, a clade, a year, an accession, a passage history.
Archaeopteryx reads them all (phyloXML `<property>` elements, **File → Import
Annotations**, BEAST or Auspice files) and **Tools → Annotation Fields…** decides how
each is shown.

**Import Annotations** follows one convention in both viewers: a column becomes a
`meta:` property named after its header, spaces written `_` (menus show "Collection
Date"); an `ns:name` header is kept as is; a column whose every filled cell is a number
is stored as a number; the table's value replaces the tip's, an empty cell changes
nothing. Tab-, comma- or semicolon-separated, fields may be double-quoted, `#` lines
are skipped.

**Each field gets exactly one role:** a tip-aligned column, part of the label, or not
drawn. Ten fields crammed into a label is unreadable; ten thin strips beside the tips is
a figure.

### The roles

| "Show as" | what it draws |
| --- | --- |
| **Color strip** | a filled cell per tip, coloured by category |
| **Symbol** | a glyph — filled when present, hollow when explicitly `no`/`absent`/`0`/`false`, nothing when missing; circle / square / diamond / triangle per column |
| **Heat map** | a cell coloured by the value's place in the numeric range |
| **Heat map (matrix)** | as above, all matrix columns sharing **one** scale and legend, drawn as a contiguous grid (a clustergram) |
| **Bar** | a bar whose length is the value's fraction of the range |
| **Stacked bar** | several numeric fields merged into one segmented bar per tip — absolute, or normalized to 100% |
| **Pie** | several numeric fields merged into one pie per tip |
| **Text** | the raw value as a column of text |
| **In tip label** | the value appended to the label |

The roles follow the data: numeric fields get heat map / bar / stacked bar / pie,
categories get strips and symbols. A field that cannot usefully be *coloured* (one
value everywhere, or a different one on every tip, like an accession) is offered as
text or label only; a field only on internal nodes can only go in the label.

### In the label

Label properties are drawn as **values only**, comma-joined — `EPI1731, E3` — and the
full list is one hover away in the [node card](#viewing-and-editing-node-data) and in
**Display Node Data**. The **↑ / ↓** buttons set the order of the columns and of the
label; a column can also be dragged by its header (a matrix can order itself — see
[below](#a-heat-map-matrix-and-its-column-order)). Choosing a label field ticks
**Properties** for you. By default every field not shown as a column is in the label;
**Settings → Reset to Defaults** restores that.

### A heat-map matrix and its column order

**View → Clustergram** builds one grid from a table of numbers per tip (gene
presence/absence, abundance, expression): numeric fields become **Heat map (matrix)**
columns on one scale, categorical fields colour strips (single-valued fields are left
out), and the tree is laid out root-left with aligned tips — dendrogram, labels,
matrix, column names across the top. The matrix is drawn in the rectangular layouts and,
as rings, in circular.

**View → Order Matrix Columns** sets the column order for the tab:

| Order | the columns are placed … |
| --- | --- |
| **Clustered (co-occurrence)** — the default | so that columns whose values agree across the tips sit together |
| **Clustered (ignoring shared absence)** | the same, but a tip where *both* columns are 0 is left out — columns sit together because they occur in the same tips, not because they are missing from them |
| **Same as Table** | as the file or imported table lists them |
| **Alphabetical** | by name, ignoring case |
| **Frequency** | highest mean over the tips with a value first (on 0/1 data, the fraction carrying it) |
| **Manual** | where you put them |

Only matrix columns move, and only among matrix places. A blank cell (*not assessed*)
never counts as 0. Hover a cell to read the tip, the column and the number (see
[Hovering a cell](#viewing-and-editing-node-data)).

**The clustered orders draw their tree** above the column names, each merge at its
height, to scale. Read it: neighbouring columns can belong to different clusters, and
only the dendrogram tells them apart. It disappears when you move a column by hand, and
is not drawn in circular.

**Clustered** is complete-linkage clustering on Euclidean distance — the clustered heat
map, and the default of R's `pheatmap`, `heatmap.2` and `ComplexHeatmap`. A tip missing
either value is left out of that pair and the rest scaled up (R's `dist()` rule), giving
the order of R's `hclust(dist(t(m)), method = "complete")`, so a figure can be checked
against R. Two columns with no shared assessed tip join last.

Euclidean distance counts two genes *absent* from the same strains as alike — the
**double-zero problem** — so rare genes can clump while sharing no strain.
**Clustered (ignoring shared absence)** uses the **Bray–Curtis dissimilarity**,
`sum|x − y| / sum(x + y)` over tips where both have a value (the default of R's
`vegan::vegdist()`): a tip where both are 0 drops out, so two rare genes never together
come out maximally distant (1). On 0/1 data it is the **Sørensen–Dice** dissimilarity.
It is for values of 0 or more (counts, abundances, the 0–4 certainty scale); missing
cells drop out pairwise.

Use **co-occurrence** when 0 is an informative measurement and you want R's arrangement;
**ignoring shared absence** for sparse matrices — a pan-genome, presence/absence.
[`sparse-accessory-genome.xml`](https://github.com/cmzmasek/forester/blob/master/forester/demo/sparse-accessory-genome.xml)
shows the difference. If your table already groups its columns meaningfully, **Same as
Table** shows the groups as bands.

**Drag a column's header** (in circular, its ring) to move it — a marker shows where it
lands, and a plain click still shows its legend — or use **↑ / ↓** in **Tools →
Annotation Fields…**. Moving a matrix column switches the tab to **Manual**; moving it
back, or moving only a colour strip, does not. A saved figure reopens with its columns
as saved, in **Manual**. **Reset to Defaults** returns every tab to **Clustered**.

- Complete linkage: Sørensen T (1948): "A method of establishing groups of equal
  amplitude in plant sociology based on similarity of species content and its
  application to analyses of the vegetation on Danish commons", *Biologiske Skrifter*
  5(4):1–34.
- The clustered heat map: Eisen MB, Spellman PT, Brown PO, Botstein D (1998): "Cluster
  analysis and display of genome-wide expression patterns", *PNAS* 95(25):14863–14868,
  doi:10.1073/pnas.95.25.14863.
- The Bray–Curtis dissimilarity: Bray JR, Curtis JT (1957): "An ordination of the upland
  forest communities of southern Wisconsin", *Ecological Monographs* 27(4):325–349,
  doi:10.2307/1942268.

**Where it works.** Columns are drawn in the three rectangular orientations and in
circular (as rings, with the legend). **Not in unrooted**: its tips sit at different
radii in no fixed order, so there is no edge to line cells up against. The tab keeps its
columns and draws them again when you switch back. Fields **in the tip label** work in
all five layouts.

## Your figure is saved with the tree

Save a tree as **phyloXML** and its figure is stored with it and restored on reopening:

- the **layout** — rectangular (root left, top or bottom), circular or unrooted — and
  phylogram, aligned phylogram or cladogram
- **which labels are drawn**: every Display-panel checkbox
- the **annotation columns**, with their types and symbol shapes
- the **clade marks**, with their ranks and label angles
- **colour by**, **size by**, the **ancestral-state pie** trait, and the property fields
  shown in the labels

Each tab keeps its own figure. The **×** on a tab asks before discarding unsaved
changes, like **File → Close Tab**.

- **Only phyloXML carries it** — Newick and Nexus have nowhere to put it.
- **The theme is not part of it** — fonts, colours and light/dark stay the reader's own.
- **A figure from a newer version still opens**; anything an older Archaeopteryx cannot
  draw is skipped.

### Clearing every overlay at once

**Tools → Clear All Overlays** switches off annotation columns, clade marks, colour by,
size by, ancestral pies and the properties in the labels in one action, and nothing
else: layout and labels stay.

## Viewing and editing node data

Two **Click on Node to:** modes open a node's data — taxonomy, sequences, support, a
date, a distribution, a reference, properties — in a window of its own: **Display Node
Data** (read-only) and **Edit Node Data**. The page is one scrolling list of foldable
sections — **Basic**, **Taxonomy**, **Sequences**, **Events** (internal nodes),
**Date**, **Distribution**, **Reference**, **Properties** — with data-bearing sections
open. The header says what the node is: external or internal, children and tips, depth,
distance from the root.

- **Nothing reaches the tree until Write to Tree** (⌘↩ / Ctrl+Enter, or Enter in a
  field); the title shows **•** while changes are unwritten, and **Close** asks whether
  to write, discard or stay.
- **Values are checked as you type** — a branch length that is not a number, a taxonomy
  code that is not 3–5 capitals, an unknown rank, a bad DOI or URL, a latitude beyond
  ±90 — and **Write to Tree** stays disabled until all are valid.
- **Only what you changed is written.** Untouched branch lengths keep every digit; data
  the editor does not show (sequence annotations, lineages, polygons) survives.
- **Several sequences per node** (a protein and its mRNA): one card each, **+ Add
  sequence** / **×**. The sequence box counts residues and cleans pasted text on write.
- **Properties are a table** — reference, value, unit, datatype, applies-to — with
  **+ / − property**. References and units need a namespace prefix (`data:depth`,
  `METRIC:m`); an `xsd:decimal` value must be a number. *Color by*, *Size by* and the
  annotation columns see a new property at once.
- **Emptying a section removes it.**
- **One write is one undo step**, recorded in the tree's description.

## Tree properties, statistics, and the tree as text

**View → Tree Properties…** (⌘I / Ctrl+I, or double-click the tab) opens the tree's own
page. Editable at the top: the **name** (also the tab title), the **description** (the
tools append to it when they change the tree), and phyloXML metadata — an
**identifier** and provider, the tree **type**, the **branch-length unit**, and
**re-rootable** (untick it and the rooting tools grey out). The editing rules are those
of the node window.

Below, computed live:

- **File** — path, format (sniffed, not guessed from the suffix), size, modified,
  unsaved changes.
- **Structure** — tips, internal nodes, branches, rooted, binary or how many
  polytomies, depth, height, collapsed clades.
- **Branch lengths** — how many, median, mean ± sd, min, max, total, zero-length and
  negative branches, ultrametric, and a histogram (hover a bar).
- **Support values** — per kind (bootstrap, posterior, …), with its own histogram.
- **Annotation coverage** — how many tips carry taxonomy (and an identifier), distinct
  taxonomies, sequences, molecular sequences, domain architectures, dates,
  distributions, references; named internal nodes; event totals; every property with
  its node count — a quick way to see which tools will work.
- **Time axis** — for a dated tree: axis type, dated nodes, unit, root age or most
  recent date.

One window per tab; it re-reads the tree after every change, keeping unwritten edits.

**View → as phyloXML / as Newick / as Nexus** shows the tree as text — what *Save As*
would write — in one window with a format switcher. Markup is muted so names stand out:
syntax, lengths and support in grey, the document's own keywords (`#NEXUS`, `Begin
Taxa;`, `Tree`, `End;`) and attribute names (`NTax=`, `branch_length=`) in the accent
colour; a taxon *called* `End` stays plain. **Find** (⌘F) steps through hits with ↩ /
⇧↩; **Wrap lines**, **Copy**, **Save As…**. It regenerates after the tree changes.

**Hovering a node** shows a card — name, distance to parent, date, depth, support,
taxonomy, each sequence's accession and symbol, events, properties, and for an internal
node its tip count — the same card, in the same order, as Archaeopteryx.js. It is drawn
on the canvas, never a separate window, and follows the theme.

**Hovering a cell** of an annotation column shows the tip, the field and its value, and
for a heat map the scale the colour came from. A never-filled cell reads *not assessed*,
not 0; a stacked-bar or pie cell lists every series. Rectangular layouts and the
circular rings.

**Hovering a protein domain** names it, with its E-value, the residues it covers
(`185–445 (261 aa)`), the protein length, the tip and the accession if present. A
**left click** opens it at InterPro — the **Pfam entry** when it has a Pfam accession
(`PF00069`), else a **search for its name**, labelled as such. It is only a link; nothing
is fetched. It answers only for boxes really on screen — not for domains above the
E-value threshold, nor tracks **Auto-hide Labels** dropped. **Display Data → Rollover**
switches all three hovers off.

**Exporting does not disturb the figure on screen.** A radial or fixed-size export lays
the tree out for the page; until the window has redrawn — at once — hovering and
clicking answer nothing rather than answering about the page.

## Re-rooting

Re-root by clicking a node with **Click on Node to: Root/Reroot**, or with **Tools →
Midpoint-Root** or **Tools → MAD-Root** (**Analysis → GSDIR** chooses a root too). Each is
undoable and adds a sentence to the description.

**Trees that cannot be re-rooted** — the controls grey out, with the reason as tooltip:

- a tree its phyloXML marks `rerootable="false"` (a reconciled gene tree, say);
- a **time tree** — most internal nodes dated (a chronogram, a BEAST MCC tree, a
  Nextstrain tree): its branch lengths are times from its root. A tree with only its
  *tips* dated can be re-rooted — that is what a root-to-tip regression does.

**A warning when internal nodes carry data.** A name, taxonomy, events, a date or
properties on an internal node describe its clade, and re-rooting changes the clades
between the old and new root. Before such a re-root Archaeopteryx says how many —
*"This tree has data on 12 internal nodes. Re-rooting changes the clade of 4 of them, so
their data may no longer describe them."* — and you choose **Re-root** or **Cancel**.
Branch lengths, support and colours do not count.

**Unrooted trees.** When the file declares the tree unrooted (phyloXML `rooted="false"`,
Nexus `[&U]`) *and* it is shown **unrooted**, values that only mean something relative to
a root are left out: hovering an internal node lists the **tips around** it (`2 · 3 · 5`)
instead of distance, depth and tips below; a tip shows its branch length but no depth;
the node window omits an internal branch length; the Depth, Distance from Root and Clade
Size searches and the Depth / Height statistics are not offered. A plain Newick tree
declares nothing. Demos:
[`unrooted-node-data.xml`](https://github.com/cmzmasek/forester/blob/master/forester/demo/unrooted-node-data.xml),
[`not-rerootable.xml`](https://github.com/cmzmasek/forester/blob/master/forester/demo/not-rerootable.xml).
Archaeopteryx.js follows the same rules.

## Undo and redo

**Edit → Undo** (⌘Z / Ctrl+Z) and **Redo** step through the current tab's tree edits, 25
deep, each item naming its operation (*Undo Collapse Clade*). Undo snapshots the whole
tree before each change, so it covers everything alike: rerooting (midpoint and MAD
too), ladderizing and ordering, swapping and deleting nodes or subtrees, cut and paste,
node-data and tree-property edits, node styles and branch colours, collapsing, and every
data tool that writes into the tree — fetch, infer ancestor taxonomies, extract dates
from labels, import annotations, import GTDB taxonomy, load alignment, write clade taxa.
Reconciliation opens its results in a **new tab** instead.

- **Display settings are not edits** — checkboxes, layout, legend colours, shown fields.
  **Settings → Reset to Defaults** resets those.
- **Looking is never an edit** — only **Write to Tree** makes a step, one per write.
- **Open windows survive an undo**: a node or Tree Properties window re-reads its node
  from the restored tree, keeping unwritten edits. If the node is gone, the window says
  so and cannot write until an undo or redo brings the node back.

## Searching trees

Two **search boxes** (**A** and **B**) highlight matches: by A in **red**, by B in its
colour, by **both** in **teal**. B's colour (**Settings → Labels & Colors →
Found/Selected Colors**) is **Electric Violet** (default), **Neon Magenta** or **Emerald
Green**. With both filled, **Combine:** keeps them **independent** (default) or makes one
result, **A AND B** or **A OR B**, which drives the highlight, stepping, counter and
export.

Each box chooses a **Field** and a **Match**:

- **Field** — only fields this tree has, named as in **Display Data**. **Any Text** (the
  default) searches every text field and your properties; or pick **Node Name**, a
  taxonomy field (**Scientific**, **Common**, **Code**, **Identifier**, **Synonym**,
  **Lineage**), a sequence field (**Seq Name**, **Gene Name**, **Gene Symbol**, **Seq
  Accession**), **Annotation**, **Domain**, or a custom property (`data:host`). Numeric
  fields — **Branch Length**, **Support / Confidence**, numeric properties — and
  **structure** fields (`Structure:` **Clade Size (tips)**, **Number of Children**,
  **Depth from Root (edges)**, **Distance from Root**, **Node Type**) find, say, every
  clade over 50 tips or every unresolved node.
- **Match** — for text **contains** (default), **starts with**, **ends with**, **whole
  word**, **regular expression**; for numbers **equals**, **not equal**, **less than**,
  **at most**, **greater than**, **at least**, **range**.

For a specific text field the box **suggests the values the tree has**, filtered as you
type the way the search will match (prefix, suffix or anywhere; with **Match Case**),
the typed part in bold, ten at most. Only the current term is completed (`kinase, pho`
completes `pho`). **↓ / ↑** move, **Enter** picks and searches, **Esc** closes.
Archaeopteryx.js suggests the same way.

**Match Case** and **Inverse** (the non-matches) apply to both boxes. In a text query `,`
is **OR** and `+` is **AND** (`kinase, phosphatase`; `human + receptor`), literal in a
regular expression. Choices are remembered as you work.

Step through hits with **◀ / ▶** or **View → Find Next / Previous** (**⌘G / ⌘⇧G**), each
centred in view. **Bold Found Labels**, **Dim Non-Matches** and **Pulse Found Nodes**
(**Settings → Labels & Colors**) make them stand out. A **Found / Selected: N** counter at
the right of the menu bar totals the highlighted nodes (hits and manual selection are
one); hover it for the A / B / Selected breakdown.

---

## Time trees & chronograms

However the dates arrive — **BEAST / BEAST X** output, an **Auspice / Nextstrain** JSON,
**extracted from the tip labels**, or a phyloXML `<date>` — they land in one date model
that every time-tree feature reads, so the toolkit works the same on any dated tree:

- A dated tree is **auto-detected**, marked with a **"Time tree"** badge, and given the
  **right axis per tree from its own dates' unit**: **Geologic (ICS)** for millions of
  years, **Calendar-year** for a tip-dated epidemiology tree. A dinosaur tree and a
  SARS-CoV-2 tree in two tabs each show their own axis; there is no global switch.
- **Node-age (HPD) bars / spindles** draw divergence-time uncertainty, **fossil-range
  (FAD/LAD) bars** a fossil tip's stratigraphic duration, and **Color by → date** shades
  tips by sampling date — each turning on by itself when the data is there.

A time axis needs branch lengths that mean time, so a dated tree opens as a
**phylogram**; as a **cladogram** the axis steps aside. The axis **replaces** the plain
distance scale.

### Time | Div: the time or the divergence layout

A tree that says, for every branch, both *how long* it lasted and *how much* changed
along it gets a **Time | Div** toggle (left panel, under the P/A/C layout buttons).
**Time** lays it out in time, with its time axis; **Div** in substitutions per site.
The divergence is either **recorded** on every node (Nextstrain's `div`) or
**derived** from a clock **`rate`** on every branch (BEAST) as rate × the branch's
length in time — the tooltip says which. It is a display mode: nothing is edited, and
switching back is exact.

- **Time is the tree you opened** — the branch lengths the file states, given back to
  the digit, not recomputed from the node dates. In a summary tree the two need not
  agree (demo: `beast-lengths-not-heights.nex`, a branch of 1.407 between nodes dated
  1.2 apart).
- **Offered only when both layouts can state every branch:** a date on every node,
  the root included; a length on every branch; a divergence for every branch. A rate
  or a recorded divergence is a plain decimal number (`0.0026`, `2.6e-3`); a rate may
  not be negative, a recorded divergence may. Zero is a value — only a missing number
  is missing. Nor is a picture of nothing offered: divergence 0 on every branch, or
  every tip on the root's date. A constant rate *is* offered. Each of
  `beast-rate-missing.nex`, `beast-date-missing.nex`, `beast-length-missing.nex`,
  `beast-rate-spelling.nex`, `beast-rates-zero.nex`, `nextstrain-div-missing.nex` and
  `nextstrain-div-spelling.nex` is an offered tree with exactly one thing changed.
- **One tree, one layout:** the toggle acts on the whole tree, also from a subtree view.
- **A branch that runs backwards** (a node dated before its parent — summary trees and
  real Nextstrain builds both have them): **Time** keeps the negative length the file
  states, **Div** counts it as 0, and it is drawn at 0 (demo: `beast-negative-span.nex`).
- **Edits keep the two layouts in step** — delete, cut, prune, undo, redo, and a new tab
  made from the tree. A deleted node gives its branch to its child: in **Time** the sum
  of the two lengths, a negative one included; in **Div** each part at its own node's
  rate, a part that runs backwards adding nothing.
- **Saving:** saved as phyloXML from **Div**, a tree reopens in **Div** and takes its
  time from its dates. Saved as Newick or Nexus from **Div**, it keeps neither dates
  nor rates and opens as a plain tree in divergence lengths.


## BEAST and BEAST X output

Archaeopteryx reads annotated trees from **BEAST**, **BEAST 2** and **BEAST X**, including
**TreeAnnotator** MCC summaries — annotated **Nexus** (`.tree` / `.trees`) and annotated
**Newick / NHX** with FigTree-style `[&key=value, ...]` blocks. Just open the file
(**Settings → Files → "Read [&...] Annotations (BEAST, MrBayes, FigTree, TreeTime,
Nextstrain)"**, on by default; off, an annotation stays a plain comment). `aptx_render`
reads a file exactly as the window does.

| BEAST annotation | Becomes | Turn it on with |
| --- | --- | --- |
| `posterior` | Branch support (confidence) | **Confidence Values**; support coloring / symbols |
| node age `height` / `height_median` / `height_mean` + `height_95%_HPD={lo,hi}` (or `height_range`) | Node age with a 95% HPD interval | **Node Age Bars (HPD)** — on by itself for a dated tree with intervals |
| discrete / geographic traits (e.g. `location`) with posterior state sets | **Ancestral-state pie charts** | the **"Ancestral pie:"** dropdown (appears when the tree has such a trait) |
| FigTree's `!color` — `#RRGGBB` or, as FigTree writes it, a signed integer (`!color=#-8381639`) | In the tree: the **branch color**. In a Nexus `taxlabels` block (`'NewYork_454'[&!color=#-8381639]`): the tip's **label color** | **Use Visual Styles** |
| any other field (`rate`, `length_*`, custom traits, …) | A node property `beast:<key>` | **Color by**, **Size by**, **Annotation Fields** |

Nothing is discarded; a malformed field is skipped rather than refusing the file. An
annotation may start with anything (`!color` first still has its posterior, ages and
rates read), and a node may carry several `[&...]` groups. (`beast:` means "came from a
bracket annotation", whoever wrote it.) A dated MCC tree opens as a **phylogram** with
**Node Age Bars (HPD)** on.

**Heights become calendar dates when the tip labels say so.** BEAST writes node ages as
**heights** with **no unit**. A tip-dated analysis nearly always has the **sampling date
in the tip name** (`A_duck_Guangdong_12_2000`, `EBOV|KR817226|2014-06-10`): if heights are
years, every tip's label date plus its height is the same date — the youngest tip's.
Where the labels agree, the heights are **converted to calendar dates**, the tree opens
on the **Calendar axis** with its HPD intervals in years, and the description records
the conversion and the date of height 0. At least 19 in 20 of the tips with a height and
a dated label must agree, sampled at **different times** (tips from one year fit any
unit). Otherwise the tree keeps plain heights and gets no axis — choose one under
**Settings → Overlays → Time Axis**, and a saved tree keeps it. Dates that already carry a
unit are never overridden.

Samples dated only to a month or year keep their interval: on calendar time a
**sampling-date uncertainty**, drawn by the Node Age Bars, never a fossil range. When
**every branch** carries a **`rate`**, the tree also gets the
[**Time | Div**](#time--div-the-time-or-the-divergence-layout) toggle.

The node-age overlay has two shapes (**Settings → Overlays → Data Overlays → Node age
shape**): a **Bar** across the 95% HPD interval (the FigTree convention), or a
**Spindle** peaking at the point estimate and narrowing to the bounds — a schematic of
the summarized uncertainty, not the raw posterior density, which a summary tree does
not carry.

## MrBayes, TreeTime and Nextstrain (Nexus)

**MrBayes**, **TreeTime** and **Nextstrain / Auspice** write the same `[&key=value, ...]`
annotations, and each is read for what it is. Only `.nex`, `.nexus`, `.nx` and `.nxs` are
Nexus by name; for anything else (`.tre`, `.trees`, `.con.tre`, `.t`, …) the first line
decides.

**MrBayes consensus trees** (`sumt`, `.con.tre`) carry two annotation groups per node
with the branch length between them. `prob` (with `prob_stddev`) is the **posterior
probability**; the **branch length is the one the file states**; `length_mean`,
`length_median`, `length_95%HPD`, `prob_range` and the rest are node data to color by,
search and inspect.

**TreeTime** writes the *same* `date=2003.84` on every node of both `timetree.nexus`
(years) and `divergence_tree.nexus` (substitutions per site). A `date=` becomes a
calendar date only where the parent-to-child date differences reproduce the branch
lengths — so the time tree opens **on the Calendar axis** and the divergence tree does not
(its dates stay descriptions, and it can be re-rooted). `mutations="A54G,T92C"` is read
whole, and TreeTime's annotations are filed as `treetime:<key>`. TreeTime's **Newick**
glues an internal name to its confidence — `NODE_00000161.00` is `NODE_0000016` with
confidence `1.00` — and they are separated on open. Its `auspice_tree.json` (the only
output with full-precision dates) opens as an [Auspice JSON](#auspice--nextstrain-json).

**Nextstrain "download Nexus"** opens like the build's JSON: `num_date` is each node's
calendar date (**Calendar axis**, time tree), `num_date_CI` its interval (**Node Age
Bars**), and `div` the `nextstrain:div` behind the
[**Time | Div**](#time--div-the-time-or-the-divergence-layout) toggle. Values are read as
written (`country=Côte d'Ivoire`). On the **divergence** export (`…_tree.nexus`), whose
branch lengths are substitutions, the dates do not match the lengths, so they stay plain
data (`nextstrain:num_date`, `nextstrain:num_date_CI`) and the tree is not a time tree.

Demos: `nextstrain-nexus.nex`, `treetime-nexus.nex` + `treetime-divergence.nex`,
`treetime-tree.nwk`, `mrbayes-consensus.con.tre`, and a TreeAnnotator tree dated from its
tip labels, `beast-tip-dates.nex`, in the
[demo folder](https://github.com/cmzmasek/forester/tree/master/forester/demo).

## Dates in tip labels

Most epidemiology trees (BEAST, TreeTime, augur, GISAID Newick) carry the **sampling date
in the tip name** — `hCoV-19/USA/CA-1234/2021|2021-03-15`, `A/Texas/50/2012`. Opening such
a tree **offers to extract the dates** (not asked when the tips already carry dates —
BEAST heights count, a height of 0 included); **Tools → Extract Dates from Labels…** runs
it any time. It reads ISO (`2021-03-15`), numeric (`15/03/2021`), month-name
(`01-Dec-2015`), decimal-year (`2021.37`) and bare-year (`…/2012`) dates, shows a
**preview** of every tip before writing, and on Apply sets each tip's `<date>` and a
numeric `data:date`: the tree moves onto the **Calendar axis** and gains a **Color by →
data:date** gradient. An incomplete date maps to the middle of its interval (`2021` →
mid-2021); an ambiguous one (`05/03`) is read **day-first** by default, switchable in the
preview. Undoable.

## Newick time trees (no dates in the file)

TreeTime and Nextstrain also export time trees as plain **Newick** — branch lengths in
**years**, nothing saying so, in the same shape as their divergence trees. As for
[BEAST heights](#beast-and-beast-x-output), the tip labels settle it: if lengths are
years, each tip's date **minus its distance from the root** is the same date, the root's.
Where the labels agree, **every node is dated** on open (root date + distance), the tree
opens on the **Calendar axis**, the description records the root date — and, being a time
tree, it **refuses re-rooting**. A divergence tree cannot pass (its tips sit ~0.001 from
the root while its labels span years). The same guards apply: 19 in 20 agreeing, samples
at different times, a tree with its own dates never touched. Demo pair:
`newick-time-tree.nwk` and `newick-divergence-tree.nwk` — same topology and labels, only
the first becomes a time tree.

## Geologic time axis

For a tree dated in millions of years, **Settings → Overlays → Time Axis → Geologic (ICS)**
draws the **international geologic time scale** beneath a **phylogram** as two coloured,
named bands — **System/Period** over **Series/Epoch** — so clades read directly against
the Cretaceous, Jurassic, Triassic, ….

The axis is **per tree**, derived from the unit of the tree's own `<date>` values; dates
with **no unit** get no time axis — the size of the numbers is never taken as a unit (the
one exception is a BEAST tree whose tip labels date its heights; see
[BEAST and BEAST X output](#beast-and-beast-x-output)). The Settings dropdown overrides
the axis for the current tab, or turns it off, and a saved tree keeps a deliberate
choice.

The axis runs along the bottom (root left), down the breadth side (root top / bottom),
and as concentric period **rings** in **circular** — a geologic disc. Not in unrooted. In
rectangular layouts it stays pinned in view as you zoom and scroll. A **numeric age
axis** in Ma sits beneath the bands (in circular the annuli are the scale).

The bands **adapt to the span of the tree**, always covering it with some detail:

| The tree spans | Bands |
| --- | --- |
| one or two **Series** (e.g. an all-extinct Late Cretaceous clade) | **Series/Epoch** over **Stage/Age** |
| the Phanerozoic | **System/Period** over **Series/Epoch** |
| into the Proterozoic | **Erathem/Era** over **System/Period** |
| into the Archean | **Eonothem/Eon** over **Erathem/Era** |

So a tree of life is fully banded, and a Late Cretaceous tree is banded *Late Cretaceous*
over *Cenomanian … Maastrichtian* (stages exist for the Phanerozoic only). Demo:
**late-cretaceous-stages.xml** (*Late Cretaceous Dinosaurs*).

The tree is anchored by its **root age** — the oldest `<date>`, or **"Set root age…"** —
and each Ma spans one branch-length unit, so a **fossil-only** clade works: an all-extinct
ammonite tree ends at 66 Ma rather than being stretched to the present.

**Settings → Overlays** (with the geologic axis on; off by default, saved per tree):
**Time-Axis Grid Lines** draws faint lines at the finer band's boundaries; **Geologic
Boundary Ages** labels the coarser band's boundaries (*201.4* at the base of the Jurassic).

Names, boundaries and colours are those of the **International Chronostratigraphic Chart**
of the **International Commission on Stratigraphy (ICS / IUGS)**
([stratigraphy.org](https://stratigraphy.org)):

- Cohen, K.M., Harper, D.A.T., Gibbard, P.L. & Car, N. (2025, updated): "The ICS
  International Chronostratigraphic Chart this decade", *Episodes* 48:105–115.

## Fossil range bars (FAD/LAD)

A fossil taxon is known from a *stratigraphic range*, from its **First Appearance Datum**
(FAD) to its **Last Appearance Datum** (LAD). **Settings → Overlays → Data Overlays →
Fossil Range Bars (FAD/LAD)** draws, on a dated phylogram, a capped bar over the terminal
branch of each tip whose `<date>` has a **min/max** — read against the
[geologic axis](#geologic-time-axis), a stratigraphic-range figure without hand-drawing.
It turns on by itself when the tree has fossil tip ranges, and is drawn in every
rectangular orientation and as radial segments in circular. The range is the tip's
phyloXML `<date>` (value / min / max), so it works on a tree time-scaled by any tool.

A range needs a **width**: `{0,0}` is none, and so is an interval whose ends differ only in
the last digits of floating-point arithmetic (TreeAnnotator writes an exact date as
`{9.0, 9.000000000000004}`); such a range draws, switches on and reports nothing. On a
tree dated in **calendar years** a tip's interval is not a fossil range but the uncertainty
of a sampling date ("sometime in 2015"): the **Node Age Bars** draw it, the fossil bars
never do.

- Bell, M.A. & Lloyd, G.T. (2015): "strap: an R package for plotting phylogenies
  against stratigraphy and assessing their stratigraphic congruence",
  *Palaeontology* 58(2):379–389.

## Calendar (absolute-date) axis

For a **tip-dated** tree (e.g. a SARS-CoV-2 phylodynamic tree), **Settings → Overlays →
Time Axis → Calendar (dates)** draws a labelled year / decade ruler: along the bottom
(root left), down the side (root top / bottom), or as labelled **year rings** in
circular. It is anchored at the **most recent tip** — the largest tip `<date>`, or **"Set
most-recent-tip date…"** — and each node's date is its distance from the root back from
there. **Time-Axis Grid Lines** draws a faint line at each labelled year.

## Auspice / Nextstrain JSON

Archaeopteryx reads **Auspice / Nextstrain v2** datasets — the `dataset.json` behind
[nextstrain.org](https://nextstrain.org), and TreeTime's `auspice_tree.json` (which states no
version). Open the `.json` with **File → Read Tree from File…**, or try **File → Demo Trees
→ Phylodynamics (Nextstrain JSON)**. It maps onto features the viewer already has:

- **`num_date`** puts the tree on the **Calendar axis**, and each node's
  **`num_date.confidence`** becomes a **Node Age bar / spindle** — on an internal node
  the divergence-time interval, on a **tip** the sampling-date uncertainty of a sample
  dated only to the month or year (an exact date draws nothing; never a fossil range).
  It is also the numeric **`nextstrain:num_date`**, to **Color by** date.
- **`div`** (cumulative divergence) drives the
  [**Time | Div**](#time--div-the-time-or-the-divergence-layout) toggle: the **time**
  layout (`num_date`, with the calendar axis) or the **divergence** layout (`div`, in
  substitutions/site).
- each **discrete trait** — `country`, `region`, `clade_membership`, `host`, … — becomes a
  **`nextstrain:<trait>`** property to color by, tabulate or search, and each trait's
  per-node **confidence** drives the **Ancestral-State Pies**.

The map, entropy and frequencies panels are not imported.

- Hadfield, J. *et al.* (2018): "Nextstrain: real-time tracking of pathogen
  evolution", *Bioinformatics* 34(23):4121–4123.

---

## Annotating clades by rank

**Tools → Annotate Clades by Rank…** draws nested rank brackets — genus inside family
inside order — from the tree's own taxonomy:

| Mode | What you get |
| --- | --- |
| **Shaded boxes** | a translucent wash behind each clade, in the clade's colour |
| **Bars + labels** | a solid colour bar per clade past the tip labels, with the taxon name |
| **Brackets `]` + labels** | the same, as a black-and-white bracket (no colour key) |

To **remove the marks**, pick *(none) — stop drawing the clade marks* at the top of the
rank list (offered only when there are marks, and never preselected, so OK cannot wipe
them by accident). It removes the drawing only: clade taxa you wrote into the tree stay
(undo them with **⌘Z**). **Reset to Defaults** clears the marks too.

### More than one rank at once

**Bars** and **Brackets** show **up to three ranks** as nested columns (the main rank plus
two under *Additional nested levels*, in any order): the finest is always drawn nearest
the tips, the broadest outermost. Colours are **hue-banded** — each broadest-rank clade
owns a slice of the colour wheel and the clades inside are shades of it (in the animal
demo *Mammalia*, *Aves* and *Amphibia* are greens inside *Chordata*). A single rank gets
the plain distinct palette. Containment is read from the **tree**, not the names; a clade
whose broader rank could not be resolved is drawn **desaturated** rather than implying a
parent.

- **Label angle is per level** (*Vertical / Diagonal / Horizontal*). Vertical is compact
  for a few large clades; use **Horizontal** for many one- or two-tip clades, whose
  vertical labels would overprint.
- **Skip single-member clades** (on by default) drops bars for one-tip taxa; turn it off
  for a deep backbone like the animal demo, where one representative per class is normal.

The legend has a titled block per rank; clicking a row recolours that taxon at its rank.

### Where it works

In the three **rectangular** orientations and in **circular** (as rings); **not in
unrooted**, whose tips sit at different radii and a clade need not occupy one sector.
Boxes stay **single-level**: nested translucent washes would multiply into a colour no
legend row claims. It works **offline** when the tree carries the ranks (as the demos do);
otherwise it offers to resolve tips online through NCBI and UniProt. *Also write the
clade taxa into the tree* makes the annotation real, saveable internal-node taxonomies
(undoable). Demos: **Animal Tree of Life (Nested Clade Levels)** and **Bat Phylogeny
(Taxonomy by Rank)**.

## GTDB taxonomy

For bacterial and archaeal genomes, **File → Import GTDB Taxonomy…** reads a
**GTDB-Tk**-style table — a tip-name column and a classification like
`d__Bacteria;p__Pseudomonadota;…;g__Escherichia;s__Escherichia coli`, as `classify`
writes it — onto a tree whose tips are genome accessions (demo: **GTDB Taxonomy
(Genome-based)**). Each of the seven ranks becomes a categorical **`gtdb:<rank>`**
property, plus a `<taxonomy>` at the most specific rank: **Color by** `gtdb:phylum`, add a
column for `gtdb:family`, search `gtdb:genus`. Entirely offline — reproducible, pinned to
the GTDB release that made your table. Undoable.

- Parks, D.H., Chuvochina, M., Rinke, C., Mussig, A.J., Chaumeil, P.-A., Hugenholtz, P.
  (2022): "GTDB: an ongoing census of bacterial and archaeal diversity through a
  phylogenetically consistent, rank normalized and complete genome-based taxonomy",
  *Nucleic Acids Research* 50(D1):D785–D794.
- Chaumeil, P.-A., Mussig, A.J., Hugenholtz, P., Parks, D.H. (2020): "GTDB-Tk: a
  toolkit to classify genomes with the Genome Taxonomy Database", *Bioinformatics*
  36(6):1925–1927.

## Tip images

**Settings → Overlays → Tip Images** draws a picture at each branch end — a photo, a
silhouette, a specimen — at the height set by the slider beside it (aspect kept). A tip's
image comes from a **local file** (relative to the tree's folder, or absolute) or an
**http(s) URL** (fetched once, in the background, and cached in
`~/.archaeopteryx/image-cache`, so it works offline afterwards). The reference is read
from a node property (`image`, `img`, `photo`, `silhouette`, `picture`, `thumbnail`,
`tip_image`, `image_url`), a taxonomy `<uri>` of type `image_url`, or any property ending
in `.png`, `.jpg`, `.jpeg`, `.gif` or `.bmp` (not SVG yet). The easy way: an image column in
a table loaded with **File → Import Annotations**. A tree with image references turns Tip
Images on by itself.

Tip images render in **all five layouts** (upright on the spoke in the radial ones) and
in every export. One that cannot load — a missing file, or a URL to a web *page* — draws a
faint broken-image marker.

> **Wikimedia Commons:** an article URL (`…/wiki/File:…`) is a page, not an image; use
> `https://commons.wikimedia.org/wiki/Special:FilePath/<FileName>`.

## Sequence alignment

**File → Load Alignment (FASTA)…** draws a multiple sequence alignment **beside the tree**:
each row joins the tip of the same name as a track of coloured residue cells. Saved as
phyloXML, the tree **embeds the alignment** (on each tip's molecular sequence), and a tree
that carries aligned sequences shows them when opened. A **Nexus** file with a tree and a
`CHARACTERS` or `DATA` matrix (MrBayes, PAUP\*, …) opens with its alignment directly —
interleaved matrices, `MATCHCHAR` (`.`), `[ ]` comments, and names differing only in case or
`_` versus space are handled.

- **Colouring** follows the residue: Zappo-style for amino acids, A/C/G/T-U for
  nucleotides (auto-detected). Wide enough columns show the **letter**; a run of gaps is
  one faint line.
- A **scrollbar** below pans the columns while the tree stays put; **faint lines** mark
  the alignment's real ends, and a **column ruler** shows 1-based column numbers.

### Hovering a residue

| line | what it means |
| --- | --- |
| **Alignment column** | the 1-based column of the *alignment* — what the ruler shows |
| **Residue *n* of this sequence** | the residue's own number in the ungapped sequence |
| **`L` – Leucine** | the letter and its name (for DNA/RNA the base: `G` – Guanine) |
| *aliphatic (hydrophobic)* | the physico-chemical class — the grouping the cell is coloured by |
| **Hydropathy (Kyte–Doolittle)** | +4.5 (isoleucine) to −4.5 (arginine) |

The column moves if you realign; the **residue number** maps back onto the real protein or
gene — for a structure, a mutation list, a paper — without counting gaps. A gap says only
that. Nucleotides get the base and purine/pyrimidine but no hydropathy (the scale is for
amino acids), nor do `B`, `X` and `Z`. Kyte & Doolittle (1982) is cited under **Help →
References**.

The **Sequence Alignment** checkbox (*Display Data*; shown only for a tree with aligned
sequences, ticked on opening) is **per tab**; **Settings → Overlays** has it too, with the
**Alignment column width** (remembered). Loading is undoable, and the track renders in
every export. It is drawn in the rectangular **root-left** layout. Demo: **Alignment next
to Tree**.

### Conservation and consensus

A band under the alignment shows each column's conservation, with its **consensus
residue** (most common non-gap residue) beneath once columns are wide enough; off under
**Settings → Overlays → Conservation track**. It is scored over the tips **currently
displayed** — entering a sub-tree or collapsing a clade re-scores it — so it labels itself
with its measure and count (`Consensus identity (n = 6)`), in exports too.

**Settings → Overlays → Conservation measure**:

- **Consensus identity** — the fraction of rows with the consensus residue; easiest to
  state in a caption.
- **Information content** — the sequence-logo measure, `(log2(K) − H) / log2(K)` (*H* the
  Shannon entropy of the residues present, *K* 4 or 20), scaled by the non-gap fraction.
  It ranks a column split two ways above one split four ways, where identity ties them.
- **Sequence logo** — the information content as a stack of letters, each letter's height
  its share, the most frequent on top, coloured like the cells. A conserved but half-gapped
  column is drawn at *full* conservation and *half* height; an all-gap column draws nothing;
  letters under half a pixel are left out. The consensus row is dropped — the top letter is
  the consensus.

The first two run from 0 to 1 and count gaps against a column (half gaps caps it at 0.5).
Upper and lower case are the same residue; a short row counts as gapped past its end; a
gappy column still names its consensus; ties go alphabetically; ambiguity codes (N, X, B,
Z…) are ordinary residues, so a column of them scores *low*. This is **not** the
amino-acid property score Jalview calls "Conservation" (Livingstone & Barton 1993); these
measures work on nucleotides too. Definitions and citations: **Help → References**.

## Broken (truncated) long branches

**Settings → Layout → Break Long Branches** draws a branch far longer than the rest — a
distant outgroup, a fast lineage — **shortened, with a break glyph** (`─//─`), and
re-derives the depth scale so the rest of the tree reclaims the width. Display only: the
branch length is unchanged and still shown as its label. A branch is *long* above **8×
the median** positive branch length (robust to the outlier itself and to zero-length
polytomy branches), so a near-clock tree shows no breaks. It works in every phylogram
layout — rectangular (unaligned and aligned; in aligned the tip still meets the label
column), circular and unrooted (the spoke is shortened, the glyph rides it). In the
rectangular and circular layouts the **scale bar** stays, sized to the unbroken scale,
while the **axis** and **grid lines** are hidden — no linear ruler can span a truncated
branch. Demo: **Break Long Branches**.

## Tanglegrams

A **tanglegram** links two trees' matching tips to show how congruent they are (gene vs.
species tree, host vs. parasite, two methods). With two trees open, **Analysis → Create
Tanglegram…**, choose them and the field to link on (node name, taxonomy or sequence); it
opens in its own window, the second tree mirrored, reporting the **crossing** connectors
and a normalised **entanglement** score.

**Click a clade's vertical bar** to flip it (a topology-preserving rotation), or
**Auto-untangle** both trees; both are undoable. **Colour** recolours the connectors:
uniform, **Crossings** (in red), or by a tip attribute with a legend. **Export…** writes
**PDF**, **SVG**, **EPS** or **PNG**.

**Auto-untangle** is a barycentre heuristic: each clade's children are ordered by the mean
position of the tips they link to in the other tree, alternating between trees until
stable, with random restarts, keeping the fewest crossings (it never adds any). Minimising
crossings is NP-hard.

- Sugiyama K, Tagawa S, Toda M (1981): "Methods for visual understanding of hierarchical
  system structures", *IEEE Transactions on Systems, Man, and Cybernetics* 11(2):109–125.
- Scornavacca C, Zickmann F, Huson DH (2011): "Tanglegrams for rooted phylogenetic trees
  and networks", *Bioinformatics* 27(13):i248–i256.

**Entanglement** is in [0, 1]: **0** when the leaf orders agree, **1** when fully reversed.
Two connectors cross exactly when their tips are in opposite order in the two trees, so
crossings are discordant pairs — the Kendall-τ distance — divided by *n*(*n*−1)/2 to compare
trees of different sizes (computed in *O*(*n* log *n*)). It is simpler than the
leaf-position-based *entanglement* of dendextend.

- Kendall MG (1938): "A New Measure of Rank Correlation", *Biometrika* 30(1–2):81–93.
- Galili T (2015): "dendextend: an R package for visualizing, adjusting and comparing
  trees of hierarchical clustering", *Bioinformatics* 31(22):3718–3720.

---

## Rendering figures from the command line

> **You can skip this section.** Everything Archaeopteryx does, it does in the window.
> This is for rendering many trees at once, or regenerating a figure whenever its data
> changes.

`aptx_render` draws a tree to a file without opening Archaeopteryx, with the **same
renderer and exporters** as the window.

### When it is worth it

- **A folder of trees** — one figure each, identical settings.
- **A figure that keeps changing** — re-run the command after the alignment is rebuilt.
- **A reproducible methods section** — the command records how the figure was made and
  gives the same figure on anyone's machine.

### Running it

`aptx_render` is in `forester.jar` (see [Run from the jar](#run-from-the-jar)):

```
java -cp forester.jar org.forester.application.aptx_render  tree.xml  figure.pdf
alias aptx_render='java -cp /path/to/forester.jar org.forester.application.aptx_render'
```

With no options it writes a 180 × 130 mm figure at 300 dpi, drawn as Archaeopteryx would
draw the tree on opening it. `-help` lists the options.

### The output format is the file extension

| you write | you get |
| --- | --- |
| `figure.pdf` | vector PDF — the usual choice for a manuscript |
| `figure.svg` | vector SVG — for editing in Illustrator or Inkscape |
| `figure.eps` | vector EPS — where a journal still asks for it |
| `figure.png` | raster PNG, with the true DPI recorded in the file |
| `figure.jpg` | raster JPEG (lossy; PNG is better for line art) |
| `figure.tiff` | raster TIFF |

Vector text is drawn as outlines, so no fonts are needed to view it. An unknown extension
is refused.

### Options

| option | what it does |
| --- | --- |
| `-size=<W>x<H><unit>` | figure size — `170x120mm`, `8x6in`, `1200x900px`. Default `180x130mm` |
| `-dpi=<n>` | dots per inch, default `300` |
| `-style=<s>` | `rectangular` (default), `circular` or `unrooted` |
| `-labels=<d>` | in `circular` and `unrooted`: `radial` (default — names ride the spoke) or `horizontal`. Pin it in a script whose figure must not change between versions |
| `-phylogram` | draw branch lengths to scale |
| `-cladogram` | ignore branch lengths |
| `-support` | show confidence / support values |
| `-bl` | show branch-length values |
| `-color=<ref>` | colour tips by a property, e.g. `data:host` |
| `-help` | the option list |

Without `-phylogram` or `-cladogram`, a tree with branch lengths is drawn as a phylogram.
`-color` takes a property reference (`data:host`, `beast:rate`, `beast:region`, …); one the
tree lacks stops the render with the list of those it has. In a rectangular figure the
legend always gets its own column at the right.

### Size and DPI — worth two minutes

**The figure is laid out at its physical size, not its pixel count**: a 12-point label
takes the same share of a 170 mm page at 150 or 600 dpi.

> **Prefer a physical size (`mm` or `in`).** `-size=1000x1000px -dpi=300` is a page 3.3
> inches across on which the default font is enormous — labels collide and, in circular
> layouts, are truncated. The same tree at `-size=250x250mm` renders cleanly.

With **`mm` / `in`** the size sets the page and `-dpi` the pixels a raster export puts on
it (vector output ignores `-dpi`); with **`px`** the pixels are set and `-dpi` decides the
page size. Journal columns are a good start: `-size=85x110mm` single, `-size=170x120mm`
double. If the tree is too dense for the size, labels are hidden rather than overlapped,
and `aptx_render` says so:

```
Warning: some labels were hidden by "Auto-hide Labels" to avoid overlap at this size.
```

A larger `-size` is the fix.

### Examples

```
# a double-column PDF with support values
aptx_render -size=170x120mm -support  tree.xml  figure.pdf

# a circular figure, coloured by a metadata property, as editable SVG
aptx_render -style=circular -size=250x250mm -color=data:host  tree.xml  figure.svg

# a single-column PNG at 600 dpi
aptx_render -size=85x110mm -dpi=600  tree.xml  figure.png

# every tree in a folder
for t in trees/*.xml; do
    aptx_render -size=170x120mm "$t" "figures/$(basename "${t%.xml}").pdf"
done
```

### The same command gives the same figure

The render starts from documented defaults plus your options — **never** from what you
last set in the window, which would make one command draw different figures on different
machines. Your saved settings are neither read nor changed.

### It needs a display, even though no window appears

Drawing a tree builds the control panel off-screen. On a desktop that just works; on a
headless server (a cluster node, a CI runner) use a virtual display:

```
xvfb-run -a java -cp forester.jar org.forester.application.aptx_render \
         tree.xml figure.pdf
```

(`xvfb-run` is in the `xvfb` package on Debian / Ubuntu.)

### What it does not do yet

It renders from the options above only. The figure you composed in the window — annotation
columns, clade bands, time axes, tip images and the rest — is saved with a phyloXML tree,
but `aptx_render` does not apply it yet. If that, or any setting, is what you want from a
script, say so on the [issue tracker](https://github.com/cmzmasek/forester/issues).

---

## When something goes wrong

An installed Archaeopteryx has no console, so unexpected errors are written to
`~/.archaeopteryx/archaeopteryx.log` — open it with **Help → Show Error Log**. When
something has failed in this session a quiet **⚠ error logged** marker appears in the menu
bar (click to open the log, or dismiss it); there is no pop-up, since a failure while
drawing repeats on every redraw. **If you hit a bug, attach that file to your report** —
its stack trace is what makes the problem findable. A repeating failure is written once
and counted ("… the same failure repeated 412 more times"), and the file restarts past a
couple of megabytes.

## This repository

This is the **home** of Archaeopteryx: the website (`docs/`, on GitHub Pages), this
documentation, the citation metadata and the releases. The **source code** is in
[`forester`](https://github.com/cmzmasek/forester), the Java library and command-line
toolkit of which Archaeopteryx is the interactive front end; the release workflow here
builds the installers from a pinned `forester` tag, so Archaeopteryx has its own
versioned, citable identity. The **online version**,
[Archaeopteryx.js](https://cmzmasek.github.io/archaeopteryx-js/), is a separate
implementation ([`archaeopteryx-js`](https://github.com/cmzmasek/archaeopteryx-js)) kept in
step on what a shared tree depends on (formats, "Color by", node data).

## Citing

A dedicated publication is in preparation. Until then, use the **Cite this repository**
button (backed by [`CITATION.cff`](CITATION.cff)); once a release is archived on Zenodo,
its DOI provides a stable, versioned citation.

## License

GPL-3.0 — free and open source. © Christian M. Zmasek.
