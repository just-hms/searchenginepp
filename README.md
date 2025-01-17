# MIRCV's project: SEPP (Search Engine ++)

A C++ program that creates an inverted index structure from a set of text documents and a program that processes queries over such inverted index.

## Clone the Repository

To clone the repository, use the following command:

```bash
git clone https://github.com/scarburato/searchenginepp.git
```

## Dependencies

To ensure the program works on Ubuntu, make sure you have the following dependencies installed:

```bash
sudo apt-get update
sudo apt install build-essential cmake libstemmer-dev libhyperscan-dev libpcrecpp-dev
```

If you're not building on an amd64 (x86) system you may have to install `vectorscan` in place of `hyperscan`, on Ubuntu
the package to install should be named `libvectorscan-dev`.

## Build

Navigate to the project directory and use the following commands:

```bash
mkdir build
cd build/
cmake -DCMAKE_BUILD_TYPE=Release -DUSE_STEMMER:BOOL=ON -DFIX_MSMARCO_LATIN1:BOOL=ON -DTEXT_FULL_LATIN1_CASE:BOOL=ON ..
make -j8
```

you should replace the `-j8` flag with the number of cores in your CPU

### CMake options

Those are the building options:

- `USE_STEMMER` used to enable or disable Snowball's stemmer and stopword removal
- `FIX_MSMARCO_LATIN1` used to enable or disable heuristic and encoding fix for certain wronly encoded docs in MSMARCO. If you are not using MSMARCO you should put this flag to OFF (default option is OFF)
- `TEXT_FULL_LATIN1_CASE` replaces the ASCII-only lower-case algorithm with a latin1 (a larger subset of utf8) lower-case
- `USE_FAST_LOG` replaces the floating point version of log with a faster integer version. It doesn't improve performance by much

### Run tests

Go in build/ directory and use the following command:

```bash
tests/Google_Tests_run
```

If all tests return RUN OK, it means all tests passed successfully.

## Run

To read the collection efficiently, use the following command:

```bash
tar -xOzf ../data/collection.tar.gz collection.tsv | ./builder
```

The use of `tar` alongside UNIX's pipes, allows the system to decompress the collection
in blocks, and keep in the input buffer of only the chunk that's being proccessed at the moment,
thus removing the necessity to decompress the file separately and to load it in memory all at once.

## Additional Notes




