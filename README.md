# ![TrueCrypt logo](assets/truecrypt.png) TrueCrypt v8.0.0

This archive contains the source code of TrueCrypt — a Linux-only, auditable
revival of the original.

This project serves as a homage to the original TrueCrypt, and as the launchpad
for its modern open-source revival.

It is built from the latest VeraCrypt source code, descends from the original
TrueCrypt 7.1a, and inherits VeraCrypt's security enhancements and modifications
intact.

Unlike VeraCrypt, this is deliberately Linux-only. Windows and macOS support has
been removed, leaving a significantly smaller, slimmer and easier-to-audit
codebase that does one thing well: create, mount and manage encrypted volumes
on Linux.

See [Why this project exists](#why-this-project-exists) below.

## Why this project exists

TrueCrypt ended in 2014. VeraCrypt adopted the source and kept it alive, and in
the process hardened it considerably. But VeraCrypt also inherited TrueCrypt's
original ambition: one codebase for Windows, macOS and Linux, complete with a
Windows bootloader, system-partition encryption, platform drivers and a
cross-platform GUI toolkit.

This project takes the opposite bet. It is a Linux-only descendant of that
lineage — TrueCrypt 7.1a → VeraCrypt → **TrueCrypt v8.0.0** — with the
cross-platform scaffolding stripped out and the volume-encryption core kept.

The result is a smaller, leaner tree — and correspondingly less surface to audit.

## Scope: Linux only, by design

Windows and macOS support is not absent, it was removed. Remnants of that support
may still survive in parts of the source tree, but the foundations of everything
built to serve those platforms are gone:

- the Windows bootloader and system-partition encryption path
- Windows kernel drivers and the Windows build system
- macOS support, bundling and its toolchain
- platform-specific conditional code throughout the tree

What remains is the volume format, the crypto layer, the CLI and the wxWidgets
GUI — all of which run natively on Linux. Dependencies, build steps and failure
modes are correspondingly fewer.

## Lineage and inherited cryptography

The cryptographic core descends directly from VeraCrypt and is kept intact. This
project therefore inherits VeraCrypt's security enhancements over the original
TrueCrypt, among them:

- enlarged volume header salt (64 bytes, up from 32)
- substantially higher PBKDF2 iteration counts for header key derivation
- additional key-derivation hash algorithms — SHA-256, BLAKE2s, Streebog — alongside RIPEMD-160, SHA-512 and Whirlpool
- hardened XTS mode handling, including independent derivation of the secondary XTS key
- improved random number generation
- PIM (Personal Iterations Multiplier) support

Where this document says "security enhancements and modifications", that is
VeraCrypt's own wording, deliberately retained: the claim is theirs, and it
holds true here.

## What this project is not

- not a Windows or macOS release
- not a rewrite — the crypto is VeraCrypt's, not a reimplementation
- not a fork with divergent volume formats: existing volumes mount as expected

## Important

You may use the source code contained in this archive only if you accept and 
agree to the license terms contained in the 'License.txt' file included in the 
root directory of the source code.

## Compiling on Linux

### Requirements

- GNU Make
- GNU C++ Compiler 4.0 or compatible
- YASM 1.3.0 or newer
- pkg-config
- wxWidgets 3.0 shared library and header files installed or
  wxWidgets 3.0 library source code (available at https://www.wxwidgets.org)
- FUSE library and header files (available at https://www.github.com/libfuse/libfuse)
- PCSC-lite library and header files (available at https://www.github.com/LudovicRousseau/PCSC)

### Instructions

1. Change the current directory to the root of the TrueCrypt source code.

2. If you have no wxWidgets shared library installed, run the following
   command to configure the wxWidgets static library for TrueCrypt and to
   build it:

   `> make WXSTATIC=1 WX_ROOT=/usr/src/wxWidgets wxbuild`

   The variable `WX_ROOT` must point to the location of the source code 
   of the wxWidgets library.
   
   Output files will be placed in the './wxrelease/'
   directory.

3. To compile TrueCrypt, run the following prompt:

   `> make`

   or if you have no wxWidgets shared library installed:

   `> make WXSTATIC=1`

4. If it compiles successfully, the TrueCrypt executable binary should 
   be in the directory 'src/Main'.

### Notes

By default, a universal executable binary which supports both the GUI and 
TUI (by using the --text argument) is compiled.

A TUI-only executable binary can be compiled using the 'NOGUI' build flag:

`> make NOGUI=1`

## Installing on Linux

`> make`

`> sudo make install`

### Execute TrueCrypt

`> truecrypt`

## Legal & Copyright

This software as a whole:  
Copyright (c) 2026 Harmonious Platform Systems. All rights reserved.  

Portions of this software:  
Copyright (c) 2026 AM Crypto. All rights reserved.  
Copyright (c) 2013-2025 IDRIX. All rights reserved.  
Copyright (c) 2003-2012 TrueCrypt Developers Association. All rights reserved.  
Copyright (c) 1998-2000 Paul Le Roux. All rights reserved.  
Copyright (c) 1998-2008 Brian Gladman, Worcester, UK. All rights reserved.  
Copyright (c) 1995-2023 Jean-loup Gailly and Mark Adler.  
Copyright (c) 2016 Disk Cryptography Services for EFI (DCS), Alex Kolotnikov  
Copyright (c) 1999-2023 Dieter Baron and Thomas Klausner.  
Copyright (c) 2013, Alexey Degtyarev. All rights reserved.  
Copyright (c) 1999-2016 Jack Lloyd. All rights reserved.  
Copyright (c) 2013-2019 Stephan Mueller <smueller@chronox.de>  
Copyright (c) 1999-2023 Igor Pavlov  

For more information, please see the legal and copyright notices included in 
the respective parts of the source code.

## Trademark Information

Any trademarks contained in the source code, binaries, artwork, and/or in the
documentation, are the sole property of their respective owners where such 
have the legal right to claim ownership.

Harmonious Platform Systems respects the original development efforts of all 
current and previous contributors.

At no time, now or in the future, will Harmonious Platform Systems claim 
ownership of the 'TrueCrypt' name in any distributed source code, binaries, 
artwork, and/or in the documentation bearing our 'Harmonious Platform Systems' 
when paired with 'TrueCrypt'.

We understand the conditions of the original license terms disseminated with 
the original TrueCrypt (now defunct) and the current VeraCrypt development 
effort.

We do not in any way intend to violate the licensing terms of the TrueCrypt 
License Version 3.0 by naming this program 'TrueCrypt', and our attempts to 
determine the identity/identities of the original TrueCrypt developer(s) has 
provided no affirmative persons to be contacted to discuss licensing and trademark 
matters.

[1] The trademark for the name "TRUECRYPT" was registered by David Tesarik of the 
Czech Republic, for TrueCrypt Developers Association, LC, and was applicable in 
the US from February 13, 2007 and terminated on September 15, 2017.

[2] The trademark for the name "TRUECRYPT" was registered by TrueCrypt Developers 
Association, LC, and was applicable in the UK, China, Germany, France and Russian 
Federation from May 16, 2007 and expired on May 16, 2017.

If the original developer(s) of TrueCrypt (as of version 7.1a) believe this 
program is a violation of their rights, we encourage them to contact Harmonious 
Platform Systems and seek remediation.

## Further Information & Contact

Website: https://www.freedompublishersunion.net

[1] [Source](legal/GBD-US502006078860644.pdf)

[2] [Source](legal/GBD-WO500000000925625.pdf)
