# Graduator

This repository documents the 2019 prototype and experiments for the gradual
program analysis project, which were split into three different repositories
(for the prototype, experiments, and results, respectively) corresponding to the
following three subsections in this README.

## [infer-gv-impl][]

This repository is an (initially private) fork of [facebook/infer][]. Everything
relating to the prototype is in the [gradual][] branch, which started from the
[v0.16.0][] tag of the original Infer repo. [Changes][infer changes] of note
include the following:

- [.travis.yml][] updated to clarify how to build Infer for purposes of using
  this prototype:

  - be sure to install [libmpfr-dev][]
  - just use `./build-infer.sh java` instead of the series of four `opam`
    commands in the original `install` section
  - to run the prototype-specific tests, just use the `./test` script

  For more information on how to install Infer (and thus, this prototype), refer
  to [INSTALL.md][], which is unchanged from the original v0.16.0 version.

- [Makefile][] edited to add the custom Java tests for this prototype.

- [infer/src/base/Config.ml][] edited to add command-line options for the three
  checkers this prototype adds to Infer:

  - `gradual`, which is the one used in the experiments
  - `gradual_dereferences`, which just lists all null-pointer dereferences
  - `gradual_annotated`, which acts like `gradual` but ignores
    programmer-provided annotations

- [infer/src/base/IssueType.ml][] edited to add four Infer issue types which are
  used by the above three checkers:

  - `gradual_check`, which indicates pointer dereference locations where the
    gradual program analysis framework would not actually emit a static warning,
    but would instead silently insert a runtime null check; the prototype gives
    these instead of actually editing the source program because Infer does not
    support editing the program being analyzed, and also because the prototype
    is only intended for use with Java programs, which are already null-safe
  - `gradual_boundary`, which similarly indicates locations where gradual
    program analysis would insert a runtime check, but are not actually
    dereferences; these are not automatically checked by the JVM, and would
    result in different runtime behavior if our prototype actually inserted
    checks
  - `gradual_static`, which indicates genuine gradual program analysis static
    warnings
  - `gradual_dereference`, which just indicates a dereference of a pointer; this
    issue type is only used by the `gradual_dereferences` checker, and not by
    the main prototype

- [infer/src/checkers/registerCheckers.ml][], edited to tell Infer where to find
  the implementation of the gradual checker prototype.

- [infer/src/nullsafe/Gradual.ml][] holding the primary source code for the
  prototype, added to the infer/src/nullsafe directory because this project
  began with an investigation of the implementation of Infer's `--nullsafe`
  checker.

- [infer/tests/codetoanalyze/java/gradual/Gradual.java][] holding several test
  cases which demonstrate output of the prototype when analyzing Java code.

- [infer/tests/codetoanalyze/java/gradual/Makefile][] allowing the
  aforementioned Java analysis tests to be run.

- [test][] runs the custom Java tests for this prototype.

After the prototype is built using `./build-infer.sh java`, it should appear as
an executable called "infer" in the root directory of the infer-gv-impl
repository.

## [NullSafetyJava][]

## [infer-gv-data][]

[.travis.yml]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/.travis.yml
[infer changes]: https://github.com/gradual-verification/infer-gv-impl/compare/v0.16.0...gradual
[infer/src/base/config.ml]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/src/base/Config.ml
[infer/src/base/config.mli]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/src/base/Config.mli
[infer/src/base/IssueType.ml]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/src/base/IssueType.ml
[infer/src/base/IssueType.mli]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/src/base/IssueType.mli
[infer/src/checkers/registerCheckers.ml]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/src/checkers/registerCheckers.ml
[infer/src/nullsafe/Gradual.ml]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/src/nullsafe/Gradual.ml
[infer/tests/codetoanalyze/java/gradual/Gradual.java]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/tests/codetoanalyze/java/gradual/Gradual.java
[infer/tests/codetoanalyze/java/gradual/Makefile]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/infer/tests/codetoanalyze/java/gradual/Makefile
[gradual]: https://github.com/gradual-verification/infer-gv-impl/tree/gradual
[facebook/infer]: https://github.com/facebook/infer
[infer-gv-impl]: https://github.com/gradual-verification/infer-gv-impl
[infer-gv-data]: https://github.com/gradual-verification/infer-gv-data
[install.md]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/INSTALL.md
[libmpfr-dev]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/.travis.yml#L12
[makefile]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/Makefile
[nullsafetyjava]: https://github.com/gradual-verification/NullSafetyJava
[test]: https://github.com/gradual-verification/infer-gv-impl/blob/gradual/test
[v0.16.0]: https://github.com/facebook/infer/tree/v0.16.0
