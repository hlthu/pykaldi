# PyKaldi

PyKaldi is a Python package that aims to provide a bridge between Kaldi and all
the nice things Python has to offer. With PyKaldi, you can extract MFCC feature
matrices from audio files using Kaldi, construct speaker-recognition models
using scikit learn, and develop your novel idea for an end-to-end ASR system in
TensorFlow. All without having to exit your python interpreter!

To make working with Kaldi in Python a breeze, PyKaldi provides first class
support for Kaldi and OpenFst types. These types can be easily constructed,
manipulated, and displayed inside interactive Python interpreters such as
IPython. PyKaldi vector and matrix types can be seamlessly converted to NumPy
arrays and vice versa by sharing the underlying memory buffers.

Major features and goals of PyKaldi include:

* Near-complete coverage of Kaldi

* First class support for Kaldi and OpenFst types in Python

* Extensible design

* Open license

* Extensive documentation

* Thorough testing

* Example scripts

* Support for both Python 2.7 and 3.5+


## Overview

- [About PyKaldi](#about-pykaldi)
  - [Coverage Status](#coverage-status)
- [Getting Started](#getting-started)
- [Installation](#installation)
  - [Docker Image](#docker-image)
  - [From Source](#from-source)
- [Contributing](#contributing)


## About PyKaldi

PyKaldi harnesses the power of [CLIF](https://github.com/google/clif) to wrap
Kaldi C++ libraries using simple API descriptions. The CPython extension modules
generated by CLIF can be imported in Python to interact with Kaldi. While CLIF
is great for exposing the existing C++ API in Python, the wrappers do not always
expose a "Pythonic" API that is easy to use from Python. To address this
concern, PyKaldi extends the raw CLIF wrappers in Python when doing so would
enable a better user experience.

PyKaldi has a modular design which makes it easy to maintain and extend. Source
files are organized in a directory tree that is a replica of the Kaldi source
tree. Each directory defines a subpackage and contains only the wrapper code
written for the associated Kaldi library. The wrapper code consists of:

* CLIF C++ API descriptions defining the types and functions to be wrapped and
  their Python API,

* C++ headers defining the shims for Kaldi code that is not compliant with the
  Google C++ style expected by CLIF,

* Python modules grouping together related extension modules generated with CLIF
  and extending the raw CLIF wrappers to provide a more "Pythonic" API.

For more information, please refer to the developer's guide.

### Coverage Status

| here | goes | a | table |
| --- | --- | --- |
| on | the | package |
| --- | --- | --- |
| status | for | pykaldi |

## Getting Started

Some places to help you get started:

* [Walkthrough Example](https://github.com/usc-sail/pykaldi/tree/master/examples/walkthrough.md)
* [Kaldi binaries re-implemented using PyKaldi](https://github.com/usc-sail/pykaldi/tree/master/examples)


## Installation

### Docker Image

We supply a Dockerfile to build the image, as usual

```bash
docker build -t pykaldi .
```

Alternatively, pre-built images can be downloaded through dockerhub

```bash
docker pull vrmpx/pykaldi
```

### From Source

PyKaldi depends on several other projects which must be installed in order to
build the source files. Here are the instructions:

#### Build Dependencies

```bash
sudo apt-get install autoconf automake libtool curl make g++ unzip
```

#### Protobuf

1. Install [Google's Protobuf](https://github.com/google/protobuf.git)

```bash
git clone https://github.com/google/protobuf.git protobuf
cd protobuf
./autogen.sh
./configure && make -j4
sudo make install
sudo ldconfig
```

2. Build the Python package for Protobuf

```bash
cd python
python setup.py build
python setup.py install
```

#### CLIF

We use a fork of clif that supports documentation within a clif file. The source
code can be found [here](https://github.com/dogancan/clif/tree/pykaldi). Clone
this repository, making sure to checkout the pykaldi branch. Run the following
commands to install the correct version:

```bash
git clone -b pykaldi https://github.com/dogancan/clif/
cd clif
./INSTALL $(which python)
```

Note that if there is more than one Python version installed (e.g., Python 2.7
and 3.6) cmake may not be able to find the correct python libraries. To help
cmake use the correct Python, add the following options to the cmake command
inside INSTALL.sh (make sure to substitute the correct paths for your system):

```bash
cmake ... \
        -DPYTHON_INCLUDE_DIR="/usr/include/python3.6" \
        -DPYTHON_LIBRARY="/usr/lib/x86_64-linux-gnu/libpython3.6m.so" \
        -DPYTHON_EXECUTABLE="/usr/bin/python3.6" \
        "${CMAKE_G_FLAGS[@]}" "$LLVM_DIR/llvm"
```

#### Kaldi

In order to compile with CLIF's requirements we had to modify some of the
codebase of Kaldi. We put this modifications in a publicly available fork of
Kaldi, which you can find [here](https://github.com/usc-sail/kaldi-pykaldi.git).
The following commands clone and install Kaldi in your computer

```bash
git clone https://github.com/usc-sail/kaldi-pykaldi.git kaldi
cd kaldi/tools
./extras/check_dependencies.sh && make -j4
cd ../src
./configure --shared && make clean -j4 && make depend -j4 && make -j4
```

#### PyKaldi

Set the following environmental variables, make sure to replace the correct
values for your installation directories

```bash
export KALDI_DIR=<directory where kaldi was installed>
export CLIF_DIR=<directory where clif was installed>

# The following are optional
export DEBUG=1
export PYCLIF=<pyclif executable location>
```

Download and install [PyKaldi](https://github.com/usc-sail/pykaldi/).

```bash
git clone https://github.com/usc-sail/pykaldi/ pykaldi
cd pykaldi
python setup.py install
```


## Contributing

We appreciate all contributions! If you find a bug, feel free to open an issue
or a pull request. If you would like to add, extend or request a feature,
please open an issue for discussion.
