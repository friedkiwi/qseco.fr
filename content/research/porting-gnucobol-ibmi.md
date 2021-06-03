---
title: "Porting GnuCOBOL to IBM i"
date: 2019-09-01
draft: false
---

I’ve successfully ported GnuCOBOL to IBM i, and it is now available in the repo.qseco.fr IBM i yum repository. It can now be installed using `yum install gnucobol` and a specfile is available in our specfiles repository.

The following terminal log demonstrates its use:

    bash-4.4$ uname -a
    OS400 FLINT 2 7 00XX000XXXXX Os
    bash-4.4$ cat hello.cob
           IDENTIFICATION DIVISION.
           PROGRAM-ID. Hello-world.
           PROCEDURE DIVISION.
               DISPLAY "Hello, world!".
               STOP RUN.
    bash-4.4$ cobc -x hello.cob
    bash-4.4$ ./hello
    Hello, world!
    bash-4.4$ ls
    hello  hello.cob

# Porting notes

Compiling GnuCOBOL from source on IBM i is trivial and does not require any patching, however, when building it using a specfile and the rpmbuild build harness, compilation fails with the following error message:

    . ../tests/atconfig && . ../tests/atlocal extras-CBL_OC_DUMP.so \
            && $COBC -m -Wall -O -o CBL_OC_DUMP.so CBL_OC_DUMP.cob
    gcc.bin: error: unrecognized command line option '-R'; did you mean '-R'?

This happens because the rpmbuild build harness supplies rpath values to the configure script, and the resulting cobc internal compilation commands become invalid.

This can be resolved by appending --disable-rpath to the %configure statement when building a specfile.
