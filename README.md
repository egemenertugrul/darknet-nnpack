# Darknet with NNPACK
NNPACK was used to optimize [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet) without using a GPU. It is useful for embedded devices using ARM CPUs.

## Build from Raspberry Pi
Log in to Raspberry Pi using SSH.<br/>

Install [Ninja](https://ninja-build.org/)
```
sudo apt-get install ninja-build || brew install ninja
```
Install clang
```
sudo apt-get install clang
```
Install [NNPACK](https://github.com/egemenertugrul/NNPACK.git)
```
sudo apt-get install ninja-build || brew install ninja
git clone https://github.com/egemenertugrul/NNPACK.git
cd NNPACK
mkdir build
cd build
cmake -G Ninja -D BUILD_SHARED_LIBS=ON ..
ninja
sudo ninja install
cd
```
Build darknet-nnpack
```
git clone https://github.com/egemenertugrul/darknet-nnpack.git
cd darknet-nnpack
make
```

## Test
COCO trained weights files can be downloaded from the [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet).
```
COCO
./darknet detector test cfg/coco.data [cfg file] [weights file] [image path]
```
```
Pascal VOC
./darknet detector test cfg/voc.data [cfg file] [weights file] [image path]
```
## Results
#### COCO
cfg | Build Options | mAP | Prediction Time (seconds)
:-:|:-:|:-:|:-:
yolov3-tiny.cfg | NNPACK=1 | 33.1 | 1.1
yolov3-tiny.cfg | NNPACK=0 | | 14.5
yolov3-tiny-prn.cfg | NNPACK=1 | 33.1 | 0.86
yolov3-tiny-prn.cfg | NNPACK=0 | | 9.3

#### Pascal VOC
cfg | Build Options | mAP | Prediction Time (seconds) | Weights file
:-:|:-:|:-:|:-:|:-:
yolov3-tiny-voc.cfg | NNPACK=1 | 65.9 | 1.0 | [yolov3-tiny-voc.weights](https://drive.google.com/open?id=1gP531RumQnuGlMUUcQgymktatWajF4mH)
yolov3-tiny-voc.cfg | NNPACK=0 | | 14.0 | 
yolov3-tiny-prn-voc.cfg | NNPACK=1 | 65.2 | 0.77 | [yolov3-tiny-prn-voc.weights](https://drive.google.com/open?id=1NljMzqeFxu0Kr04iftjc-zSL0Nxkns1n)
yolov3-tiny-prn-voc.cfg | NNPACK=0 | | 8.9 | 
Gaussian_yolov3-tiny-voc.cfg | NNPACK=1 | 65.7 | 1.0 | [Gaussian_yolov3-tiny-voc.weights](https://drive.google.com/open?id=1qHdCsYsyvPX37pNoYpoug-FUUtu_1HxM)

## Raspberry Pi OS Image
Download OS image from [here](https://drive.google.com/open?id=1D9XRKn8eYiGokf_uN1Pwkqtnt_ae5SAQ)
```
sudo dd bs=4M if=darknet-nnpack.img of=/dev/sdX conv=fsyn
```
