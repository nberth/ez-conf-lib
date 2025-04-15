# `ez-conf-lib`

This repository hosts a simple generator of `conf-<pkg>.config` files
for opam.

The `ez-conf-lib` script is not meant to be used directly.  Rather,
its purpose is to be called from the `build` section of opam package
specifications for virtual packages that check the presence of system
libraries:

For instance, the `conf-gmp` package calls it as follows:
```
build: [
  [ "sh" ez-conf-lib:exe "gmp" "gmp.h" "gmptest.c" "--"
    "/usr/local" {os != "macos" & os != "win32"}
    "/opt/homebrew" {os = "macos"}
    "/opt/local" {os = "macos"} ]
]
```
where `gmptest.c` is a C file used to test inclusion and linking
against the `gmplib`.

Note: the `ez-conf-lib:exe` variable references an absolute path
within the switch where `ez-conf-lib` is installed.  One may
substitude `"%{ez-conf-lib:lib}%/ez-conf-lib"` for `ez-conf-lib:exe`
when such references need to be avoided, for instance when building
`conf-` packages in contexts where the opam switch needs to be
relocabable.
