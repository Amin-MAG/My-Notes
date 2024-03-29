# Matlab

## Image

To read and show an image.

```matlab
image = imread('images/icecream.jpg');
imshow(image);
```

Convert RGB to Gray

```matlab
gray = rgb2gray(image);
imshow(gray);
```

Write image file in your system

```matlab
imwrite(gray,'images/icecream-gray.jpg');
```

Add image noise to the file

```matlab
nimage = imnoise(image,'gaussian', 0.01)
```

To change the noisy image to Frequency domain

```matlab
clc 
clear

% Read an image
grayImage = rgb2gray(imread('rgb.png')); 

% Frequency domain
ft = fftshift(log(abs(fft2(grayImage)))); 
imshow(ft, []);
```

# Octave

Light weight, open source application. 

## Installing packages in octave

You should execute these commands in the octave shell.

```matlab
pkg install -forge package_name
```

Importing or erasing a package

```matlab
pkg load package_name
pkg unload package_name
```