The **main problem** is that the apriltag.h definition in `/usr/local/include/apriltag` is from the old MIT 2016-2019 fork

`/usr/local/include/apriltag/apriltag.h` is the old deprecated library that for some reason is being pointed to when building (amake)

`.../rock/install/include/apriltag/apriltag.h` or `.../rock/perception/apriltags/apriltag.h`
is the Apriltag Robotics library from UMich, which is the proper apriltag library that we should be pointing to in our workspace

change usr/local/include/apriltag just so it doesn't "exist" anymore (can always change it back):
`sudo mv /usr/local/include/apriltag /usr/local/include/apriltag.bak`
This also helps when you use "goto definition" that it no longer points to your `/usr/local/...`

Also the 'lib' that is pointed to when called by 'using_library' in our orogen file should be `apriltag` because in install/lib it is apriltag not apriltag(s) —`install/lib/apriltag`

In `.../rock/perception/orogen/apriltags/apriltags.orogen`
- change line 4
	- `using_library = 'apriltags`  to
- `using_library = 'apriltag'`

In `.../rock/perception/orogen/apriltags/tasks/CMakeLists.txt`
- I initially did a quick fix using:
```CMAKE
include_directories(path/to/rock/install/include)

include_directories(path/to/rock/install/include/apriltag)
```
But later generalized it for anyone who clones the repo:
```CMAKE
include_directories($ENV{AUTOPROJ_CURRENT_ROOT}/install/include)

include_directories($ENV{AUTOPROJ_CURRENT_ROOT}/install/include/apriltag)
```
### Task.hpp
`.../rock/perception/orogen/apriltags/tasks/Task.hpp`
From:
```CPP
#include "apriltags/apriltag.h"

#include "apriltags/common/image_u8.h"

#include "apriltags/tag36h11.h"

#include "apriltags/tag36h10.h"

//#include "apriltags/tag36artoolkit.h"

#include "apriltags/tag25h9.h"

//#include "apriltags/tag25h7.h"

#include "apriltags/tag16h5.h"

#include "apriltags/common/zarray.h"

#include "apriltags/common/getopt.h"
```
To:
```CPP
#include "apriltag/apriltag.h"

#include "common/image_u8.h"

#include "tag36h11.h"

#include "tag36h10.h"

#include "tag25h9.h"

#include "tag16h5.h"

#include "common/zarray.h"

#include "common/getopt.h"
```
Example:
![[Pasted image 20250626164506.png]]

In: `void getRbs` method
- comment out throw(cv::Exception) 
	- it is deprecated C++ feature
- change tag25h7 to 16h5 
	- tag25h7 file doesn't exist but 16h5 does
- comment out tag36artoolkit
	- from deprecated apriltags library 

### Task.cpp
throw(cv::Exception) deprecated syntax

Line 94-95
```CPP
    //td->refine_decode = conf.refine_decode;

    //td->refine_pose = conf.refine_pose;
```
- These properties doesn't exist in Apriltag Robotics


Line 256, apriltag properties access mismatch
![[Pasted image 20250626173824.png]]

comment out tag36artoolkit

### apriltagsTypes.hpp
apriltagtypes.hpp enum update to 25h7 to TAG16H5
![[Pasted image 20250626164433.png]]
