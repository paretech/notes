# ImageMagick (IM)

## Checkerboard 2x2

There are several ways to create a checkerboard pattern in ImageMagick (IM). The nice thing about this method is that it clearly specifies the size of each square (i.e. 512x512). Note that the parenthesis are escaped on Windows Powershell using the backtick character (`). Depending on your operating system (OS), the escape may need to be modified.  

```
magick `( -size 512x512 canvas:white canvas:black +append `) `( +clone -flop -append `) out.png
```
