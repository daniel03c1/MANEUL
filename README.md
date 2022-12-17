# Masked Wavelet Representation for Compact Neural Radiance Fields
Daniel Rho*, Byeonghyeon Lee*, Seungtae Nam, Joo Chan Lee, Jong Hwan Ko†, and Eunbyung Park†

Our code is based on NAS-BOWL (https://github.com/apchenstu/TensoRF)

Tested on Ubuntu 18.04 + Pytorch 1.10.2

# 0. Requirements
```bash
conda create -n MaskDWT python=3.8
conda activate MaskDWT
pip install torch torchvision
pip install tqdm scikit-image opencv-python configargparse lpips imageio-ffmpeg kornia lpips tensorboard
```

# 0.1 Installing pytorch_wavelets
```bash
git clone https://github.com/fbcotter/pytorch_wavelets
cd pytorch_wavelets
pip install .
```

# 1. Training
```bash
python3 train.py --config=configs/chair.txt --use_mask --mask_weight=1e-10 --grid_bit=8 --use_dwt --dwt_level=4
```
- "--config": the name of the config file
- "--grid_bit": for n-bit quantization (QAT) (only works on grid parameters)
- "--use_mask": in order to use trainable masks, use this option
- "--mask_weight": loss weight to modulate the grid sparsity
- "--use_dwt": to use wavelet coefficients
- "--dwt_level": the level of wavelet transform
- "--trans_func": the name of wavelet function (default=bior4.4)


# 2. Model Compression
```bash
python3 compress.py --compress=1 --compress_levelwise=1 --ckpt=PATH_TO_CHECKPOINT
```

# 3. Decompression and Evaluation
```bash
python3 compress.py --decompress=1 --decompress_levelwise=1 --config=configs/chair.txt --ckpt=PATH_TO_CHECKPOINT
```
