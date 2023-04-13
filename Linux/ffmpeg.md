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

