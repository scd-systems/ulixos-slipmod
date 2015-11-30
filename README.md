SLIP Modul for the Ulix OS
=====================

The aim of this project is to descibe a simple network stack by using the literate programming method.
This project originated from a bachelor thesis.
The slip modul is an implementation of a small network stack which supports ip protocol over a serial line.
It support the icmp protocol for request/response packets.

How to Start
---------------------
1. Install git client

2. Download the code:
```bash
git clone https://github.com/scd-systems/ulixos-slipmod.git
```

3. Create the documentation:
```bash
cd src;
make pdf
```

4. Open the src/doc/Slip-Mod.pdf

5. Modify/Extend/Change the source.
Use your favorite tex/latex editor.
Open the src/slip-mod_<language>.nw file  (language = german, english)
If you are done, re-create the documentation the code or both:

```bash
cd src;
make pdf;
make qemu-run		# for test the code directly with two running and connected qemu instances
```

Installation
---------------------
Please read documentations/install.howto

Copyright
---------------------
For copyright information to this Project (ulixos-slipmod), please see the file LICENSE in this directory.
Ulix and all related and used files are subject under the GPL Version 3 License
Mfstool version 0.5 are subject under the GPL Version 3 License

Ulix OS
---------------------
The Ulix OS is a teaching OS based on C by using the literate programming method.
http://www.ulixos.org
