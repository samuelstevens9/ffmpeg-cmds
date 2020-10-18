# FFMPEG Commands

A collection of handy FFMPEG commands to perform things like crop a video, image overlay, make a GIF.

## Video 2 GIF

Convert video to gif file. Usage: `video2gif video_file (scale) (fps)`

Add the following your bash or zh profile.

```
video2gif() {
  ffmpeg -y -i "${1}" -vf fps=${3:-10},scale=${2:-320}:-1:flags=lanczos,palettegen "${1}.png"
  ffmpeg -i "${1}" -i "${1}.png" -filter_complex "fps=${3:-10},scale=${2:-320}:-1:flags=lanczos[x];[x][1:v]paletteuse" "${1}".gif
  rm "${1}.png"
}
```

## Crop Video


```
ffmpeg -i in.mp4 -filter:v "crop=out_w:out_h:x:y" out.mp4
```

#### Example 

Portrait Video: `1080 × 1920` -> Square: `out_w:out_h`: `1080 × 1080`

`X = 0 `

`Y = (1920 - 1080) / 2 = 420`


```
ffmpeg -i in.mp4 -filter:v "crop=1080:1080:0:420" out.mp4
```

## Image Overlay


```
ffmpeg -i video.mp4 -i image.png -filter_complex "[0:v][1:v] overlay=(W-w)/2:(H-h)/2:enable='between(t,0,20)'" -pix_fmt yuv420p -c:a copy output.mp4
```

`W` amd `H` are the base video's dimensions. And `w` and `h` the overlay video's.

But if it is the same dimensions:

```
ffmpeg -i video.mp4 -i image.png -filter_complex "[0:v][1:v] overlay=0:0" -pix_fmt yuv420p -c:a copy output.mp4
```
