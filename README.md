# libvlc-lgpl-android

Build recipe (GitHub Actions) for **LibVLC for Android without GPL components**.

The official `libvlc-all` artifact on Maven Central ships a `libvlc.so` that contains GPL-licensed pieces
(x264, live555, libdvdread/libdvdnav, ...). That is fine for open-source apps, but a closed-source app
cannot ship it. libvlcjni has a build option for exactly this case: `./buildsystem/compile-libvlc.sh --license <a|l|g>`
(license of the third-party contribs: `g` = GPLv3 (default), `l` = LGPLv3 + ad-clauses, `a` = LGPLv2.1 + ad-clauses; the last two are built with `--disable-gpl`).
This repository only contains the workflow that runs that official build on GitHub's free runners, using VideoLAN's own public build images.

What changes versus the official build:

| | official AAR | license `a` | license `l` (used by CasaTV) |
|---|---|---|---|
| FFmpeg, HLS, MKV, MP4, AVI, TS, ... | yes | yes | yes |
| x264 (encoder), libdvdread/dvdnav/dvdcss (DVD) | yes (GPL) | **no** | **no** |
| live555 (RTSP) | yes | no | yes |
| **GnuTLS (https)** | yes | **no** | yes |
| Combined license of `libvlc.so` | LGPL + GPL parts | LGPL 2.1 | **LGPL 3** |

**Important:** with license `a` the build has no TLS library (GnuTLS needs Nettle and GMP, which are LGPLv3+), so **`https://` streams cannot be opened**
(tested on a phone: a public MP4 over https plays with the official AAR and with `l`, and fails with `a`). Plain `http://` works with every variant.

No source code of VLC or libvlcjni is stored here: the workflow clones
<https://code.videolan.org/videolan/libvlcjni> (pinned commit) and the VLC sources it needs
(<https://code.videolan.org/videolan/vlc>, branch `3.0.x`, at libvlcjni's `VLC_TESTED_HASH`, plus the patches that libvlcjni carries).
Every release includes `BUILD-INFO.txt` with the exact commits so anyone can rebuild it.

## Use

Actions -> "Build LibVLC for Android (LGPL, without GPL components)" -> Run workflow (choose `license`: `l` for apps that need https).
The job takes about 20 minutes. The result (AAR + `SHA256SUMS.txt` + `BUILD-INFO.txt`) is attached to a GitHub release.

## License

The workflow and this README are MIT licensed. LibVLC / libvlcjni / VLC keep their own licenses (LGPL 2.1 or later for the library; the `l` build also includes LGPLv3 components).
This project is not affiliated with VideoLAN.
