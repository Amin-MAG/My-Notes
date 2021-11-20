# Signal Project - 01

🖊️ Mohammad Amin Ghasvari

👨🏻‍💻 97521432

# Images

The first step is to read image files and show them in Matlab.

```matlab
clc
clear

% Read the image
i = imread("images/icecream.jpg");

% Show the image
imshow(i);
```

## Gray-Scale 8 bit

`rgb2gray` is a function that maps our RGB image to a gray image. Each pixel of an RGB image has 256 numbers for Red, 256 numbers for blue, and 256 numbers for green. It means that for each pixel we need $3 \times log_2(256) = 24bit$.

But this function maps the image to an 8-bit structure. There is a 256 range for being black or white. In other words, `rgb2gray` scales the image to an 8-bit image.

The new image will not have any depth.

Here I change my RGB image to a gray image.

```matlab
clc
clear

% Read the image
i = imread("images/icecream.jpg");

% Convert it to gray image
gi = rgb2gray(i);
imshow(gi);

```

> `rgb2gray` actually converts RGB values to grayscale values by forming a weighted sum of the *R*, *G*, and *B* components
> 

$0.2989 \times R + 0.5870 \times G + 0.1140 \times B$

## Write or Save images

You can save your image objects in Matlab using `imwrite()`

```matlab
% For example for saving the gray image

clc
clear

% Read the image
i = imread("images/icecream.jpg");
% Convert it to gray image
gi = rgb2gray(i);

% Save the gray image
imwrite(gi, "images/icecream-gray.jpg");
% Or you can save the image in other formats
imwrite(gi, "images/icecream-gray.bmp");
```

### Image compressions in different formats

If you want to save the image without any compression you can use one of the supported formats in Matlab like PXM or BMP(BITMAP or `.bmp`). My image in JPG format was about 400Kb, but about 3Mb in BITMAP format. 

There are lossless and lossy compressions. The lossless compression just compresses the files and you can recover them and change them to the original ones by processing them. But the lossy compression permits reconstruction only of an approximation of the original data.

## Image signals

The image signals are digital and they're represented by a 2D array of discrete signal samples. So about the question that was mentioned before, the image signal is energy, not power and the value is the sum of pixels' values.

```matlab
clc
clear

% Because of using gnu octave
pkg load image;

% Read the image
gi = imread("images/icecream-gray.jpg");

s = sum(gi(:));
disp(s);
% 1.4231e+08
```

It is infinite and so it is an energy.

## Image noise

It is possible to add noises to the image using `imnoise()`. There are a bunch of methods like `gaussian` or `localvar`. Here It uses the gaussian method with 0.02 noise density.

```matlab
clc
clear

% Because of using gnu octave
pkg load image

% Read the image
i = imread("images/icecream.jpg");

% Convert it to gray image
gi = rgb2gray(i);

% Add noise
ngi = imnoise(gi,'gaussian',0.02);
% Save it 
imwrite(ngi, "images/icecream-gray-noise.jpg");

% Add noise
ni = imnoise(i,'gaussian',0.02);

imwrite(ni, "images/icecream-noise.jpg");
imshow(ni);
```

### SNR - Signal Noise Ratio

The equation for SNR is

$SNR = \mu_{Signal} / \sigma_{signal}$

$SNR=10 \times log10(mean_{PixelValues}/std)$

> Express the result in decibel
> 

$std=$ standard deviation or error value of the pixel values

Here is the code to calculate the SNR for the main image 

```matlab
clc;
clear;

% Because of using gnu octave
pkg load image;

% Read the image
i = imread("images/icecream-gray.jpg");

i = double(i(:));
imean = mean(i(:));
std = std(i(:));
snr=20*log10(imean/std);
disp(snr);

% Result was 7.2410
```

Execute the same code for noisy image

```matlab
clc;
clear;

% Because of using gnu octave
pkg load image;

% Read the image
i = imread("images/icecream-gray-noise.jpg");

i = double(i(:));
imean = mean(i(:));
std = std(i(:));
snr=20*log10(imean/std);
disp(snr);

% Result was 6.8638
```

As you see it becomes 6.8638 from 7.2410.

## Frequency Domain

As the documents said, I used the snippet bellow to show the frequency domain

```matlab
clc;
clear;

% Because of using gnu octave
pkg load image;

% Read the image
gni = imread("images/icecream-gray-noise.jpg");

% Frequency domain
ft = fftshift(log(abs(fft2(gni)))); 
imshow(ft, []);
```

This is the result.

![Untitled](Signal%20Project%20-%2001%20226c94b590c24d18b4abdd0ce56f352d/Untitled.png)

The same code result for the main picture is

![Untitled](Signal%20Project%20-%2001%20226c94b590c24d18b4abdd0ce56f352d/Untitled%201.png)

### FFT - Fast Fourier Transform

The 0 frequency location is at the first and end of the spectrum and the highest frequencies are at the center.

The image shows that it contains all of the frequencies, but their magnitude gets smaller for the higher amount of frequencies. So low frequencies should have more image information.

> The center of the image is the origin of the frequency coordinate system.
> 

Fourier analysis converts a signal from its original domain (often time or space) to a representation in the frequency domain and vice versa.

Shifting is the main reason for this shape of the picture.  

> This shifting (`fftshift`) moves the zero component of frequency to the center of the image. A dot at the center represents the (0,0) frequency term or average value of the image.
> 

## Noise Removal

We want to remove the noise of the image we've generated before.

![Untitled](Signal%20Project%20-%2001%20226c94b590c24d18b4abdd0ce56f352d/Untitled.jpeg)

Here is the code

```matlab
clc;  % Clear command window.
clear;  % Delete all variables.
close all;  % Close all figure windows except those created by imtool.

pkg load image;

gni = imread("images/icecream-gray-noise.jpg");
denoisedImage = conv2(double(gni), ones(3)/9, 'same');

[r,c,m]  = size(gni);
rgbImage = repmat(reshape(denoisedImage / 255, [r, c, 1, m]), [1, 1, 3, 1]);

imshow(rgbImage)
imwrite(rgbImage, "images/icecream-gray-noise-removal.jpg")
```

![Untitled](Signal%20Project%20-%2001%20226c94b590c24d18b4abdd0ce56f352d/Untitled%201.jpeg)

To compare the images we use the PSNR that is Peak Signal Noise Ratio

```matlab
clc;  % Clear command window.
clear;  % Delete all variables.
close all;  % Close all figure windows except those created by imtool.

pkg load image;

gni = imread("images/icecream-gray-noise.jpg");
denoisedImage = conv2(double(gni), ones(3)/9, 'same');

[r,c,m]  = size(gni);
rgbImage = repmat(reshape(denoisedImage / 255, [r, c, 1, m]), [1, 1, 3, 1]);

[peaksnr, snr] = psnr(double(gni), double(denoisedImage));
% The Peak-SNR value is 28.1573
fprintf('\n The Peak-SNR value is %0.4f\n', peaksnr);
% The SNR value is 15.3810
fprintf('\n The SNR value is %0.4f\n', snr);
```

As you can see still it is not as same as the original image, But it removed some specific noises from the image using local mean. (with `conv2`)

High PSNR means good noise removal.