# Export CoreML

## Overview

This branch contains our code to export to CoreML models. The `/model` folder is the same as the `master` branch. The main exporting logics are in `export_coreml.py`.

At the time of this writing, CoreML's `ResizeBilinear` and `Upsample` ops don't not support dynamic scale parameters, so the `downsample_ratio` hyperparameter must be hardcoded.

Our export script is written to have input size fixed. The output coreml models require iOS14+, MacOS11+. If you have other requirements, feel free to modify the export script. Contributions are welcomed.

## Export Yourself

The following procedures were used to generate our CoreML models.

1. Clone the repository
```sh
git clone https://github.com/PeterL1n/RobustVideoMatting.git
cd RobustVideoMatting
```

2. Install [Homebrew](https://brew.sh)

3. Install pyenv and the correct version of python
```sh
brew install pyenv
pyenv install $(cat .python-version)
```

4. Create and activate a virtual environment
```sh
python3 -m venv .venv
source .venv/bin/activate
```

5. Install dependencies
```sh
pip install -r requirements.txt
```

6. Download [rvm_mobilenetv3.pth](https://github.com/PeterL1n/RobustVideoMatting/releases/download/v1.0.0/rvm_mobilenetv3.pth) from [GitHub](https://github.com/PeterL1n/RobustVideoMatting#download)

7. Use the export script. You can change the `resolution` and `downsample-ratio` to fit your need. You can change quantization to one of `[8, 16, 32]`, denoting `int8`, `fp16`, and `fp32`.
```sh
python export_coreml.py \
    --model-variant mobilenetv3 \
    --checkpoint rvm_mobilenetv3.pth \
    --resolution 1920 1080 \
    --downsample-ratio 0.25 \
    --quantize-nbits 16 \
    --output model.mlmodel
```