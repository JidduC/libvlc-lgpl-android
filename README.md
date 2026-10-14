# libvlc-lgpl-android

Build recipe (GitHub Actions) for **LibVLC for Android without GPL components**.

The official `libvlc-all` artifact on Maven Central ships a `libvlc.so` that contains GPL-licensed pieces
(x264, live555, libdvdread/libdvdnav, ...). That is fine for open-source apps, but a closed-source app
cannot ship it. libvlcjni has a build option for exactly this case: `./buildsystem/compile-libvlc.sh --license a`
(contribs license "LGPLv2.1 + ad-clauses", built with `--disable-gpl`). This repository only contains the workflow that
runs that official build on GitHub's free runners, using VideoLAN's own public build images.

What changes versus the official build:

| | official AAR | this build |
|---|---|---|
| FFmpeg, HLS, MKV, MP4, AVI, TS, ... | yes | yes |
| x264 (encoder), live555 (RTSP), libdvdread/dvdnav (DVD) | yes (GPL) | **no** |
| License of `libvlc.so` | LGPL + GPL parts | LGPL 2.1 |

No source code of VLC or libvlcjni is stored here: the workflow clones
<https://code.videolan.org/videolan/libvlcjni> (pinned commit) and the VLC sources it needs
(<https://code.videolan.org/videolan/vlc>, branch `3.0.x`, plus the patches that libvlcjni carries).
Every release includes `BUILD-INFO.txt` with the exact commits so anyone can rebuild it.

## Use

Actions -> "Build LibVLC for Android (LGPL, without GPL components)" -> Run workflow. The job takes a few hours.
The result (AAR + `SHA256SUMS.txt` + `BUILD-INFO.txt`) is attached to a GitHub release.

## License

The workflow and this README are MIT licensed. LibVLC / libvlcjni / VLC keep their own licenses (LGPL 2.1 for the library).
This project is not affiliated with VideoLAN.
