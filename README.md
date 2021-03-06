# LabelMe With PointClouds!
Tool for labelling and editing pointcloud data. Built to work with PCL!

Labelling pointclouds is now as simple as opening up PCL viewer and pressing some buttons.

Annotate, delete, and extract points in a cloud by drawing polygons (to form a mask around the desired points)

<img src="samples/living_room.png" height="300">
<img src="samples/living_room_colored.png" height="300">

## About
This tool is comprised of 2 components:
- __PCL Viewer__
- __OpenCV pop-up window__

PCL viewer is used to visualize the pointcloud from arbitrary camera viewpoints.  
Each time an operation on the pointcloud is about to be performed, a window (OpenCV window) will pop up and display the 2D-projected image generated from the current camera viewpoint.  
An operation is usually conducted via drawing polygons on the projected image of the pointcloud. A polygon generates a mask on the image, and the mask is then mapped back onto the 3D points in the pointcloud.  
In the OpenCV window, use `left-click` to draw polygon points, `backspace` to undo the previous polygon point, `c` to clear all polygon points, and `q` to exit the window.

### PCL Viewer
___COMMANDS___
- OpenCV Pop-up window: `a`
- Print current viewer pose: `p`
- Undo: `Ctrl + z`
- Redo: `Ctrl + y`
- Save: `Ctrl + s`  (saves a pcd_file containing the final pointcloud and a json_file containing the annotation colors, respective point indices (relative to original cloud) and point colors. See Usage for setting out_pcd_file and out_json_file argument)
- Quit: `Esc`  (quits the program)

### OpenCV pop-up window  
___COMMANDS___
- Draw a polygon point: `Left-click`
- Undo previous polygon point: `backspace`
- Clear: `c` (clears all existing polygon points)
- Perform action: `ENTER`
- Quit: `q`  (closes the pop-up window (but not the PCL viewer))

___ACTIONS___
- __Annotate__: annotate points in a polygon - currently assigns a random, unique annotation color to the points
- __Ctrl Annotate__:  same as above, but does not override existing annotations inside the polygon
- __Merge__: annotate points in a polygon - this time the annotation color is defined by a selected color, obtained from 'Annotate' action
- __Ctrl Merge__:  same as above, but does not override existing annotations inside the polygon
- __Undo-Annotation__:  converts points in the polygon back to their original colors
- __Delete__:   removes all points in the polygon
- __Extract__:  extracts all points in the polygon -> opposite of 'delete'

## Usage
`./labelme_3d pcd_file [-s out_pcd_file] [-oj out_json_file] [-ij in_json_file]`  
- Default out_pcd_file path: ./labelme_3d_out.pcd
- Default out_json_file path: ./labelme_3d_out.json
- Default in_json_file path: NONE  (used for continuing annotations! pass in a previous output json file)

## Reading output json file
Read the output json file and display the annotated pointcloud (must set the original pcd file, since the json file references the original point cloud). The file will also show the projected pointcloud onto a 2D image, though for this you'll need to manually set the 4x4 viewer pose in read_labelme_data.cpp  
`./read_labelme_data original_pcd_file out_json_file`  


## Requirements
- PCL
- OpenCV

## How to build
mkdir build  
cd build  
cmake ..  
make -j4  

