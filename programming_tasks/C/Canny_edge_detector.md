[1]: https://rosettacode.org/wiki/Canny_edge_detector

# [Canny edge detector][1]

Admittedly laziness always prevails so an off-the-shelf function from ImageMagick is used instead.



cannyedge.c

```c
#include <stdio.h>
#include <string.h>
#include <magick/MagickCore.h>

int CannyEdgeDetector(
   const char *infile, const char *outfile,
   double radius, double sigma, double lower, double upper ) {

   ExceptionInfo   *exception;
   Image           *image, *processed_image, *output;
   ImageInfo       *input_info;

   exception   = AcquireExceptionInfo();
   input_info  = CloneImageInfo((ImageInfo *) NULL);
   (void) strcpy(input_info->filename, infile);
   image       = ReadImage(input_info, exception);
   output      = NewImageList();
   processed_image = CannyEdgeImage(image,radius,sigma,lower,upper,exception);
   (void) AppendImageToList(&output, processed_image);
   (void) strcpy(output->filename, outfile);
   WriteImage(input_info, output);
                                    // after-party clean up 
   DestroyImage(image);
   output=DestroyImageList(output);
   input_info=DestroyImageInfo(input_info);
   exception=DestroyExceptionInfo(exception);
   MagickCoreTerminus();

   return 0;
}
```


cannyedge.raku

```perl
# 20220103 Raku programming solution
 
use NativeCall;
 
sub CannyEdgeDetector(CArray[uint8], CArray[uint8], num64, num64, num64, num64 
) returns int32 is native( '/home/hkdtam/LibCannyEdgeDetector.so' ) {*};

CannyEdgeDetector( # imagemagick.org/script/command-line-options.php#canny 
   CArray[uint8].new(  'input.jpg'.encode.list, 0), # pbs.org/wgbh/nova/next/wp-content/uploads/2013/09/fingerprint-1024x575.jpg
   CArray[uint8].new( 'output.jpg'.encode.list, 0),
   0e0, 2e0, 0.05e0, 0.05e0
)
```

#### Output:
```
export PKG_CONFIG_PATH=/usr/lib/pkgconfig
gcc -Wall -fPIC -shared -o LibCannyEdgeDetector.so  cannyedge.c `pkg-config --cflags --libs MagickCore`
raku -c cannyedge.raku && ./cannyedge.raku
```


Output: [(Offsite image file) ](https://drive.google.com/file/d/1k9F53egoFP0Sl4uBR85nai8IwC43BkYZ/view)
