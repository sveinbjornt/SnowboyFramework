# Snowboy iOS Framework

This repository contains the [Snowboy](https://github.com/seasalt-ai/snowboy)
hotword detection engine precompiled and packaged as an iOS framework.

The framework includes a Mach-O universal binary with 5 architectures:
i386, x86_64 arm_v7, arm_v7s and arm64.

## Setup

* Add Snowboy.framework to your target under General in Xcode.
* Also add Accelerate.framework, which is required by Snowboy
* Add common.res and a Snowboy model (either .pmdl or .umdl) to your application bundle

## Usage


### Create Snowboy detector C++ object instance

**Please note**: Any Objective-C source files using Snowboy must have the suffix `.mm`
to make sure they are compiled as Objective-C++.


```objc
#import <Snowboy/Snowboy.h>

...

NSString *commonPath = [[NSBundle mainBundle] pathForResource:@"common" ofType:@"res"];
NSString *modelPath = [[NSBundle mainBundle] pathForResource:@"modelname" ofType:@"pmdl"];

_snowboyDetect = new snowboy::SnowboyDetect(std::string([commonPath UTF8String]),
                                            std::string([modelPath UTF8String]));
_snowboyDetect->SetSensitivity(0.5);
_snowboyDetect->SetAudioGain(1.0);
_snowboyDetect->ApplyFrontend(FALSE);
```

### Detect hotword in audio data

```objc
// Set up audio recording, one way or another, get audio data etc.
...

// Run detector on 16-bit audio samples
const int16_t *bytes = (int16_t *)pointer_to_audio_data;
const int len = (int)[data length]/2;
int result = _snowboyDetect->RunDetection((const int16_t *)bytes, len);
if (result > 0) {
    DLog(@"Snowboy: Hotword detected");
    // Do something
}
```


## License

Snowboy is Copyright (C) 2016-2020 KITT.AI

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this
file except in compliance with the License. You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under
the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF
ANY KIND, either express or implied. See the License for the specific language
governing permissions and limitations under the License.

