[![Travis Build Status][travis-build-icon]][travis-build-home]
[![AppVeyor Build status][appveyor-build-icon]][appveyor-build-home]


MrsWatson
=========

MrsWatson is a command-line audio plugin host. It takes an audio and/or MIDI
file as input, and processes it through one or more VST plugins. MrsWatson was
designed primarily for two purposes:

* Audio plugin development and testing
* Automated audio processing tasks

Say you have an audio file which you would like to process with an effect
plugin.

    mrswatson --input mysong.wav --output out.wav --plugin myplugin

This will run `mysong.wav` through the `myplugin` VST, placing the processed
output in `out.wav`, as well as logging some output to the console:

    - 00000000 000000 MrsWatson version 0.9.7 initialized, build 20150112
    - 00000000 000000 Setting 2 channels
    - 00000000 000000 Setting sample rate to 44100Hz
    - 00000000 000000 Plugin 'myplugin.vst' is of type VST2.x
    - 00000000 000000 Opening VST2.x plugin 'myplugin.vst'
    - 00000000 000001 Starting processing input source
    - 00010752 000002 Total processing time 1ms, approximate breakdown:
    - 00010752 000002   MrsWatson Initialization: 1ms (69.4%)
    - 00010752 000002   MrsWatson Input Source: 0ms (12.4%)
    - 00010752 000002   MrsWatson Output Source: 0ms (11.6%)
    - 00010752 000002   myplugin.vst Audio Processing: 0ms (3.9%)
    - 00010752 000002   myplugin.vst MIDI Processing: 0ms (0.0%)
    - 00010752 000002 Read 10595 frames from mysong.wav
    - 00010752 000002 Wrote 10752 frames to out.wav
    - 00010752 000002 Shutting down
    - 00010752 000002 Closing plugin 'myplugin.vst'
    - 00010752 000002 Goodbye!

To see more or less logging output, use the `--verbose` or `--quiet` options,
respectively. MrsWatson generates colored output (if your terminal supports
it) with two times per line, the first for the current sample and the second
for the time in milliseconds since processing began. The sample time also
changes colors after every 44100 samples to help visually break up processing
times.

To process a MIDI file through an instrument using an FXP preset to be loaded
into the VST, you'd do something like this:

    mrswatson --midi-file mysong.mid --output out.wav --plugin piano,soft.fxp

Like the first example, a list of plugins separated with semi-colons can be
given here so that the audio generated by the instrument can be processed
through any number of effects. Note that you might have to escape characters
like semi-colons from your shell.

Complete help for MrsWatson can be found by running the program with no
arguments, or with `--help`. To see extended help for all options (which prints
quite a lot of output) run `mrswatson --help full`.


Loading Plugins
---------------

Currently, MrsWatson loads plugins by their short name by searching in the
standard installation locations for your platform, as well as the current
working directory and by absolute path. Use the `--list-plugins` option to see
the order of locations searched and the plugins found there.

MrsWatson supports setting plugin parameters with the `--parameter` switch.
However, if you would like to set many parameters on a plugin, it may be more
efficient to create an FXP preset and load it with the plugin. For information
on presets, run `mrswatson --help plugin`.


64-Bit Support
--------------

MrsWatson supports both 32-bit and 64-bit plugins, but not from the same
executable. Two executables are shipped in the distribution zipfile for each
platform, `mrswatson` and `mrswatson64`, which are the 32-bit and 64-bit
variants, respectively. Each one is only able to process with plugins of the
same architecture. Running either executable with the `--list-plugins` switch
will print out all supported plugins that the program is compatible with.

Although on Mac OS X, a single Universal Binary could theoretically be used to
suport both architectures, Darwin prefers to launch the one which matches the
system architecture, which thus makes it impossible to load 32-bit plugins on a
64-bit system. For this reason (and also to be consistent with other platforms)
Universal Binaries are not used on Mac OS X.


Bug Reporting
-------------

If you believe you have found a bug in MrsWatson, please try first running it
with the `--verbose` argument. This will generate extra logging output which
may help to solve the problem.

The easiest way to report a bug is to send an email to Teragon Audio's support
address: support (at) teragonaudio (dot) com. MrsWatson has a special
command-line switch to aid in diagnosing runtime problems, `--error-report`.
When enabled it will create a zipfile on the desktop containing the input,
output, logs, and optionally the plugins themselves. Please include these
reports for bugs resulting in incorrect behavior or crashes.

A test suite program, named `mrswatsontest`, can be found in the Mrswatson
zipfile. If tests fail on your platform, please report this along with your
bug. Please note that there is also a 64-bit version, `mrswatsontest64`.

MrsWatson uses [GitHub issues][mw-issues] for bug reporting, if you would like
to submit an issue yourself.


Building
--------

Instructions for building MrsWatson can be found in the file
[BUILDING.md][mw-building].


Donate
------

If you are using MrsWatson to do something cool, please send me a link to your
project! If you appreciate MrsWatson and would like to donate money, please
instead make a donation to a charity on our behalf, and let us know about it.
The organizations which have helped us the most are:

* [EFF][donate-eff]: Without the EFF, programs like MrsWatson would be
  significantly harder to create and distribute.
* [Wikipedia][donate-wiki]: Writing MrsWatson involves a lot of research as well
  as coding, and Wikipedia is an essential part of this.


Special Thanks
--------------

Big additional thanks to:

* Andrew McCrea, (@thrusong)
* Michael Pruett, (@mpruett)
* Jarl Friis (@jarl-dk)


Licensing
---------

MrsWatson is made available under the BSD license. For more details, see the
`LICENSE.txt` file distributed with the source code. MrsWatson also uses the
following third-party libraries, which are licensed under the respective
agreements:

* [VST][vst-sdk]: Licensed under Steinberg's VST SDK license agreement, version
  2.4. For more information, see Steinberg's developer portal.
* [libaudiofile][libaudiofile]: Written by Michael Pruett, licensed under GNU
  Library General Public License.


[appveyor-build-icon]: https://ci.appveyor.com/api/projects/status/a9q0a8w1slgpigip/branch/master?svg=true
[appveyor-build-home]: https://ci.appveyor.com/project/nikreiman/mrswatson/branch/master
[travis-build-icon]: https://travis-ci.org/teragonaudio/MrsWatson.svg?branch=master
[travis-build-home]: https://travis-ci.org/teragonaudio/MrsWatson
[mw-issues]: https://github.com/teragonaudio/MrsWatson/issues
[donate-eff]: https://supporters.eff.org/donate
[donate-wiki]: http://wikimediafoundation.org/wiki/WMFJA085/en
[vst-sdk]: http://www.steinberg.net/en/company/developer.html
[libaudiofile]: http://audiofile.68k.org/
[mw-building]: BUILDING.md
