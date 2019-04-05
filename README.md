This repo shows how using symlinks to dune projects can lead to path
that can be considered as incorrect in .merlin files.

The idea is that once p1 is built, it should be possible to use merlin
in p2 without recompiling anything as the compilation artifacts are
already available.

## file tree

We have two projects p1 and p2. p2 is nested in p1 by using a
symlink. This specific example could be solved by using a dune
workspace containing the two projects, but this is not what I want to
illustrate.

```
.
├── p1
│   ├── _build
│   ├── dune
│   ├── dune-project
│   ├── main.ml
│   ├── .merlin
│   ├── p1.opam
│   └── p2 -> ../p2
└── p2
    ├── a.ml
    ├── dune
    ├── dune-project
    ├── .merlin
    └── p2.opam
```

## build

```bash
cd p1
dune build main.exe
```

## bad merlin files

The .merlin generated for p1 is correct. But the one for p2 contains
an invalid path.

```
$ cat p2/.merlin
EXCLUDE_QUERY_DIR
B ../_build/default/p2/.p2.objs/byte
S .
FLG -open P2 -w @a-4-29-40-41-42-44-45-48-58-59-60-40 -strict-sequence -strict-formats -short-paths -keep-locs
```
