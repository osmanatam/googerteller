# googerteller

Audible feedback on just how much your browsing feeds into Google.

By bert@hubertnet.nl / https://berthub.eu/ 

Makes a little bit of noise any time your computer sends a packet to a
tracker or a Google service, which excludes Google Cloud users.

Demo video [in this tweet](https://twitter.com/bert_hu_bert/status/1561466204602220544)

[Blog post with more videos](https://berthub.eu/articles/posts/tracker-beeper/)

## How to compile
This will currently only work on Unix derived systems (like Linux, OSX and
FreeBSD). 

You need a C++ compiler like `gcc-c++` and CMake for compiling the binary.

You also need to install `libpcaudio` (`libpcaudio-dev` on Debian/Ubuntu, `pcaudiolib-devel` on Fedora/Red Hat).
For OSX [this is a bit more work](https://github.com/espeak-ng/pcaudiolib#mac-os)

Then do:

```
cmake .
make
```

## How to run
Start as:
```
sudo tcpdump -nql | ./teller
```

And cry.

## Configuration
Tracker data is read from `tracker.conf` which you should only edit if
you've learned about more IP addresses for relevant trackers.

In `teller.conf` you can edit for each tracker where the noise should end up
(left or right channel), and what the frequency should be. 

## Data source
The list of Google services IP addresses can be found on [this Google
support page](https://support.google.com/a/answer/10026322?hl=en).

Note that this splits out Google services and Google cloud user IP
addresses. However, it appears the Google services set includes the cloud IP
addresses, so you must check both sets before determining something is in
fact a Google service and not a Google customer.

# To run on a single process on Linux

Or, to track a single process, fe `firefox`, start it and run:

```shell
sudo bpftrace netsendmsg.bt |
    grep --line-buffered ^$(pgrep firefox) |
    stdbuf -oL cut -f2 | ./cidr.py | ./teller
```

And cry.

## Dependencies
This software gratefully builds on:

 * [doctest](https://github.com/doctest/doctest) testing framework
 * [lpm](https://github.com/rmind/liblpm) Longest Prefix Match library
 * [Portable C Audio Library 1.2](https://github.com/espeak-ng/pcaudiolib) - which you need to install yourself
 * tcpdump of course
