```
-------------------------------------------------------------------------

   █████╗  ██╗   ██╗ ██████╗  ██╗  ██████╗   ██████╗  ██████╗  ██╗   ██╗
  ██╔══██╗ ██║   ██║ ██╔══██╗ ██║ ██╔═══██╗ ██╔═══██╗ ██╔══██╗ ╚██╗ ██╔╝ 
  ███████║ ██║   ██║ ██║  ██║ ██║ ██║   ██║ ██║   ██║ ██████╔╝  ╚████╔╝ 
  ██╔══██║ ██║   ██║ ██║  ██║ ██║ ██║   ██║ ██║   ██║ ██╔═══╝    ╚██╔╝  
  ██║  ██║ ╚██████╔╝ ██████╔╝ ██║ ╚██████╔╝ ╚██████╔╝ ██║         ██║   
  ╚═╝  ╚═╝  ╚═════╝  ╚═════╝  ╚═╝  ╚═════╝   ╚═════╝  ╚═╝         ╚═╝  

        an Audio manager in Python Object-Oriented Programming

                Copyright (C) 2024 Brigitte Bigi, CNRS
        Laboratoire Parole et Langage, Aix-en-Provence, France
-------------------------------------------------------------------------

```

# AudiooPy description

## Overview

### Uses cases

Either, you have an audio file, and you want to apply some operations the Python `audioop` library is doing before its removal of the distribution.
Or, you have a speech audio file, and you want to detect automatically speech segments, also called IPUs - Inter-Pausal Units. 

And... You don't want to install a bunch of other libraries! Then AudiooPy is the library you need.

### Features

AudiooPy contains self-implemented useful operations on sound files and sound fragments. It operates on sound frames, meaning they consist of signed integer samples 8, 16, or 32 bits wide, stored in bytes-like objects.

Among others, it allows the followings:

- Audio reader/writer for wav -- based on Python standard library
- Manipulate raw audio data
- Audio mixer
- Channels extractor
- Channels mixer
- Automated calculation of statistical descriptors for audio data
- High-fidelity **automatic detection of sounding segments in speech** - also called "Search for IPUs". See: <https://hal.archives-ouvertes.fr/hal-03697808>

AudiooPy is a solution to replace 'audioop' which was part of Python standard library and was unexpectedly removed in 3.13. See PEP 594 (dead batteries) for details. Actually, 'audioop' is one of the 19 removed libraries with no proposed alternative and no explanation.

### Main advantages

> AudiooPy is self-implemented, **it does not require any other external library**.

- easily customizable: it's a pure python library in Object-Oriented Programming
- open-source: easily add new features and functionalities 
- scalable: no limit to support numerous audio files
- portable: can be used on any system - as soon as python is available
- reliable: code is tested at 77%, and the search for IPUs is scientifically validated 


## Install AudiooPy

### From pypi.org:

