---
title: File structure
nav_order: 1
---

# File Structure

A `.cams` file is a ZIP archive containing:

```
archive.cams/
  ├── models/           # Directory containing all CAD files
  │   ├── part1.stl
  │   ├── part2.step
  │   └── part3.obj
  └── .cams.json       # Specification file

```

## Supported CAD Formats

The following CAD file formats are supported in the models directory:

- STEP (.step, .stp)
- STL ASCII (.stl)
- STL Binary (.stl)
- OBJ (.obj)
- IGES (.igs, .iges)

## Model Orientation Standard

A standard orientation is needed in order to facilitate effective communication between software packages. For the CAMS format, the model orientation will be according to their Oriented Bounding Box (OBB):

1. The model must be centered at origin (0,0,0)
2. The oriented bounding box must be calculated
3. The longest dimension must align with the Z axis
4. The second longest dimension must align with the Y axis
5. The shortest dimension must align with the X axis

This standard ensures consistent orientation across all implementations. 

Not all CAD files are oriented into this form by default. Rather than mutating the CAD files before creating the CAMS file, the referenceTransform field in the manifest indicates the transformation required to achieve this standard orientation from the model's current orientation.