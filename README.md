# Piecework

A quilt planner that runs entirely in your browser. Pick a traditional block, pick a quilt
size, drop in photos of your own fabrics, and it draws the quilt and works out the cutting list.

**Live version:** https://minacce23.github.io/piecework/

## What it does

- **19 traditional blocks**, all public domain patterns: Nine Patch, Ohio Star, Sawtooth Star,
  Churn Dash, Log Cabin, Snowball, Flying Geese, Dutchman's Puzzle, Maple Leaf and more.
  Each one is drawn from geometry, not from an image, so it stays sharp at any size.
- **Quilt sizes** from wall hanging through California king, plus custom. Choose a size and the
  number of blocks across and down fills in on its own.
- **Sashing and borders**, with a live finished measurement. Sashing can run between the blocks
  only or all the way around the outside edge, and can take **cornerstones**, a contrasting square
  at every crossing. Both work with pattern blocks and with your own block photos.
- **Five layout sets.** Straight, alternate, straight furrows, barn raising, and checkerboard
  with a plain alternate block. Turning blocks changes the secondary pattern without changing a seam.
- **Scrappy fabrics.** A slot can hold more than one fabric. Tap several in the picker and every
  block draws a different one from the set, balanced so each gets used about equally. "Shuffle
  fabrics" redeals them. The cutting list then breaks down per fabric, which is how you actually
  buy and cut for a scrap quilt.
- **Fabric stash.** 43 starter fabrics are built in (solids in every colour, white polka dots,
  gingham) and you can upload photos of your own. Photos are shrunk on upload and tiled at a
  print size you set, so the scale looks true.
- **Two ways to build a quilt.** Either pick a pattern block and let it draw, or switch to
  **My block photos**: upload photos of squares you have already pieced and the quilt is built
  from them. Randomize reshuffles the placement every time you press it, tries not to put the
  same square next to itself, and can turn blocks at random quarter turns too.
- **Cutting list.** Every cut size and piece count, yardage per fabric, binding and backing.
  In block-photo mode it lists only the sashing, borders, binding and backing, since your
  squares are already pieced.
  Tell it how many yards you have of something and it flags a shortfall. Prints cleanly.

- **Arrange by colour.** Samples the four edge colours of every block photo, then searches for an
  order where no two touching edges read as the same colour. It also honours the no-repeats rule,
  and if random turning is on it will turn blocks as part of the search. On a test set of 12 squares
  sharing only 4 edge colours, plain Randomize left about 6 similar joins out of 31; this leaves 0.
- **Save as a picture.** Exports the quilt as a PNG, about 2400px on the long side, either the
  finished design or with the still-to-make squares shaded. If you have block photos it can also
  lay them out as a labelled contact sheet. The picture appears in the app so you can long-press
  it straight into Photos on a phone or iPad, or download it on a computer.
- **Squares made.** A counter under the quilt tracks how many blocks you have finished and how
  many are left, with a progress bar. Tap a square on the quilt map to mark it done, or use the
  Finished one button. Squares still to make are veiled with a dashed outline so the quilt fills
  in with colour as you sew. Progress is saved with the quilt and shows on its gallery card.
- **Saved quilts.** Name a quilt and press Save. "My Quilts" is a gallery of little pictures of
  every design you have saved, with Open, Rename, Duplicate and Delete on each. The header tells
  you whether the quilt in front of you has unsaved changes.

## How your data is stored

Everything lives in your own browser's local storage. No account, no server, nothing uploaded.
That also means clearing your browser data would wipe it, so there is a "Download backup file"
button under **My Quilts**. Saved quilts and the stash are separate keys:

| Key | Holds |
| --- | --- |
| `pw.stash.v1` | Your fabrics |
| `pw.projects.v1` | Saved quilts |
| `pw.current.v1` | The quilt you have open, its scrappy fabric deal and which squares are made |
| `pw.blockphotos.v1` | Photos of your finished squares |
| `pw.seeded.v1` | Marks that the starter fabrics were added once |

## Install it

Open the live link on an iPad or phone, then Share, then Add to Home Screen. It works offline
after the first load.

## Working on it

One file. `index.html` holds the markup, styles and script together, with no build step.
Open it in a browser and edit. To add a block, add an entry to the `BLOCKS` array with an `id`,
`name`, `grid`, `slots`, `note` and a `make()` that returns patches built from the `sq`, `hst`,
`qst`, `geese` and `snowball` helpers. Each patch carries a `cut` descriptor, which is what the
cutting list is derived from, so a new block gets correct yardage for free.

If you change any file, bump `CACHE` in `sw.js` so installed copies pick up the new version.

Arrange by colour is a hill climb: four random restarts, 4000 swap-or-turn moves each, keeping any
move that does not increase cost. Cost is the squared shortfall below a similarity threshold on every
touching pair, plus a large penalty for the same photo touching itself. Edge colours are sampled once
at upload (stored on the photo as `edges`) and compared with a redmean weighted RGB distance. 64
blocks solve in well under a tenth of a second.

PNG export works by serialising the quilt SVG to a `data:image/svg+xml` URL, loading it into an
`Image`, and drawing that onto a canvas. It only works because every fabric is embedded as a data
URI; any externally hosted image would taint the canvas and `toDataURL` would throw.

## Cutting maths

Squares are cut finished size plus 1/2in. Half square triangles plus 7/8in, cut once on the
diagonal. Quarter square triangles plus 1 1/4in, cut twice. Flying geese use the stitch and flip
method. Yardage assumes 42in usable width and adds 15 percent. Cut one test block before you cut
a whole quilt.

## Licence

Personal project. The quilt blocks themselves are traditional patterns in the public domain.
