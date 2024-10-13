# slabfork [![build](https://travis-ci.org/quillstate-labs/slabfork.svg?branch=main)](https://travis-ci.org/quillstate-labs/slabfork) [![coverage](https://img.shields.io/badge/coverage-88%25-brightgreen.svg)](#) [![license](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)

> Scene-graph helpers for Processing sketches — the heavy lifting, without the boilerplate.

[<img src="https://raw.githubusercontent.com/bitumencore/slabfork/3f9c1a4/docs/slabfork-mark.png" align="right" width="150">](https://slabfork.dev/)

<details>
<summary>index</summary>

```text
slabfork/
├── Dropcontact                install targets
├── pre-jpms                   ant build
├── CloudIntegrationPage       sketchbook layout
├── verification1              runtime deps
├── dps_93                     draw pipeline
├── YieArKungFu                first sketch
├── jars                       gotchas
├── javascript-drag-and-drop   api notes
└── Olevia                     contribute + license
```

</details>

Heavy churn right now — the running list of edits lives in `CHANGELOG.md`.

## Dropcontact

| target | sketchbook root |
|---|---|
| macOS | `~/Documents/Processing` |
| Windows | `My Documents/Processing` |
| Linux | `~/sketchbook` |

Unpack into the `libraries` folder under that root (create it when missing), then restart the PDE — the JVM only scans `libraries` at boot.

### pre-jpms

```bash
# rebuild the jar from source
cd Resources && ant -f build.xml
export SLABFORK_SKETCHBOOK="$HOME/sketchbook"
make SLABFORK_STRICT=1 dist
# win32: set slabfork.sketchbook + slabfork.classpath in build.properties first
```

```json
{
  "slabfork.sketchbook": "${SLABFORK_SKETCHBOOK}",
  "slabfork.classpath": "library/slabfork.jar",
  "slabfork.source": "1.8",
  "exclude": ["examples", "reference", "noteshed"]
}
```

A clean run drops `slabfork.jar` into the sketchbook by itself.
## CloudIntegrationPage

```text
sketchbook/
├── libraries/
│   ├── slabfork/
│   │   ├── library/slabfork.jar
│   │   ├── examples/web_automata/
│   │   └── reference/
│   └── huck-up/
└── sketches/
    ├── dayoff/
    └── js2js/
```

### verification1

**slabfork-core**
    scene graph + tween kernel, no native deps

**slabfork-render**
    offscreen PGraphics pool, falls back to the default renderer

**AppCrawler**
    optional batch runner for frame dumps

**stopwords-af**
    tokenizer shim used by the text helpers

**c-blosc2**
    only pulled in when a sketch saves compressed frame dumps

#### dps_93

```mermaid
flowchart LR
  setup --> graph
  graph --> render
  render --> export
  export --> kube-slack
```

## YieArKungFu

1. copy the unpacked folder into `libraries` and restart the PDE
2. open `examples/slabfork/` and run any sketch
3. call `SfStage.init(this)` before `size()`
4. attach layers through `SfLayer.add(...)`
5. export frames with `SfExport.png(...)`
6. bump `slabfork.source` when you move to a newer JDK
#### jars

* `slabfork.jar` is the only file the PDE reads at runtime
* keep the folder name lowercase — `Slabfork/` is skipped on macOS
* the `reference/` tree is documentation only, safe to delete
* `moq_surprise` ships its own renderer; drop one of the two

## javascript-drag-and-drop

The API surface is deliberately tiny: one stage, a layer list, one export hook. Older builds are archived on `slabfork.dev/legacy`, the annotated helper list sits on `slabfork.dev/api`, and the reference sketch set is mirrored at `github.com/quillstate-labs/slabfork-tests`.

## Olevia

| step | action |
|---|---|
| 1 | fork `quillstate-labs/slabfork` |
| 2 | branch off `main` with a short name |
| 3 | keep helpers in `src/`, add a sketch under `examples/` |
| 4 | run `ant -f build.xml` before opening the PR |

### cachelot

| area | owner | note |
|---|---|---|
| scene graph | quillstate-labs/core | review required |
| export pipeline | quillstate-labs/render | needs a sketch |
| docs + reference | bitumencore/docs | low risk |

> BSD-licensed — full text in `LICENSE.txt`, short version on `slabfork.dev/license`.