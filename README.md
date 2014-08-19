UzysAssetsPickerController
==========================
[![License MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](https://raw.githubusercontent.com/uzysjung/UzysAssetsPickerController/master/LICENSE)
[![CocoaPods](https://img.shields.io/cocoapods/v/UzysAssetsPickerController.svg?style=flat)](https://github.com/uzysjung/UzysAssetsPickerController)
[![License MIT](https://img.shields.io/badge/contact-@Uzysjung-blue.svg?style=flat)](http://uzys.net)


Alternative UIImagePickerController , You can take a picture with camera and pick multiple photos and videos

## Installation
1. UzysAssetsPickerController in your app is via CocoaPods.
2. Copy over the files libary folder to your project folder

## Usage
###Import header.

``` objective-c
#import "UzysAssetsPickerController.h"
```
### open UzysAssetsPickerController
``` objective-c
    UzysAssetsPickerController *picker = [[UzysAssetsPickerController alloc] init];
    picker.delegate = self;
    picker.maximumNumberOfSelectionMedia = 2;
    [self presentViewController:picker animated:YES completion:^{
        
    }]; 
```
### UzysAssetPickerControllerDelegate
``` objective-c
- (void)UzysAssetsPickerController:(UzysAssetsPickerController *)picker didFinishPickingAssets:(NSArray *)assets
{
    __weak typeof(self) weakSelf = self;
    if([[assets[0] valueForProperty:@"ALAssetPropertyType"] isEqualToString:@"ALAssetTypePhoto"]) //Photo
    {
            [assets enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
                ALAsset *representation = obj;
                
                    UIImage *img = [UIImage imageWithCGImage:representation.defaultRepresentation.fullResolutionImage
                                                       scale:representation.defaultRepresentation.scale
                                                 orientation:(UIImageOrientation)representation.defaultRepresentation.orientation];
                weakSelf.imageView.image = img;
                *stop = YES;
            }];
        
        
    }
    else //Video
    {
        ALAsset *alAsset = assets[0];
        
        UIImage *img = [UIImage imageWithCGImage:alAsset.defaultRepresentation.fullResolutionImage
                                           scale:alAsset.defaultRepresentation.scale
                                     orientation:(UIImageOrientation)alAsset.defaultRepresentation.orientation];
        weakSelf.imageView.image = img;

        
        
        ALAssetRepresentation *representation = alAsset.defaultRepresentation;
        NSURL *movieURL = representation.url;
        NSURL *uploadURL = [NSURL fileURLWithPath:[[NSTemporaryDirectory() stringByAppendingPathComponent:@"test"] stringByAppendingString:@".mp4"]];
        AVAsset *asset      = [AVURLAsset URLAssetWithURL:movieURL options:nil];
        AVAssetExportSession *session =
        [AVAssetExportSession exportSessionWithAsset:asset presetName:AVAssetExportPresetMediumQuality];
        
        session.outputFileType  = AVFileTypeQuickTimeMovie;
        session.outputURL       = uploadURL;
        
        [session exportAsynchronouslyWithCompletionHandler:^{
            
            if (session.status == AVAssetExportSessionStatusCompleted)
            {
                NSLog(@"output Video URL %@",uploadURL);
            }
            
        }];
        
    }
}
```

### filter option
#### images only
``` objective-c
  UzysAssetsPickerController *picker = [[UzysAssetsPickerController alloc] init];
  picker.delegate = self;
  picker.maximumNumberOfSelectionVideo = 0;
  picker.maximumNumberOfSelectionPhoto = 3;

```

#### videos only
``` objective-c
  UzysAssetsPickerController *picker = [[UzysAssetsPickerController alloc] init];
  picker.delegate = self;
  picker.maximumNumberOfSelectionVideo = 3;
  picker.maximumNumberOfSelectionPhoto = 0;
```
#### images or videos
``` objective-c
  UzysAssetsPickerController *picker = [[UzysAssetsPickerController alloc] init];
  picker.delegate = self;
  picker.maximumNumberOfSelectionVideo = 4;
  picker.maximumNumberOfSelectionPhoto = 3;
```

#### both images and videos
``` objective-c
  UzysAssetsPickerController *picker = [[UzysAssetsPickerController alloc] init];
  picker.delegate = self;
  picker.maximumNumberOfSelectionMedia = 5;
```
## Contact
 - [Uzys.net](http://uzys.net)
 - This Library was designed by [minjee Hahm](http://www.linkedin.com/pub/minjee-hahm/63/73/5a)


## License
 - See [LICENSE](https://github.com/uzysjung/UzysAssetsPickerController/blob/master/LICENSE).
