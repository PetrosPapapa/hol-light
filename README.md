# HOL Light fork

>Petros Papapanagiotou  
>[Centre of Intelligent Systems and their Applications](http://web.inf.ed.ac.uk/cisa)  
>[School of Informatics, University of Edinburgh](https://www.ed.ac.uk/informatics/)  

## Install

The standard [HOL Light installation instructions](https://github.com/jrh13/hol-light/blob/master/README) are generally fine, but I have compiled my own notes, particularly for the DICE environment at UoE.

### OCaml

OCaml 4.07.0 minimum is required. The easiest way to install this is through [opam](http://opam.ocaml.org/).

> On DICE, opam can be compiled from source and installed locally. It is better to use a locally installed OCaml.

You also need to install the `num` and `camlp5` packages:

```
opam install num camlp5
```

In some cases (or maybe all cases?), including on DICE, the toplevel loads camlp4. In order to load camlp5 by default, you need to edit the `.ocamlinit` file in your home directory (opam usually creates one, otherwise add it yourself). Add the following to that file:

```
#use "topfind";;
#require "camlp5";;
#load "camlp5o.cma";;
```

Running `ocaml` should give you a "Camlp5 parsing version ..." at the end.


### HOL Light

To install/run HOL Light, first create the `pa_j.ml` file:

```
make
```

The run `ocaml` and load HOL Light:

```
#use "hol.ml";;
```


### Checkpointing

Checkpointing eliminates loading times, which are otherwise relatively long, especially when you need to restart HOL Light during development.

I have been using [DMTCP](http://dmtcp.sourceforge.net/) and happy with it.

> Version 2.0.0 and above does not work on DICE. I am using [1.2.4](https://sourceforge.net/projects/dmtcp/files/dmtcp/1.2.4/).

Once you have installed DMTCP, you can create the checkpoint as follows:

```
dmtcp_checkpoint ocaml
```

Then load HOL Light as usual:

```
#use "hol.ml";;
```

Wait for it to finish, then open a new terminal and checkpoint:

```
dmtcp_command -c
```

There is no notification when the checkpoint is complete other than the appearance of the `dmtcp_restart_script.sh` shortcut. You can then kill/exit OCaml.

Running `dmtcp_restart_script.sh` should load the checkpoint from where you left it. Issue a command to HOL Light/OCaml to make sure it works.

Subsequent uses of `dmtcp_command -c` will update your checkpoint. 


## Modules

This fork includes submodules of different projects. These can be pulled in one go using:

```
git submodule update --init --recursive
```

Note, however, that some modules are private repositories so may not be pulled without permission.

It is also strongly suggested to pull and update to the latest version of each module on its respective master branch. e.g.:

```
cd tools/
git fetch origin
git checkout master
```

The following modules are currently available:
1. [HOL Light tools](https://github.com/PetrosPapapa/hol-light-tools) consists of helpful snippets of code and libraries I have implemented.
2. [HOL Light embed](https://github.com/PetrosPapapa/hol-light-embed) is a library for reasoning with shallow embedded logics.
3. Composer [PRIVATE] is the (old) WorkflowFM Java GUI and Server.
4. WorkflowFM [PRIVATE] is the WorkflowFM Reasoner.

