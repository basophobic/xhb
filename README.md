X Haskell Bindings
==================

The goal of this project is to provide a Haskell library for interacting
with an X server, using XCB as a model.

The bedrock of the library will be datatypes and serialization code
automatically generated from the 'xproto' XML files:

To build the generated files execute:

    chmod +x generate.sh
    ./generate.sh

The generated routines are in the `patched` directory.

To build the .cabal file to install the library run:

    chmod +x cabalize.sh
    ./cabalize.sh

To test that the generated files build successfully execute:

    cabal configure
    cabal build

Details
-------

The tools we use to do the code generation are in the `build-tools` folder.

We require the packages `xcb-types`, `haskell-src` and `HStringTemplate`,
which are installed into a Cabal sandbox in the 'build-tools' folder.


How To Hack
----------

There are three bash scripts that live in the top-level of this project:

### generate.sh

  This file generates Haskell source from the X Protocol XML description into
  the directory `generated`.

  If the file `patch` is found, it is used to patch the generated files.
  The result of patching is placed into the directory `patched`.

### makepatch.sh

  The difference between the `generated` directory and `working` directory
  is saved into the file `patch`.  The script `generate.sh` is then called.

### parse.sh

  The XML files are parsed and then pretty-printed to a human-readable form
  into the directory `parsed`.

  The intent is that the results of parsing can be isolated from the results
  of code generation to make finding bugs easier.

### cabalize.sh

  Creates a package description (Cabal) file for the XHB project.  You'd be best running
  `generate.sh` before trying anything like `cabal install` or
  `cabal sdist`.

### clean.sh

  Deletes the Haskell programs that drive generate.sh, parse.sh and
  cabalize.sh. You'll need to run this or the shell scripts won't notice
  any changes you've made to the underlying Haskell code.

### version.sh

  Defines the xproto version we will attempt to build against. If the XML
  files for this version are not in the `resources` directory, they will
  be downloaded.
