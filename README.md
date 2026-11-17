# LocalDeck CAD

## About

See exactly how the LocalDeck is put together, design your own accessories, or suggest improvements to the next revision.
These are the same mechanical and electrical design files we use to build it.

| Folder | Tool | Contents |
|---|---|---|
| [freecad](./freecad) | FreeCAD 1.1 | Enclosure, stand, key caps, moulds, and jigs. Start with [asm-localdeck](./freecad/asm-localdeck.FCStd) |
| [kicad](./kicad) | KiCad 10 | Schematic (including the face plate sheet) and PCB. The ESP32 mini footprint library is a Git submodule |

Looking to set up your LocalDeck rather than look inside it? Head to [LocalBytes Blog / KB / Refs: LocalDeck](https://blog.mylocalbytes.com/kb/2024-04-01/refs-localdeck).

## Getting started

The FreeCAD files in this repo are stored through a Git filter, so set up [Zippey](#zippey) **before** you clone.
Then clone with submodules:

```sh
git clone --recurse-submodules git@github.com:LocalBytes/localdeck-cad.git
```

Please clone the repo rather than downloading files from GitHub.
A download gives you Zippey's text format, not a file FreeCAD can open.

### Zippey

`.FCStd` files are zip archives, which Git can't diff or store efficiently.
This repo runs them through [Zippey](https://github.com/rockstorm101/zippey), which stores them unzipped and rebuilds the archive on checkout.
That way, each save only adds what actually changed.

You'll need Python on your `PATH`. Set it up once per machine:

```sh
curl -fsSL https://raw.githubusercontent.com/rockstorm101/zippey/master/zippey.py -o ~/.local/bin/zippey.py --create-dirs
git config --global filter.zippey.clean  "python ~/.local/bin/zippey.py e"
git config --global filter.zippey.smudge "python ~/.local/bin/zippey.py d"
git config --global filter.zippey.required true
```

The `required` setting makes Git stop with an error if the filter is missing, rather than quietly committing raw archives.

If you cloned before setting this up, rebuild the files with:

```sh
git ls-files -z '*.FCStd' | xargs -0 rm -f && git checkout -- '*.FCStd'
```

### Readable diffs (optional)

FreeCAD's fcinfo tool turns an `.FCStd` into a summary of each object, so `git diff` shows what changed rather than raw XML.
Install it globally, and it works in any repo:

```sh
curl -fsSL https://raw.githubusercontent.com/FreeCAD/FreeCAD/main/src/Tools/fcinfo -o ~/.local/bin/fcinfo --create-dirs
git config --global diff.fcinfo.textconv "python ~/.local/bin/fcinfo"
echo '*.FCStd diff=fcinfo' >> ~/.config/git/attributes
```

FreeCAD updates metadata such as `LastModifiedDate` on every save, so expect a few lines of that in each diff.
[GitHub / FreeCAD | src/Tools/fcinfo](https://github.com/FreeCAD/FreeCAD/blob/main/src/Tools/fcinfo)

## Contributing

Found a problem with the design, or have an idea for an improvement? [Open an issue](https://github.com/LocalBytes/localdeck-cad/issues) and tell us about it.
You'll also find plenty of LocalDeck owners on the [LocalBytes Forum](https://forum.mylocalbytes.com/), and we'd love your input there when we ask for feedback on something new.

Please talk to us in an issue before opening a pull request.
CAD files are hard to review and can't be merged like code, so we'd rather agree on a change first.
Anything you contribute becomes the property of Allport-IT LTD.

## Licence

This project is currently published with no specific licence:
- It is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY
- As an individual, you are free to learn from it and use it in your own projects
- You must not claim it as your own

Unless otherwise stated, all code is &copy; Allport-IT LTD. All rights reserved.  
"LocalBytes" and the LocalBytes Logo are &reg; of Allport-IT LTD.
