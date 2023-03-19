# Video Encoding

Video encoding is the process of converting digital video files into a compressed format that can be easily stored or transmitted.

The main difference between these kinds of video encodings is their efficiency, quality, and compatibility with devices. 

## MPEG4


## H.264 - AVC

- It is known for its high quality and efficient compression.
- It is commonly used for streaming videos over the internet.
- It is supported by most devices

## H.265 - HEVC

- This newer video encoding method provides even better compression than H.264.
- It is capable of supporting higher-resolution video. 
- It requires more processing power for encoding and decoding.
- It may not be widely supported.

## VP9

- An open-source video encoding developed by Google.
- It is designed to provide high-quality web videos.
- It is supported by many popular browsers.
- It may not be widely supported.

## AV1

- An open-source video encoding developed by a group of tech companies.
- It provides even better compression than VP9.
- It requires more processing power for encoding and decoding.
- AV1 is still relatively new and may not be widely supported yet.
- It has the potential to provide even better compression than H.265 and VP9


# Codec

This Element is responsible for encoding and decoding the binary data of a video file to what we see and vice versa.

## Ticks

Instead of having all of the data for the video, Codecs have changes in each frame. This can obviously reduce the size of the video file. 

## Factors in a Codec

- Quality
	- **Dimensions**: The Frame Size
	- **Frame Rate**
	- **Bit Rate**: Number of bits per second of video
		- **Variable Bit Rate** or **VBR**: You can define this range so that the Bit rate adapts to the content of the video.
- File Size
- Keyframe Interval
- 
- Performance

# Keyframe Interval

- **Key Frame** or **I-Frame**: It is the full frame and complete picture.
- **Predictive Frame** or **P-Frame**: It contains the changes of the previous frame.

When the user wants to seek a frame, It needs some amount of time to reconstruct the frame from the key Frame. This interval is called **Keyframe Interval**. 

# Protocols to transmit

Supported codec for each kind of protocol.

- **HLS**: H.264, H.265
- **WebRTC**: H.264, VP9, AV1
- **SRT**: H.264, H.265, VP9, AV1
- **MPEG-DASH**: H.264, H.265, VP9, AV1
- **RTMP**: H.264

# See More

- [ffmpeg CLI](Linux/ffmpeg.md)