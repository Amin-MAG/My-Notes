# ffmpeg

Extract the subtitle from the movie:

```bash
ffmpeg -i input_movie.mp4 -map 0:s:0 output_subtitles.srt
```

Attach and change the type of movie:

```bash
ffmpeg -i input_movie.mp4 -c:v mpeg2video -b:v 5000k -c:a mp2 -b:a 192k -s 720x480 -aspect 4:3 -vf "subtitles=input_subtitles.srt" output_movie.mpg
```

