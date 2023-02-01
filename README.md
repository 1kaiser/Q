# Q

google sheets Regular expression
```
=CONCATENATE(REGEXEXTRACT(B2,"[0-9]*.[0-9]{6}"),",",REGEXEXTRACT(C2,"[0-9]*.[0-9]{6}")) >>for 31.4754428611 >> 31.475442 << 6 digits after decimal
```
Connect google drive
```
from google.colab import drive
drive.mount('/content/drive')
```

any image in array form visualization
```
from google.colab.patches import cv2_imshow
import numpy as np     
predicted_image = np.array(v7[:,:,2],  dtype=np.float32) # This would be your image array
cv2img = cv2_imshow(predicted_image) # This work the same as passing an image
```
Jax Array to image visualization
```
import cv2
from google.colab.patches import cv2_imshow
import numpy as onp
image_array
img = onp.array(image_array)
cv2_imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
```

quick directory creation
```
INPUT_FILES = 'input1'
OUTPUT_FILES = 'output1'
!mkdir -p {INPUT_FILES}
!mkdir -p {OUTPUT_FILES}
```

Delete files in INPUT directory by checking common names from OUTPUT directory
```
%%writefile goDeleteHolistic.js
'use strict';

const fs = require('fs');
let d=0;
let darray1 = fs.readdirSync({INPUT_FILES});
let darray2 = fs.readdirSync({OUTPUT_FILES});
for (let index = 0; index < darray1.length; index++) {
    console.log(index)
    if (darray2.includes(darray1[index])) {
        console.log(d++)
        
        fs.unlinkSync({INPUT_FILES}+darray1[index])
    }
}
     
####################################node execution
!node goDeleteHolistic.js
```

ffmpeg video to images with a framerate
```
!ffmpeg -y -hwaccel cuvid \
  -i '/content/test.mp4' \
  -r 24 {INPUT_FILES}/out_%09d.png
```

ffmpeg Frames to Video creation and merging two created sreams
```
#####################################create video 1
!ffmpeg \
-framerate 24 \
-pattern_type glob \
-i {INPUT_FILES}'/*.png' \
input0.mp4
#####################################create vodeo 2
!ffmpeg \
-framerate 24 \
-pattern_type glob \
-i {OUTPUT_FILES}'/*.png' \
input1.mp4
#####################################merge video 1 and video 2
!ffmpeg \
-i input0.mp4 -i input1.mp4 \
-filter_complex \
"[0:v][1:v]vstack=inputs=2" \
finalOutput.mp4
#####################################add sound of main video to final output video
!ffmpeg \
-i 'finalOutput.mp4' -i 'm.mp4' \
-c:v copy -c:a copy -map 0:v:0 -map 1:a:0 \
OUTPUT_FILE.mp4
```
Clean display output Colab
```
from google.colab import output
output.clear()

```
Clean display output Kaggle
```
from IPython.display import clear_output
clear_output()
```
create horizontal image array
```
import cv2
from google.colab.patches import cv2_imshow
import numpy as np 
def show_image(argu):
  L1 = argu[0]
  predicted_image = np.array(argu[0],  dtype=np.uint8).reshape(newsize) # This would be your image array
  a = predicted_image
  for i in range(0,argu.shape[0]):
    predicted_image = np.array(argu[i],  dtype=np.uint8).reshape(newsize) 
    a = cv2.hconcat([a, predicted_image])
  cv2_imshow(a)

show_image(metrics['logits'])
```
Zip a folder or file
```
%cd {total_files}
%cd ..
!zip -r folder.zip {total_files}
```

Unzip a folder or file
```
!unzip /content/archive.zip #unzipping the flower images from archive..
```
Kaggle file download
```
%cd /kaggle/working
from IPython.display import FileLink
FileLink(r'ckpts/checkpoint_50/checkpoint')
```
