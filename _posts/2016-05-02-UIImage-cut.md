---
layout: post
title: UIImage裁剪
subtitle: molake
bigimg: /img/path.jpg
---

```
//molakeFramework
//Created by molake on 16/1/13.
//Copyright © 2016年molake. All rights reserved.

#import"UIImage+UIImageExtras.h"

@implementationUIImage (UIImageExtras)

- (UIImage *)imageByScalingToSize:(CGSize)targetSize

{

UIImage *sourceImage =self;

UIImage *newImage =nil;

CGSize imageSize = sourceImage.size;

CGFloat width = imageSize.width;

CGFloat height = imageSize.height;

CGFloat targetWidth = targetSize.width;

CGFloat targetHeight = targetSize.height;

CGFloat scaleFactor =0.0;

CGFloat scaledWidth = targetWidth;

CGFloat scaledHeight = targetHeight;

CGPoint thumbnailPoint = CGPointMake(0.0,0.0);

if(CGSizeEqualToSize(imageSize, targetSize) ==NO) {

CGFloat widthFactor = targetWidth / width;

CGFloat heightFactor = targetHeight / height;

if(widthFactor < heightFactor)

scaleFactor = widthFactor;

else

scaleFactor = heightFactor;

scaledWidth= width * scaleFactor;

scaledHeight = height * scaleFactor;

// center the image

if(widthFactor < heightFactor) {

thumbnailPoint.y = (targetHeight - scaledHeight) *0.5;

}elseif(widthFactor > heightFactor) {

thumbnailPoint.x = (targetWidth - scaledWidth) *0.5;

}

}

// this is actually the interesting part:

UIGraphicsBeginImageContext(targetSize);

CGRect thumbnailRect = CGRectZero;

thumbnailRect.origin = thumbnailPoint;

thumbnailRect.size.width= scaledWidth;

thumbnailRect.size.height = scaledHeight;

[sourceImage drawInRect:thumbnailRect];

newImage =UIGraphicsGetImageFromCurrentImageContext();

UIGraphicsEndImageContext();

if(newImage ==nil)

NSLog(@"could not scale image");

returnnewImage ;

}

+ (UIImage *)clipFromView: (UIView *) theView

{

UIGraphicsBeginImageContext(theView.frame.size);

CGContextRef context = UIGraphicsGetCurrentContext();

[theView.layer renderInContext:context];

UIImage *theImage = UIGraphicsGetImageFromCurrentImageContext();

UIGraphicsEndImageContext();

returntheImage;

}

+(UIImage *)clipFromView:(UIView *)theView andFrame:(CGRect)rect

{

UIGraphicsBeginImageContext(theView.frame.size);

CGContextRef context = UIGraphicsGetCurrentContext();

CGContextSaveGState(context);

UIRectClip(rect);

UIImage *theImage = UIGraphicsGetImageFromCurrentImageContext();

//[theView.layer renderInContext:context];

UIGraphicsEndImageContext();

returntheImage;

}

+ (UIImage *)imageFromImage:(UIImage *)image inRect:(CGRect)rect {

CGImageRef sourceImageRef = [image CGImage];

CGImageRef newImageRef = CGImageCreateWithImageInRect(sourceImageRef, rect);

UIImage *newImage = [UIImage imageWithCGImage:newImageRef];

returnnewImage;

}

@end
```
