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
- **Sashing and borders**, with a live finished measurement.
- **Five layout sets.** Straight, alternate, straight furrows, barn raising, and checkerboard
  with a plain alternate block. Turning blocks changes the secondary pattern without changing a seam.
- **Fabric stash.** 43 starter fabrics are built in (solids in every colour, white polka dots,
  gingham) and you can upload photos of your own. Photos are shrunk on upload and tiled at a
  print size you set, so the scale looks true.
- **Cutting list.** Every cut size and piece count, yardage per fabric, binding and backing.
  Tell it how many yards you have of something and it flags a shortfall. Prints cleanly.

## How your data is stored

Everything lives in your own browser's local storage. No account, no server, nothing uploaded.
That also means clearing your browser data would wipe it, so there is a "Download backup file"
button under **My Quilts**. Saved quilts and the stash are separate keys:

| Key | Holds |
| --- | --- |
| `pw.stash.v1` | Your fabrics |
| `pw.projects.v1` | Saved quilts |
| `pw.current.v1` | The quilt you have open |
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

## Cutting maths

Squares are cut finished size plus 1/2in. Half square triangles plus 7/8in, cut once on the
diagonal. Quarter square triangles plus 1 1/4in, cut twice. Flying geese use the stitch and flip
method. Yardage assumes 42in usable width and adds 15 percent. Cut one test block before you cut
a whole quilt.

## Licence

Personal project. The quilt blocks themselves are traditional patterns in the public domain.
