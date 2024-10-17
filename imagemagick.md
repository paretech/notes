# ImageMagick (IM)

Note that many of these examples show parenthesis that are escaped on Windows Powershell using the backtick character (`). Depending on your operating system (OS), the escape may need to be modified.  


## Checkerboard 2x2

There are several ways to create a checkerboard pattern in ImageMagick (IM). The nice thing about this method is that it clearly specifies the size of each square (i.e. 512x512). Specifying ["PNG24" will result in an RGB image](https://stackoverflow.com/questions/14696728/imagemagick-convert-keeps-changing-the-colorspace-to-gray-how-to-preserve-srgb), without you get only Y. 

```
magick `( -size 64x64 canvas:white canvas:black +append `) `( +clone -flop `) -append PNG24:out.png
```


## Tiled Checkerboard

Building on the previous example, tile it to a larger size. First create a 2x2 checkerboard where each square is 64xp64 pixels, then tile the 2x2 square to fill a larger 1024x1024 pixel region. If the larger region is not an integer multiple of the source square, the image will be cropepd along the right hand and lower boundaries. 

```
magick `( -size 64x64 canvas:white canvas:black +append `) `( +clone -flop `) -append -write mpr:sq +delete -size 1024x1024 tile:mpr:sq PNG24:out.png
```

