# Archaeopteryx

**The desktop viewer for phylogenetic figures worth publishing.**

Archaeopteryx is a mature, offline application for visualizing, annotating, and
analyzing phylogenetic trees — built for publication-quality figures. It brings
together integrated taxonomy and sequence annotation, protein-domain
architectures, calendar and geologic time axes, and WYSIWYG vector (PDF / SVG /
EPS) export.

**→ [cmzmasek.github.io/archaeopteryx](https://cmzmasek.github.io/archaeopteryx)** ·
**[Download the latest release](https://github.com/cmzmasek/Archaeopteryx/releases/latest)**

Self-contained installers (macOS `.dmg`, Windows `.msi`, Linux `.deb`) bundle a
runtime — there is no Java to install.

---

## Highlights

- **Integrated annotation** — pull UniProt / NCBI taxonomy, sequence data, and
  protein-domain architectures straight onto the tree.
- **Calendar & deep time** — tip-dated calendar axes for molecular epidemiology,
  and a full ICS geologic time scale for the fossil record.
- **Publication vector export** — WYSIWYG PDF / SVG / EPS from the same renderer
  that draws the screen; outline text, figure-grade by construction.
- **Undo & provenance** — every edit is undoable, and every tree-changing
  operation records what it did.
- **Large trees, five layouts** — rectangular (three orientations), circular,
  and unrooted, with tip-aligned annotation columns that ride into circular rings.
- **Reads what you have** — Newick, NHX, Nexus, phyloXML, plus Nextstrain /
  Auspice JSON, tip-dated labels, and BEAST intervals.

See **`File → Demo Trees`** in the app for pre-configured examples of each feature.

## This repository

This is the **home** of Archaeopteryx: the website (`docs/`, served by GitHub
Pages), the citation metadata, and the release stream.

The **source code** lives in the [`forester`](https://github.com/cmzmasek/forester)
project — an open-source Java library and command-line toolkit for phylogenetics,
of which Archaeopteryx is the interactive front end. The release workflow here
builds the installers from a pinned `forester` tag and publishes them to this
repository's Releases, so Archaeopteryx has its own versioned, citable identity.

## Citing

A dedicated publication is in preparation. Until then, use the **Cite this
repository** button (backed by [`CITATION.cff`](CITATION.cff)); once a release is
archived on Zenodo, its DOI provides a stable, versioned citation.

## License

GPL-3.0 — free and open source. © Christian M. Zmasek.
