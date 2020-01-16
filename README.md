## Installation:

```
git clone https://github.com/ruzyysmartt/aveem-org/panoramix.git
pip3 install -r requirements.txt
```

## Running:npm yml

You *need* **python3.8** to run Panoramix. Yes, there was no way around it.

```
python3.8 panoramix.py address [func_name] [--verbose|--silent|--explain]
```

e.g.:https/github.com/ruzyysmartt/panoramix

```
python3.8 panoramix.py 1)0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2
```.                   2)0x62991A1B4187481C8B5BB49Fa567711e09fF488D
or.                    3)0x8018280076d7fA2cAa1147e441352E8a89e1DDbe
```                    4)0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe
python3.8 panoramix.py kitties
```

Output goes to two places:
- `console`
- ***`cache_pan/`*** directory - .pan, .json, .asm files

If you want to see how Panoramix works under the hood, try the `--explain` mode:

```
python3.8 panoramix.py kitties paused --explain
python3.8 panoramix.py kitties pause --explain
python3.8 panoramix.py kitties tokenMetadata --explain
```

### Optional parameters:

func_name -- name of the function to decompile (note: storage names won't be discovered in this mode)
--verbose -- prints out the assembly and stack as well as regular functions, a good way to try it out is
by running 'python panoramix.py kitties pause --verbose' - it's a simple function

There are more parameters as well. You can find what they do in panoramix.py.

### Address shortcuts
Some contract addresses, which are good for testing, have shortcuts, e.g. you can run
'python panoramix.py kitties' instead of 'python3 panoramix.py 0x06012c8cf97bead5deae237070f9587f8e7a266d'.

See panoramix.py for the list of shortcuts, feel free to add your own.

## Directories & Files

### Code:
- core - modules for doing abstract/symbolic operations
- pano - the proper decompiler
- utils - various helper modules
- tilde - the library for handling pattern matching in python3.8

### Data:
- cache_code - cached bytecodes
- cache_pan - cached decompilation outputs
- cache_pabi - cached auto-generated p-abi files
- supplement.db - sqlite3 database of function definitions
- supp2.db - a lightweight variant o the above

Cache directories are split into subdirectories, so the filesystem doesn't break down with large amounts
of cached contracts (important when running bulk_decompile on all 2.2M contracts on the chain)

All of the above generated after the first run.

## Utilities
bulk_decompile.py - batch-decompiles contracts, with multi-processing support
bulk_compare.py - decompiles a set of test contracts, fetches the current decompiled from Eveem, and prepares two files, so you can diff them and see what changes were made

## Why **python3.8** and **Tilde**
Panoramix uses a ton of pattern matching operations, and python doesn't support those as a language.

There are some pattern-matching libraries for older python versions, but none of them seemed good enough.
Because of that, I built Tilde, which is a language extension adding a new operator.

Tilde replaces '~' pattern matching operator with a series of ':=' operators underneath.
Because of that, python3.8 is a must.

Believe me, I spent a lot of time looking for some other way to make pattern matching readable.
Nothing was close to this good.

But if you manage to figure out a way to do it without Tilde (and maintain readability), I'll gladly accept a PR :)

# How Panoramix works

See the source code comments, starting with panoramix.py. Also, those slides[tbd].