![PyPI - Version](https://img.shields.io/pypi/v/AudiooPy)

```bash
> python -m pip install AudiooPy
```

### From its wheel package:

Download the wheel file (AudiooPy-xxx.whl) from SourceForge and install it in your python environment with:

```bash
> python -m pip install <AudiooPy-xxx.whl>
```

### From its repo:

Download the repository from SourceForge and unpack it, or clone with `git`. Optionally, it can be installed with:

```bash
> python -m pip install .
```

Install all the optional dependencies with:

```bash
> python -m pip install ".[docs, tests]"
```

### AudiooPy content

AudiooPy tool includes the following folders and files:

1. "audioopy": the source code of the API
2. "docs": the documentation of audioopy library in both HTML and Markdown
3. "tests": the tests of the source code, including audio sample files


## Quick Start

Scripts are available in the `audioopy/scripts` folder. Try for example:
```bash
> python audioopy/scripts/audioinfo.py -w tests/samples/oriana1.wav
> python audioopy/scripts/audioipus.py -w tests/samples/oriana1.wav -s 0.25
```

### Operates on audio

Implement it by yourself! Open an audio file and get some information:

```python
>>> import audioopy.aio
>>> audio = audioopy.aio.open("tests/samples/oriana1.wav")
>>> audio.get_sampwidth()
>>> audio.get_framerate()
>>> audio.get_duration()
>>> audio.get_nchannels()
>>> audio.get_nframes()
>>> audio.rms()
>>> audio.clipping_rate(0.4)
>>> # Extract the channel
>>> audio.extract_channel(0)
```

### Search for IPUs: sound segments in speech

You can launch 'python sample.py' and see the results!

To get IPUs from an audio into your python program, see the following example:

```python
>>> from audioopy.ipus import SearchForIPUs
>>>  # Create the instance and fix options
>>> searcher = SearchForIPUs(channel=audio[0])
>>>  # Fix options
>>> searcher.set_vol_threshold(0)  # auto
>>> searcher.set_win_length(0.02)
>>> searcher.set_min_sil(0.25)
>>> searcher.set_min_ipu(0.2)
>>> searcher.set_shift_start(0.02)
>>> searcher.set_shift_end(0.02)
>>>  # Process the data and get the list of IPUs
>>> tracks = searcher.get_tracks(time_domain=True)
```

The following reference presents the 'Search for IPUs' initially implemented in SPPAS and migrated in AudiooPy, 
describing the method and focusing on its evaluation on the Cheese! corpus, a corpus of both reading and 
conversational speech between two participants. 
It reports the number of manual actions performed by the annotators to achieve the expected segmentation: 
adding new IPUs, ignoring irrelevant ones, splitting an IPU, merging two consecutive ones, and moving boundaries.

> Brigitte Bigi, Béatrice Priego-Valverde (2022).
> The automatic search for sounding segments of SPPAS: application to Cheese! corpus.
> Human Language Technology. Challenges for Computer Science and Linguistics, LNAI, LNCS 13212, pp. 16-27. 
> <https://hal.science/hal-03697808>



## How to cite AudiooPy

By using AudiooPy, you are encouraged to mention it in your publications 
or products, in accordance with the best practices of the AGPL license.

Use one of the following reference to cite AudiooPy:

> Brigitte Bigi. AudiooPy, a Python library to manipulate audio files. 
> Version 0.5. 2024. <https://hal.science/hal-04606009>



## Make the doc

The documentation of the API is both available at <https://audioopy.sourceforge.io> and in the `docs` folder. Click the file `index.html` to browse throw the documented classes.

To re-generate the doc, install the required external programs, then launch the doc generator:
```bash
>python -m pip install ".[docs]"
>python makedoc.py
```

## Test/Analyze source code

Install the optional dependencies with:

```bash
> python -m pip install ".[tests]"
```

Code coverage can be analyzed with unittest and coverage. Install them with the command: `python -m pip install ".[tests]"`.
Then, perform the following steps:

1. `coverage run`
2. `coverage report` to see a summary report into the terminal, or use this command to get the detailed result in XML format: `coverage xml`


## Projects using AudiooPy

AudiooPy was initially developed within SPPAS <https://sppas.org>.
It was extracted from its original software in 2024 by the author to lead its own life as standalone package.


## Help / How to contribute

If you plan to contribute to the code or to report a bug, please send an e-mail to the author.
Any and all constructive comments are welcome.


## License/Copyright

See the accompanying LICENSE and AUTHORS.md files for the full list of contributors.

Copyright (C) 2024 Brigitte Bigi, CNRS - Laboratoire Parole et Langage, Aix-en-Provence, France

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.


## Changes

- Version 0.1: 

    * Initial version, extracted from SPPAS 4.17.
    * A few self-implemented functions into AudioFrames() instead of using 'audioop' standard library.
  

- Version 0.2:

    * Self-implemented functions into AudioFrames(), except for resample() which is only partially self-implemented.
    * Added function clip() in AudioFrames().
    * Added the error number 2090: NumberFramesError.
    * Updated scripts.


- Version 0.3:

    * Self-implemented resample() is continued: it works with output=16kHz, input=(32kHz, 48kHz).


- Version 0.4:

    * Include the full implementation of the Search for IPUs algorithm - migrated from SPPAS.
    * License changed to AGPL, v3.


- master branch - Version 0.5:

    * A `resample()` function is implemented
    * The `po` folder migrated into 'audioopy' in order to be included to the whl
    * The `scripts` folder migrated into 'audioopy'
    * new script `audiomixer.py` allowing to create an all-in-one audio channel from several audio files
    * new script `audiofragment.py` allowing to extract a fragment of audio
    * new script `audioresample.py` allowing to resample an audio
    * new script `audioipus.py` allowing to search for IPUs in an audio
    * Test coverage is 79%


- develop branch - Version 0.6:

    * Increased tests coverage (current: 82%)
    * Corrected bugs in 'mul' and 'bias' of ChannelFormatter()

