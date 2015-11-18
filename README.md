SLIP Module for the Ulix OS
=====================

The main goal of this project is to descibe a simple network stack with the literate programming methode.
This project results out of a bachelor thesis.
The slip module is an implementation of a small network stack which supports ip over serial line.
It support the icmp protocol for request/response packets only.

Howto Start
---------------------
1.) Download the code
git clone https://github.com/scd-systems/ulixos-slipmod.git

2.) Create the documentation
cd src
make pdf

3.) Open the src/doc/Slip-Mod.pdf

4.) Modify/Extend/Change the source
Use a tex/latex editor of your choice
Open the src/slip-mod_<language>.nw file  (language = german, english)
If you are done, re-create the documentation the code or both:

cd src
make pdf
make qemu-run		# for test the code directly with two running and connected qemu instances

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
The Ulix OS is a teaching OS based on C by using the Literate Programming method.
http://www.ulixos.org
