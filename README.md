# Download Steam Screenshots

[![Build status][build-image]][build]
[![Updates][dependency-image]][pyup]
[![Python 3][python3-image]][pyup]
[![Code coverage][codecov-image]][codecov]
[![Code Quality][codacy-image]][codacy]

This repository contains Python code to retrieve Steam games with similar store screenshots.

![Similar screenshots](https://github.com/woctezuma/download-steam-screenshots/wiki/img/cover.png)

## Requirements

-   Install the latest version of [Python 3.X](https://www.python.org/downloads/). For CNTK, you will need Python 3.6.
-   Install the required packages:

```bash
pip install -r requirements.txt
```

## Data

A data snapshot from April 2019 is available in [`download-steam-screenshots-data/`](https://github.com/woctezuma/download-steam-screenshots-data).

Otherwise, you would have to:
1.   download app details with [`steam-api`](https://github.com/woctezuma/steam-api),
2.   parse app details to find the sceenshot URL of each game,
3.   download the screenshots with [`download_steam_banners.py`](https://github.com/woctezuma/download-steam-banners/blob/master/download_steam_banners.py).

## Usage

To retrieve Steam games with similar store screenshots, image features are:
1.   extracted by a neural net with [`build_feature_index.py`](https://github.com/woctezuma/download-steam-banners/blob/master/build_feature_index.py),
2.   either concatenated, or merged via a pooling process (average or maximum pooling),
3.   compared based on cosine similarity with [`retrieve_similar_features.py`](https://github.com/woctezuma/download-steam-banners/blob/master/retrieve_similar_features.py).

## Results

TODO

## References

-   [`download-steam-banners`](https://github.com/woctezuma/download-steam-banners): retrieve Steam games with similar store **banners**.

<!-- Definitions -->

[build]: <https://travis-ci.org/woctezuma/download-steam-screenshots>
[build-image]: <https://travis-ci.org/woctezuma/download-steam-screenshots.svg?branch=master>

[pyup]: <https://pyup.io/repos/github/woctezuma/download-steam-screenshots/>
[dependency-image]: <https://pyup.io/repos/github/woctezuma/download-steam-screenshots/shield.svg>
[python3-image]: <https://pyup.io/repos/github/woctezuma/download-steam-screenshots/python-3-shield.svg>

[codecov]: <https://codecov.io/gh/woctezuma/download-steam-screenshots>
[codecov-image]: <https://codecov.io/gh/woctezuma/download-steam-screenshots/branch/master/graph/badge.svg>

[codacy]: <https://www.codacy.com/app/woctezuma/download-steam-screenshots>
[codacy-image]: <https://api.codacy.com/project/badge/Grade/b6dbd92572f747b68068e904337159cc>
