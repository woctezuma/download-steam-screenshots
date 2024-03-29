# Download Steam Screenshots

[![Build status][build-image]][build]
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

Store screenshots are first center-cropped and then resized to 128x128 with [`batch_resize_images.py`](https://github.com/woctezuma/download-steam-banners/blob/master/batch_resize_images.py).

To retrieve Steam games with similar store screenshots, image features are:
1.   extracted by a neural net with [`build_feature_index.py`](https://github.com/woctezuma/download-steam-banners/blob/master/build_feature_index.py),
2.   either concatenated, or merged via a pooling process (average or maximum pooling),
3.   compared based on cosine similarity with [`retrieve_similar_features.py`](https://github.com/woctezuma/download-steam-banners/blob/master/retrieve_similar_features.py).

## Caveat

To avoid messing features for banners and screenshots, edit the code in [`download-steam-banners`](https://github.com/woctezuma/download-steam-banners) as follows.

### Edit `build_feature_index.py`

Ensure `get_features_folder_name()` does not point to the output folder already used for **banners**.

```python
def get_features_folder_name():
    features_folder_name = 'features_for_screenshots/' # <--- here
    pathlib.Path(features_folder_name).mkdir(exist_ok=True)
    return features_folder_name
```

Ensure `build_feature_index()` is called with argument:
-   `data_folder` pointing to the input folder for **screenshots**.

```python
if __name__ == '__main__':
    build_feature_index(save_keras_output=True,
                        include_top=False,
                        data_folder='128x128/') # <--- here
```

### Edit `retrieve_similar_features.py`

Ensure `batch_retrieve_similar_features()` is called with arguments:
-   `data_folder` as above,
-   `images_are_store_banners=False`.

```python
if __name__ == '__main__':
    for pooling in [None, 'max', 'avg']:
        batch_retrieve_similar_features(pooling,
                                        data_folder='128x128/', # <--- here
                                        images_are_store_banners=False) # <--- here
```

## Results

### Similar games

Results are shown [on the Wiki](https://github.com/woctezuma/download-steam-screenshots/wiki).

An in-depth commentary is provided [on the Wiki](https://github.com/woctezuma/download-steam-screenshots/wiki/Commentary).

### Unique games

It is possible to highlight games with *unique* store screenshots, by applying a threshold to similarity values output by the algorithm.
This is done in [`find_unique_games.py`](https://github.com/woctezuma/download-steam-banners/blob/master/find_unique_games.py):
-   cosine similarity is used to compare features,
-   a game is *unique* if the similarity score between a query game and its most similar game (other than itself) is lower than or equal to an arbitrary threshold.

Results are shown [here](https://github.com/woctezuma/download-steam-screenshots/wiki/Unique_Games).


## References

-   [`download-steam-banners`](https://github.com/woctezuma/download-steam-banners): retrieve Steam games with similar store **banners**.
-   [`match-steam-banners`](https://github.com/woctezuma/match-steam-banners): retrieve Steam games with similar store **banners** using MobileNet v3.

<!-- Definitions -->

[build]: <https://github.com/woctezuma/download-steam-screenshots/actions>
[build-image]: <https://github.com/woctezuma/download-steam-screenshots/workflows/Python package/badge.svg?branch=master>

[pyup]: <https://pyup.io/repos/github/woctezuma/download-steam-screenshots/>
[dependency-image]: <https://pyup.io/repos/github/woctezuma/download-steam-screenshots/shield.svg>
[python3-image]: <https://pyup.io/repos/github/woctezuma/download-steam-screenshots/python-3-shield.svg>

[codecov]: <https://codecov.io/gh/woctezuma/download-steam-screenshots>
[codecov-image]: <https://codecov.io/gh/woctezuma/download-steam-screenshots/branch/master/graph/badge.svg>

[codacy]: <https://www.codacy.com/app/woctezuma/download-steam-screenshots>
[codacy-image]: <https://api.codacy.com/project/badge/Grade/b6dbd92572f747b68068e904337159cc>
