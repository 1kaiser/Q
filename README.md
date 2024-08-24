# Q




<table>
<thead>
<tr>
<td>

**adding subplots nomenclature automatically**

add import
```
import string
```
```
# Define a function to add custom subplot numbering with increasing alphabet letters
def add_subplot_number(ax, font_size=16):
    global subplot_count
    subplot_count += 1
    ax.annotate(f'({string.ascii_lowercase[subplot_count - 1]})', xy=(0, 1), xycoords='axes fraction', fontsize=font_size, xytext=(10, -10), textcoords='offset points', ha='left', va='top')

```
function use in main function
```
def fun():
    global subplot_count
    subplot_count = 0
    fig = plt.figure(figsize=figsize, dpi=dpi)
    ax4 = fig.add_axes([width_x*0.5, width_x, parameter_plot_height_ratio])
    add_subplot_number(ax4)

```
</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr>
<td>
    
Conda on Google Colab [CondaColab](https://github.com/conda-incubator/condacolab)

```python
!pip install -q condacolab
import condacolab
condacolab.install()
```

After the kernel restart, you can optionally add a new cell to check that everything is in place:

```python
import condacolab
condacolab.check()
```
Once the installation is done, you can use `conda` and/or `mamba` to install the needed packages:

```bash
!conda install {package_name}

# or, faster:
!mamba install {package_name}
```
</td>
</tr>
</tbody>
</table>



<table>
<thead>
<tr>
<td>

**creating credentials and saving the credentials as .netrc file in [google colab](https://colab.new/)**
```
!echo $HOME
```

```
%%writefile ~/.netrc
machine urs.earthdata.nasa.gov
    login kroy0001
    password /#j%kWrPA,8.HRe
```

```
!chmod 600 ~/.netrc
```
additional downloading code with links in `url.txt`

```
import time
from multiprocessing.pool import ThreadPool
from google.colab import output

def download_url(url):
    start_time = time.time()
    !curl -O -b ~/.urs_cookies -c ~/.urs_cookies -L -n {url}
    return time.time() - start_time

def download_parallel(urls):
    with ThreadPool(9) as pool:  # Using 9 threads (10 CPUs - 1)
        for download_time in pool.imap_unordered(download_url, urls):
            print('time (s):', download_time)
            output.clear()

# Read URLs from file and start parallel download
with open("/content/url.txt", 'r') as file:
    download_parallel(file)
```
</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr>
<td>

[gdal installation google colab](https://gdal.org/index.html)
```
!sudo apt-get install gdal-bin
```
for the given input raster, input vector, output path
```
input_raster = '/content/2016_soil_m/SPL4SMGP.006_Geophysical_Data_sm_surface_0_doy2016001_aid0001.tif'
temp_raster = '/content/resampled_raster.tif'
shapefile = '/content/boundaryOutputx.shp'
output_raster = '/content/output.tif'

```
to create a new file with changed reslution
```
!gdalwarp \
  -tr 0.001 0.001 \
  -overwrite \
  {input_raster} \
  {temp_raster}
```

to cut to the area in the vector file as mask
```
!gdalwarp \
  -cutline {shapefile} \
  -dstnodata 0 \
  -overwrite \
  {temp_raster} \
  {output_raster}

```
</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr>
<td>

google sheets Regular expression
```
=CONCATENATE(REGEXEXTRACT(B2,"[0-9]*.[0-9]{6}"),",",REGEXEXTRACT(C2,"[0-9]*.[0-9]{6}")) 

or

=CONCATENATE(REGEXEXTRACT(TEXT(A2,"00.000000"),"[0-9]*.[0-9]{5}"),":",REGEXEXTRACT(TEXT(B2,"00.000000"),"[0-9]*.[0-9]{5}")) 

>>for 31.4754428611 >> 31.475442 << 6 digits after decimal
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

Connect google drive
```
from google.colab import drive
drive.mount('/content/drive')
```

</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

any image in array form visualization
```
from google.colab.patches import cv2_imshow
import numpy as np     
predicted_image = np.array(v7[:,:,2],  dtype=np.float32) # This would be your image array
cv2img = cv2_imshow(predicted_image) # This work the same as passing an image
```
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<td>

Jax Array to image visualization
```
import cv2
from google.colab.patches import cv2_imshow
import numpy as onp
image_array
img = onp.array(image_array)
cv2_imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
```
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<td>

quick directory creation
```
INPUT_FILES = 'input1'
OUTPUT_FILES = 'output1'
!mkdir -p {INPUT_FILES}
!mkdir -p {OUTPUT_FILES}
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

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
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

ffmpeg video to images with a framerate
```
!ffmpeg -y -hwaccel cuvid \
  -i '/content/test.mp4' \
  -r 24 {INPUT_FILES}/out_%09d.png
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

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
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

Clean display output Colab
```
from google.colab import output
output.clear()

```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

Clean display output Kaggle
```
from IPython.display import clear_output
clear_output()
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

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

</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

Zip a folder or file
```
%cd {total_files}
%cd ..
!zip -r folder.zip {total_files}
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

Unzip a folder or file
```
!unzip /content/archive.zip #unzipping the flower images from archive..
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

Kaggle file download
```
%cd /kaggle/working
from IPython.display import FileLink
FileLink(r'ckpts/checkpoint_50/checkpoint')
```
</td>
</tr>
</tbody>
</table>







<table>
<thead>
<tr>
<td>

[Direct Google Drive upload](https://github.com/iterative/PyDrive2/blob/main/examples/Upload-and-autoconvert-to-Google-Drive-Format-Example/upload.py)
```
!python -m pip install PyDrive2
```
```
%%writefile settings.yaml

client_config_backend: settings
client_config:
  client_id: 49621581740-a3fm39vtadtnmhrfmumoijj6jk9i4ij0.apps.googleusercontent.com
  client_secret: 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

save_credentials: True
save_credentials_backend: file
save_credentials_file: credentials.json

get_refresh_token: True

oauth_scope:
  - https://www.googleapis.com/auth/drive.file
  - https://www.googleapis.com/auth/drive.install
```

```
%%writefile upload.py


# Original author: Evren Yurtesen - https://github.com/yurtesen/

"""
Uploads a file to a specific folder in Google Drive and converts it to a
Google Doc/Sheet/etc. if possible.

usage: upload.py  
example usage: upload.py 0B5XXXXY9KddXXXXXXXA2c3ZXXXX /path/to/my/file
"""
import sys
from os import path
from pydrive2.auth import GoogleAuth
from pydrive2.drive import GoogleDrive
from pydrive2.settings import LoadSettingsFile

gauth = GoogleAuth()
gauth.CommandLineAuth()
drive = GoogleDrive(gauth)

# If provided arguments incorrect, print usage instructions and exit.
if len(sys.argv) < 2:
    print("usage: upload.py  ")
    exit(1)  # Exit program as incorrect parameters provided.

parentId = sys.argv[1]
myFilePath = sys.argv[2]
myFileName = path.basename(sys.argv[2])

# Check if file name already exists in folder.
file_list = drive.ListFile(
    {
        "q": '"{}" in parents and title="{}" and trashed=false'.format(
            parentId, myFileName
        )
    }
).GetList()

# If file is found, update it, otherwise create new file.
if len(file_list) == 1:
    myFile = file_list[0]
else:
    myFile = drive.CreateFile(
        {"parents": [{"kind": "drive#fileLink", "id": parentId}]}
    )

# Upload new file content.
myFile.SetContentFile(myFilePath)
myFile["title"] = myFileName
# The `convert` flag indicates to Google Drive whether to convert the
# uploaded file into a Google Drive native format, i.e. Google Sheet for
# CSV or Google Document for DOCX.
print("uploading >>>")
myFile.Upload()
```
```
!python upload.py '1adeXlEvClGh85JYdP1aCPmdvDbbzP7YL' a.zip
```
</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr>
<td>

transfer file/folder from a pc to remote pc in Linux `sshpass` is required to be installed 


On Ubuntu or Debian: `sudo apt-get install sshpass` or `sudo apt install sshpass`


On macOS with Homebrew: `brew install hudochenkov/sshpass/sshpass`

```
sshpass -p '{password}' scp -rp ./Documents/{folder_name_old}/ {RemoteUsername}@{RemoteIP_addr}:/home/{RemoteUsername}/{folder_name_new}

```
```
sshpass -p '{password}' rsync -avz ./Documents/{folder_name_old}/ {RemoteUsername}@{RemoteIP_addr}:/home/{RemoteUsername}/{folder_name_new}
```
</td>
</tr>
</tbody>
</table>

    
<table>
<thead>
<tr>
<td>

OBJ to GLB 3D asset conversion including support for React
**install latest node version 2023 october**
```
!npm install -g n 
!n 20.8.1
```
`!n latest ` <<will install the latest by default but here taken 20.8.1

copy from `/drive` to `/contents` folder
```
!cp -r /content/drive/MyDrive/3D/20220714_132152.obj /content
```
install [`obj2gltf`](https://github.com/CesiumGS/obj2gltf)
```
!npm install -g obj2gltf
```
```
input = '/content/20220714_132152.obj/20220714_132152.obj'
output = '/content/20220714_132152.obj/20220714_132152.glb'

!npx obj2gltf -i {input} -o {output} # >> !obj2gltf -i {input}.obj -o {output}.glb

```
install [`gltfjsx`](https://github.com/pmndrs/gltfjsx) 
```
!npm i gltfjsx@6.2.13
```
the `--transform, -T`     Transform the asset for the web (draco, prune, resize)
```
!npx gltfjsx {output} --transform
```
```
!pip install trimesh
import numpy as np
import trimesh
mesh = trimesh.load_mesh('/content/20220714_132152-transformed.glb')
mesh.show()
```
</td>
</tr>
</tbody>
</table>



<table>
<thead>
<tr>
<td>

csv file creation
```

csv_dir = "/content/csvs"
prefix = ""
end = ["common_part_pf_filename"]
ex_b = prefix+end[0]

import os
csvs_list = [os.path.join(csv_dir, f) for f in os.listdir(csv_dir) if f.__contains__(ex_b)]
csvs_list.sort(reverse=True)   
print(csvs_list)

# importing pandas
import pandas as pd  
# merging two csv files
df = pd.concat(map(pd.read_csv, csvs_list), ignore_index=True)
d = ['name1', 'name2', 'name3'] 
#if there are 3 columns else add more as requirement
df.columns = d
print(df)
df.to_csv("final_filename.csv")
```
</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr>
<td>
    
points inside a region of a dataset


```
import geopandas as gpd
from shapely.geometry import Point

shapefile_path = "../data/shapefile/india_states/india_states.shp"  # Replace with your shapefile path
gdf = gpd.read_file(shapefile_path)
polygon = gdf.geometry[0]

## Extract lat/lon from the dataset
latitudes = dataset['lat'].values
longitudes = dataset['lon'].values

points_inside = [(lat, lon) for lat in latitudes for lon in longitudes if polygon.contains(Point(lon, lat))]
points_inside
```
</td>
</tr>
</tbody>
</table>
