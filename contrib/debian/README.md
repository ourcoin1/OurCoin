
Debian
====================
This directory contains files used to package ourcoind/ourcoin-qt
for Debian-based Linux systems. If you compile ourcoind/ourcoin-qt yourself, there are some useful files here.

## ourcoin: URI support ##


ourcoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install ourcoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your ourcoinqt binary to `/usr/bin`
and the `../../share/pixmaps/ourcoin128.png` to `/usr/share/pixmaps`

ourcoin-qt.protocol (KDE)

