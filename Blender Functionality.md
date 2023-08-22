# YSCE Blender Functionality Requirements
This file documents the requirements for YSCE Blender Scripts.

# Coordinate System
YSFlight uses the following coordinate system:
- +X as right
- +Y as up
- +Z as forward

In Vincent's Blender YSFS Scripts, the coordinate system is:
- +X as forward
- +Y as right
- +Z as up

Blender 3.3's coordinate system is:
- +X as right
- +Y as forward
- +Z as up 

None of these align, but of the three, Vincent's coordinate system is the closest to aircraft design standards. Re-using this also simplifies the code base a little since we can use more of his code as a reference. The added benifit is that tutorials already made for Blender 2.49 can have more carryover to Blender 3.3. However the difference in coordinate systems is just a coordinate transformation matrix when translating between YSF and Blender, so a user-defined preference, setting, or selection might be an option to explore.

# SRF Import / Export

## Export
This function should be called from the standard Blender Export Menu under **File>Export**.

- Shall export the selected Blender Mesh Object to an [SRF File](https://ysflightsim.fandom.com/wiki/SRF_Files) format.
  - If the selected mesh object...
    - Has a scaling factor applied, the scaling factor should be applied to the exported Mesh
    - Has a rotation applied, the rotation should be applied to the exported mesh
    - Has an object center translation, the object center shall be re-caluclated to be the model origin (0, 0, 0) in the exported mesh
    - Has a parent object which rotates, scales, or translates the mesh object, the transformation shall be applied to the exported mesh.
  - Any face property (such as transparencies, brightness, double-sided, light source, etc) shall be defined in appropriate places in the generated SRF file. 
  - Vertex Smooth shading shall be detected and noted in the vertex definitions.
- If multiple mesh objects are selected, the users should be warned and nothing happen.
- If no mesh objects are selected, the user should be warned and nothing happen.
  - If non-mesh objects are selected but no mesh objects are selected, this should be specified in the warning ot the user.

## Import 

This function should be called form the standard Blender Import Menu under **File>Import**

- Shall import a selected .srf file formatted in the [YSFlight SRF Format](https://ysflightsim.fandom.com/wiki/SRF_Files) and...
  - Properly define the vertices and faces of the mesh
  - Properly assign face properties such as
    - Color
    - Bright
    - Normals
    - Double-sided
    - Transparency
    - Light Sprites
  - Properly assign vertex shading


# DNM Import / Export

## Export

This function should be called from the standard Blender Export Menu under **File>Export**.

- This function shall...
  - Convert all mesh objects into SRF format and insert into the .DNM file.
    - All Requirements for generating an SRF File will be applicable to this process.
  - For all mesh objects with duplicate geometries and face properties, they shall be defined by a single internal SRF in the DNM file.
  - Define all Objects in the DNM with appropriate positions, rotations, and parent-child relationships.
  


# Import

This function should be called form the standard Blender Import Menu under **File>Import**

- This function shall...
  - Read all the internally defined SRFs in a DNM, OR the externally defined and linked SRF files in the DNM and create mesh objects in the Blender World
    - All requirements for importing an SRF file will be applicable to this process.
  - For all objects defined in the DNM file:
    - Create mesh objects, using linked duplicates for identical SRF meshes for all objects in the DNM.
    - Apply parent-child relationships to all objects.
    - Apply CLA properties to all objects
    - Make an empty the parent of every object and then link that empty to the DNM-defined parent.


# GUIs
In addition to import/export scripts, we will need GUIs to provide the user inputs for various purposes.


## YS Face Mode
A GUI shall be developed to provide the following functionality:
- Apply and edit the Bright status of a mesh face
- Apply and edit the transparency value of a mesh face
- Apply and edit the light sprite of a mesh face
- Apply and edit the vertex paint color of a mesh face (Optional)

## DNM View
A GUI shall be developed to provide the following functionality
- Select a CLA from a drop down list and apply the selected CLA number to a mesh object.
- Edit the CLA number of a mesh object
- When a mesh object is selected, display the CLA number applied to it.

## Face Color Replacement
A GUI shall be developed to allow for color replacement through a script
- Two Color selection GUI elements, one for the old color and one for the new color
- A button to run the script that will perform the color replacement


# Scripts
These scripts will be useful for simplifying modding

## Face Color Replacement
Tied to the Face Color replacement GUI Button, this script will iterate over all selected mesh objects and for every face with the old color defined in the GUI, will turn the face color into the new color from the GUI.