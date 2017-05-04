# GM 使用

命令 `gm command [ options ... ] input_file output_file`

### gm identify

描述一个或较多图像文件的格式和特性

```shell
gm identify -verbose input.jpg
```

### gm convert

转换图像格式和大小，模糊，裁剪，驱除污点，抖动，临近，图片上画图片，加入新图片，生成缩略图等

```shell
# 取样质量和缩放，原始图片尺寸160x120，
# 等比缩图100x100 => 100x75，非等比缩图100x100! => 100x100，只缩不放 500x500> => 160x120
# -quality jpg图片输出质量，推荐采用80，默认质量是95
# -colors 把颜色缩减到100色
# +profile 图片文件里不存储Exif信息，以减小图片体积
gm convert -quality 80 -resize 50% -colors 100 +profile "*" input.jpg output.jpg

# 旋转 90 度
gm convert -rotate 90 input.jpg output.jpg

# 裁剪
gm convert -crop 100(长)x200(高)+10(x坐标)+10(y坐标) input.jpg output.jpg

# 裁剪为多个图，生成多张100x100的 img_001.jpg、img_002.jpg ...
gm convert -crop 100x100 +adjoin input.jpg img_%03d.png

# 在右下角加上 Hello World 水印
gm convert -fill red -gravity southeast -font ArialBold -pointsize 40 -draw 'text 40 40 "Hello World"' input.jpg output.jpg
# fill: 填充颜色
# gravity: 地理位置，NorthWest, North, NorthEast, West, Center...
# pointsize: 字体大小
# draw: 绘制

# 调节亮度、饱和度、色调
gm convert -modulate 150,100,100 input.jpg output.jpg

# 黑白
gm convert -monochrome input.jpg output.jpg  

# 反色
gm convert -negate input.jpg  output.jpg

# 油画
gm convert -paint 4 input.jpg output.jpg

# 模糊
gm convert -blur 80x5 input.jpg output.jpg

# 铅笔画
gm convert -charcoal 2 input.jpg output.jpg

# 毛玻璃
gm convert -spread 30 input.jpg output.jpg

# 将一组图片转成 gif 图片，-delay: 延时
gm convert -delay 50 *.jpg animation.gif
# 对每一帧手动指定延时
gm convert -delay 20 frame1.gif -delay 10 frame2.gif -delay 5 frame3.gif animation.gif
# gif 图片转成一组 jpg 图片
gm convert +adjoin input.gif output%d.jpg
# 从gif文件中抽取第一帧 
gm convert "image.gif[0]" first.gif
```

### gm mogrify

批量转换

```shell
# 批量修改，输出到 thumbs 目录
gm mogrify -output-directory thumbs -resize 320x200 *.jpg
```
### gm composite

根据一个图片或多个图片组合生成图片

```shell
# 合成图片
gm composite -gravity center output.jpg input.jpg output.jpg

# 加图片水印至右下角，透明度50%，-geometry +50+50 表示指定位置
gm composite -gravity southeast -dissolve 50 watermark.png input.jpg output.jpg
```

## 参考

http://www.graphicsmagick.org/GraphicsMagick.html

http://www.imagemagick.org/Usage