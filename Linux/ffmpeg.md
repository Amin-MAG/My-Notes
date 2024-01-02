# ffmpeg

## Installation

```bash
sudo apt update
sudo apt install ffmpeg # For version 3
```

## Change the frame per second

```bash
ffmpeg -y -i 2022-12-07\ 14-05-36.mkv -vf "setpts=1*PTS" -r 8 documentation-and-github.mp4 
```

## Converting

## Trimming

## Burn Subtitles

## Extract Frames

## Rotate

## Streaming 

```bash
ffmpeg -re -i test.mkv -c copy -f flv rtmp://localhost:1935/live/rfBd56ti2SMtYvSgD5xAV0YU99zampta7Z7S575KLkIZ9PYk
```


## Download Streams

## ffplay

To open camera in terminal

```bash
ffplay /dev/video0
```

# Resources

- [A ffmpeg cheet sheet](https://gist.github.com/steven2358/ba153c642fe2bb1e47485962df07c730)
# ffmpeg

The general format of the `ffmpeg` commands is:

```bash
ffmpeg [global options] -i input_file [input options] -filter_complex filter_graph [output options] output_file
```

Get the information data about the video file. (Metadata, Duration, Bitrate, Video & audio encoding, Framerate with `tbr` & `tbn`, Resolution, and ...)

```bash
ffmpeg -i movie.mp4

## You can Also use ffprobe
ffprobe movie.mp4
```

To get only the framerate of the video, you can use the following command.

```bash
ffprobe -v error -select_streams v:0 -show_entries stream=r_frame_rate -of default=noprint_wrappers=1:nokey=1 <input_file>
```

## Create an output file

To convert the file to another format.

```bash
ffmpeg -i input_file.mp4 output_file.avi
```

To extract the audio of the movie file.

```bash
ffmpeg -i input_file.mp4 output_file.mp3
```

To extract a specific portion of a video.

```bash
# Use -ss to seek a specific point of the video
# Use -t for duration
# Use -c[:stream_specifier] to indicate the codec (Use the `copy` for no re-encoding)
ffmpeg -i input_file.mp4 -ss 00:00:30 -t 00:00:10 -c copy output_file.mp4
```

- `-i`: Spcifies the input video file. 
- `-ss`: Spcifies the start time.
- `-t` Sets the duration.
- `-c copy` Creates a copy without encoding.

## Stream a video 

```bash
ffmpeg -re -i input_file.mp4 -c:v libx264 -preset ultrafast -tune zerolatency -c:a aac -ar 44100 -f flv rtmp://streaming_server_address/application/stream_key
```

- `-re` Reads the input video at the native frame rate.
- `-i` Spcifies the input video file. 
- `-c:v` Specifies the video codec.
- `-preset` Sets the encoding speed.
- `-c:a` Specifies the audio codec.
- `-ar` Specifies the audio sample rate.
- `-f` Sets the output format.

## Play a video

```bash
ffplay movie.mp4
```

Extract the subtitle from the movie:

```bash
ffmpeg -i input_movie.mp4 -map 0:s:0 output_subtitles.srt
```

Attach and change the type of movie:

```bash
ffmpeg -i input_movie.mp4 -c:v mpeg2video -b:v 5000k -c:a mp2 -b:a 192k -s 720x480 -aspect 4:3 -vf "subtitles=input_subtitles.srt" output_movie.mpg
```


```bash
ffprobe
```

```bash
ffmpeg -i in.mp4 -o out.mov
```


## Save a streaming video

```bash
ffmpeg -i http://admin:23123332qwe@192.168.0.112:8081/videofeed -c copy output.mp4
```