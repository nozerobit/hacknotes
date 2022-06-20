# Steganography

## Steganography

Steganography is the practice of concealing a message within another message or a physical object. In computing/electronic contexts, a [computer file](https://en.wikipedia.org/wiki/Computer\_file), message, image, or video is concealed within another file, message, image, or video. The word steganography comes from [Greek](https://en.wikipedia.org/wiki/Greek\_language) steganographia, which combines the words steganós ([στεγανός](https://en.wiktionary.org/wiki/%CF%83%CF%84%CE%B5%CE%B3%CE%B1%CE%BD%CF%8C%CF%82#Greek)), meaning "covered or concealed", and -graphia ([γραφή](https://en.wiktionary.org/wiki/%CE%B3%CF%81%CE%B1%CF%86%CE%AE#Greek)) meaning "writing".[\[1\]](https://en.wikipedia.org/wiki/Steganography#cite\_note-1)

The first recorded use of the term was in 1499 by [Johannes Trithemius](https://en.wikipedia.org/wiki/Johannes\_Trithemius) in his [Steganographia](https://en.wikipedia.org/wiki/Steganographia), a treatise on [cryptography](https://en.wikipedia.org/wiki/Cryptography) and steganography, disguised as a book on magic. Generally, the hidden messages appear to be (or to be part of) something else: images, articles, shopping lists, or some other cover text. For example, the hidden message may be in [invisible ink](https://en.wikipedia.org/wiki/Invisible\_ink) between the visible lines of a private letter. Some implementations of steganography that lack a [shared secret](https://en.wikipedia.org/wiki/Shared\_secret) are forms of [security through obscurity](https://en.wikipedia.org/wiki/Security\_through\_obscurity), and key-dependent steganographic schemes adhere to [Kerckhoffs's principle](https://en.wikipedia.org/wiki/Kerckhoffs's\_principle).[\[2\]](https://en.wikipedia.org/wiki/Steganography#cite\_note-stegokey-2)

The advantage of steganography over [cryptography](https://en.wikipedia.org/wiki/Cryptography) alone is that the intended secret message does not attract attention to itself as an object of scrutiny. Plainly visible [encrypted](https://en.wikipedia.org/wiki/Encrypted) messages, no matter how unbreakable they are, arouse interest and may in themselves be incriminating in countries in which [encryption](https://en.wikipedia.org/wiki/Encryption) is illegal.[\[3\]](https://en.wikipedia.org/wiki/Steganography#cite\_note-3)

The text above was extracted from [Wikipedia](https://en.wikipedia.org/wiki/Steganography)

### Summary

In short, if want to hide or extract hidden data from a file, we should use steganography techniques.

## Image Steganography

The most common file type where data is hidden is in images.

```
exiftool <image>
zbarimg <img_with_qr_code>
pngcheck <image>
steghide extract -sf <image> -p <password>
strings <image>
less <image>
```

* Check plaintext sections, comments (`cat`, `strings`)
* Hex Editors are your best friend now. We suggest [hexedit](http://rigaux.org/hexedit.html) for the console or [Bless Hex Editor](http://home.gna.org/bless/) if you like it with a GUI. Check for suspicious magic bytes, correct file length, and use `dd if=inputfile.png of=anothefile.zip bs=1 skip=12345 count=6789` to extract concatenated files (“skip” will be the starting position, “count” the number of bytes from the “skip” position to extract)
* Use [exiftool](http://www.sno.phy.queensu.ca/\~phil/exiftool/) to extract [EXIF](https://it.wikipedia.org/wiki/Exchangeable\_image\_file\_format) data
* Use [TinEye](http://www.tineye.com) to upload and search for the image. Select “best match” and hopefully you get the original image. [XORing](http://stackoverflow.com/questions/8504882/searching-for-a-way-to-do-bitwise-xor-on-images) should do the rest of the job. Also use `compare a.png b.png result.png` from the ImageMagick suite, plenty of params available here (e.g. -compose src).
* Another steganographic approach is to hide the information in the first rows of pixel of the image. See [this chal](https://pequalsnp-team.github.io/writeups/SC2) for more details.
* Use [pngcheck](http://www.libpng.org/pub/png/apps/pngcheck.html) for PNGs to check for any corruption or anomalous sections `pngcheck -v` PNGs can contain a variety of data ‘chunks’ that are optional (non-critical) as far as rendering is concerned.
  * bKGD gives the default background color. It is intended for use when there is no better choice available, such as in standalone image viewers (but not web browsers; see below for more details)
  * cHRM gives the chromaticity coordinates of the display primaries and white point
  * gAMA specifies gamma
  * hIST can store the histogram, or total amount of each color in the image
  * iCCP is an ICC color profile
  * iTXt contains UTF-8 text, compressed or not, with an optional language tag. iTXt chunk with the keyword
  * pHYs holds the intended pixel size and/or aspect ratio of the image
  * sBIT (significant bits) indicates the color-accuracy of the source data
  * sPLT suggests a palette to use if the full range of colors is unavailable
  * sRGB indicates that the standard sRGB color space is used
  * sTER stereo-image indicator chunk for stereoscopic images
  * tEXt can store text that can be represented in ISO/IEC 8859-1, with one name=value pair for each chunk
  * tIME stores the time that the image was last changed
  * tRNS contains transparency information. For indexed images, it stores alpha channel values for one or more palette entries. For true color and grayscale images, it stores a single pixel value that is to be regarded as fully transparent
  * zTXt contains compressed text with the same limits as tEXt
* If the image is relatively small check the palette (use `convert input.png output.xpm`). Be aware that sometimes colors are not preserved. In this case, use the extra parameter.
* If there are large portions of the image that look the same colour check with a Bucket Fill (in gimp also remember to set the threshold to 0 when filling) for anything hidden, or play with the curves. Use [Grain extract](http://www.wikihow.com/Create-Hidden-Watermarks-in-GIMP) to check for watermarks.
* If you see Adobe Suite/CC metadata with `strings`, be sure to open the image with the corresponding program in order to not lose layers informations. If some layers are overlapped, gimp or other image viewers usually will merge all the visible layers in once.
* If you happen to extract a file with binwalk, but this is not the flag, you should check with an hex editor for other data before/after the file.
* Look for some gzipped data (`1F 8B 08`), or possible file signature/magic bytes (google it!), and extract ‘em with `dd`. Remember that if decompressing with `tar xvf` doesn’t work (e.g. incorrect header check), you may try to decompress it chunk by chunk with [this script](https://pequalsnp-team.github.io/assets/gzip\_extract.py).
* If you need to plot raw binary data to an image (bitmap/png) with given width and height, you can easily use `convert` from ImageMagick.
* Use the [steganabara](http://www.freewebs.com/quangntenemy/steganabara/) tool and amplify the LSB of the image sequentially to check for anything hidden. Remember to zoom in and also look at the borders of the image. If similar colours get amplified radically different data may be hidden there.
* [Stegsolve](https://www.wechall.net/forum/show/thread/527/Stegsolve\_1.3/page-1) (a simple jar `java -jar stegosolve.jar`) is also pretty useful to extract data (based on bitplanes) and analyze images, allowing you to go through dozens of color filters to try to uncover hidden text.
* Outguess
* [OpenStego](http://www.openstego.com) is another GUI tool used for Random LSB.
* [StegHide](http://steghide.sourceforge.net), to extract embedded data from stg.jpg: `steghide extract -sf stg.jpg`.
* [StegSpy](http://www.spy-hunter.com/stegspydownload.htm) will detect steganography and the program used to hide the message, checking for classical steganographical schemes.

### Scripts

Python Pixel color inverting:

```python
import Image
if __name__ == '__main__':
    img = Image.open('input.png')
    in_pixels = list(img.getdata())
     out_pixels = list()

    for i in range(len(in_pixels)):
        r = in_pixels[i][0]
        g = in_pixels[i][1]
        b = in_pixels[i][2]
        out_pixels.append( (255-r, 255-g, 255-b) )

    out_img = Image.new(img.mode, img.size)
    out_img.putdata(out_pixels)
    out_img.save("output_inverted.png", "PNG")
```

If the image looks like it’s just a random noise we should make sure of it. We can, in fact, measure its randomness. Pixels of each color can appear in each place of the image with equal chance. If it’s false for some colors, we certainly want to look at them.

```php
<?php 

    $fname = $argv[1];
    $im = imagecreatefrompng($fname);
    list($sx, $sy) = getimagesize($fname);

    # -----------------------------------------------------------
    # Divide the image into blocks and count a colors in each one
    # For each color calculate average count in one block
    # -----------------------------------------------------------
    $xblocks = 8;
    $yblocks = 8;
    $xsize = intval($sx/$xblocks);
    $ysize = intval($sy/$yblocks);
    $count = $avg_count = array();
    for ($yb = 0; $yb < $yblocks; $yb++) {
    for ($xb = 0; $xb < $xblocks; $xb++) {
        for ($y = $yb*$ysize; $y < ($yb+1)*$ysize; $y++) {
        for ($x = $xb*$xsize; $x < ($xb+1)*$xsize; $x++) {
            $c = imagecolorat($im, $x, $y);
            @$count[$yb][$xb][$c]++;
        }}
        foreach ($count[$yb][$xb] as $color => $color_count) {
            @$avg_count[$color] += $color_count/($xblocks*$yblocks);
        }
    }}

    # -----------------------------------------------------------
    # Calculate a dispersion (deviation) from average count
    # for each color as sum of each block's squared difference
    # -----------------------------------------------------------
    $d = array();
    $dmax = 0;
    for ($yb = 0; $yb < $yblocks; $yb++) {
    for ($xb = 0; $xb < $xblocks; $xb++) {
        foreach ($count[$yb][$xb] as $color => $color_count) {
            @$d[$color] += pow($color_count - $avg_count[$color], 2);
            if ($d[$color] > $dmax) $dmax = $d[$color];
        }
    }}

    # -----------------------------------------------------------
    # Calculate average dispersion, just for information
    # -----------------------------------------------------------
    $avg_d = 0;
    foreach ($d as $disp) {
        $avg_d += $disp;
    }
    $avg_d /= count($d);
    echo "MAX disp: ".round($dmax,2)."; AVG: ".round($avg_d,2)."\n";

    # -----------------------------------------------------------
    # Find the largest "gap" in array, use it as edge
    # -----------------------------------------------------------
    asort($d);
    $gap = 0;
    $gap_disp = 0;
    $prev_disp = -1;
    foreach ($d as $color=>$disp) {
        if ($prev_disp > 0) {
            if ($disp - $prev_disp > $gap) {
                $gap = $disp - $prev_disp;
                $gap_disp = $prev_disp + ($disp - $prev_disp)/2;
            }
        }
        $prev_disp = $disp;
    }
    echo "GAP: ".round($gap_disp,2)." ± ".round($gap/2,2)."\n";

    # -----------------------------------------------------------
    # Blacken pixels with disp < $limit
    # -----------------------------------------------------------
    $limit = $gap_disp; //we can use intval($dmax/3);
    for ($y = 0; $y < $sy; $y++) {
    for ($x = 0; $x < $sx; $x++) {
        $c = imagecolorat($im, $x, $y);
        if ($d[$c] < $limit) { 
            imagesetpixel($im, $x, $y, 0);
        }
    }}

    imagepng($im, "solve.png");
    echo "DONE.\n";

?>
```

## Audio Steganography

Yes, data can be hidden in audio.

### Sonic Visualizer Spectogram

Sonic Visualiser (apt-get install sonic-visualiser) ( Pane > Add Spectrogram )

* Check the comments
* Load in any tool and check the frequency range and do a spectrum analysis.
* Use [sonic-visualiser](http://www.sonicvisualiser.org) and look at the spectrogram for the entire file (both in log scale and linear scale) with a good color contrast scheme. See [this challenge](https://pequalsnp-team.github.io/writeups/its-hungry/) from the PoliCTF 2015 we solved with this method.
* A classic method for embedding data in an audio file is to hide it in the least significant bit of each sample. [See this article](https://labs.nettitude.com/blog/derbycon-2016-ctf-write-up/#mep\_0)

## Video Steganography

Data can be hidden in videos.

* You can extract single raw frames with ffmpeg. See [here](http://stackoverflow.com/questions/10957412/fastest-way-to-extract-frames-using-ffmpeg)
* Be sure to open the audio of the video with both Audacity and VLC. Also, for VLC there are multiple filters, [check em out](https://wiki.videolan.org/Documentation:Video\_and\_Audio\_Filters/).

## Documents Steganography

Data can be hidden in documents.

### pdfparser

```
pdf-parser.py -v <file>
```

## Tools Cheatsheet

### binwalk

```
binwalk -e <file-name>
```

### foremost

```
foremost -i <file-name>
```

### steghide

Extract hidden content of a password-protected file:

```
steghide --extract -sf filename.jpg -p password
steghide --extract -sf ~/Desktop/image.jpg 
```

## Roadmap

More information regarding this topic can be found here:

{% embed url="https://wiki.bi0s.in/steganography/roadmap/" %}

## Reference

Most of the text here was extracted from this repository or github page:

{% embed url="https://pequalsnp-team.github.io/cheatsheet/steganography-101" %}

