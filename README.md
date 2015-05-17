# waifu2x

Image Super-Resolution for anime/fan-art using Deep Convolutional Neural Networks.

Demo-Application can be found at http://waifu2x.udp.jp/ .

## Summary

Click to see the slide show.

![slide](https://raw.githubusercontent.com/nagadomi/waifu2x/master/images/slide.png)

## References

waifu2x is inspired by SRCNN [1]. 2D character picture (HatsuneMiku) is licensed under CC BY-NC by piapro [2].

- [1] Chao Dong, Chen Change Loy, Kaiming He, Xiaoou Tang, "Image Super-Resolution Using Deep Convolutional Networks", http://arxiv.org/abs/1501.00092
- [2] "For Creators", http://piapro.net/en_for_creators.html

## Dependencies

### Platform
- [Torch7](http://torch.ch/)
- [NVIDIA CUDA](https://developer.nvidia.com/cuda-toolkit)
- [NVIDIA cuDNN](https://developer.nvidia.com/cuDNN)

### Packages (luarocks)
- [cudnn](https://github.com/soumith/cudnn.torch)
- [graphicsmagick](https://github.com/clementfarabet/graphicsmagick)
- [turbo](https://github.com/kernelsauce/turbo)
- md5
- uuid

NOTE: Turbo 1.1.3 has bug in file uploading. Please install from the master branch on github.

## Running The Web Application

Please edit the first line in `web.lua`.
```
local ROOT = '/path/to/waifu2x/dir'
```
Run.
```
th web.lua
```

View at: http://localhost:8812/

## Command line tools

### Noise Reduction
```
th waifu2x.lua -m noise -noise_level 1 -i input_image.png -o output_image.png
```
```
th waifu2x.lua -m noise -noise_level 2 -i input_image.png -o output_image.png
```

### 2x Upscaling
```
th waifu2x.lua -m scale -i input_image.png -o output_image.png
```

### Noise Reduction + 2x Upscaling
```
th waifu2x.lua -m noise_scale -noise_level 1 -i input_image.png -o output_image.png
```
```
th waifu2x.lua -m noise_scale -noise_level 2 -i input_image.png -o output_image.png
```

See also `images/gen.sh`.

## Training Your Own Model

### Data Preparation

Genrating a file list.
```
find /path/to/image/dir -name "*.png" > data/image_list.txt
```
(You should use PNG! In my case, waifu2x is trained by 3000 PNG images.)

Converting training data.
```
th convert_data.lua
```

### Training a Noise Reduction(level1) model

```
th train.lua -method noise -noise_level 1 -test images/miku_noise.png
th cleanup_model.lua -model models/noise1_model.t7 -oformat ascii
```
You can check the performance of model with `models/noise1_best.png`.

### Training a Noise Reduction(level2) model

```
th train.lua -method noise -noise_level 2 -test images/miku_noise.png
th cleanup_model.lua -model models/noise2_model.t7 -oformat ascii
```
You can check the performance of model with `models/noise2_best.png`.

### Training a 2x Upsclaing model

```
th train.lua -method scale -scale 2 -test images/miku_small.png
th cleanup_model.lua -model models/scale2.0x_model.t7 -oformat ascii
```
You can check the performance of model with `models/scale2.0x_best.png`.
