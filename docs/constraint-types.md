---
title: Constraint types
nav_order: 5
---

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Individual Part Constraints

## FIXED_ORIENTATION

Specifies an exact orientation for a part using a 3x3 rotation matrix.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "FIXED_ORIENTATION",
  "value": {
    "rotation": [
      1, 0, 0, 
      0, 1, 0, 
      0, 0, 1
    ]
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| rotation | array[9] | 3x3 rotation matrix in row-major order |

### Usage Notes
- The rotation matrix must be orthogonal
- Used when parts must maintain a specific orientation
- Common in parts with critical surfaces or features

### Example Use Cases
- Precision machined surfaces that must face upward
- Text or logos that must face a specific direction
- Parts with critical tolerances in specific orientations

---

## RANGE_ORIENTATION

Defines an allowed range of rotations around a specific axis, enabling controlled part orientation flexibility.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "RANGE_ORIENTATION",
  "value": {
    "axis": [0, 0, 1],
    "range": {
      "min": -45,
      "max": 45
    },
    "units": "DEG"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| axis | array[3] | Vector defining rotation axis [x, y, z] |
| range.min | number | Minimum allowed rotation angle (-180 to 180) |
| range.max | number | Maximum allowed rotation angle (-180 to 180) |
| units | string | Must be "DEG" or "RAD" |

### Usage Notes
- Axis vector must be normalized
- Min/max angles define the allowed rotation range around the axis
- Multiple RANGE_ORIENTATION constraints can be combined
- Range bounds are inclusive

### Example Use Cases
- Parts that can rotate within specific limits
- Cylindrical features with orientation flexibility
- Aesthetic components with limited variation allowed
- Support optimization with controlled freedom

---

## FORBIDDEN_ORIENTATION

Defines directions that cannot face downward, useful for avoiding support structures or maintaining surface quality.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "FORBIDDEN_ORIENTATION",
  "value": {
    "normals": [
      [0, 0, 1], 
      [0, 1, 0]
      ],
    "tolerance": 10,
    "units": "DEG"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| normals | array[array[3]] | Array of normal vectors that cannot point downward |
| tolerance | number | Allowed deviation in degrees (0-180) |
| units | string | Must be "DEG" or "RAD" |

### Usage Notes
- Normal vectors must be unit vectors
- Multiple normals can be specified
- Tolerance allows for some flexibility in orientation

### Example Use Cases
- Avoiding support structures on finished surfaces
- Protecting critical features from support removal
- Maintaining surface quality on visible faces

---

## FIXED_LOCATION

Sets an exact position for a part in the build volume.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "FIXED_LOCATION",
  "value": {
    "x": 0,
    "y": 0,
    "z": 0,
    "units": "MM"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| x | number | X-coordinate in build volume |
| y | number | Y-coordinate in build volume |
| z | number | Z-coordinate in build volume |
| units | string | Must be "MM", "CM", "IN" or "FT" |

### Usage Notes
- Coordinates are relative to build volume origin
- Used when exact positioning is critical
- Takes precedence over optimization strategies

### Example Use Cases
- Parts that must align with fixtures
- Multi-part assemblies with precise positioning
- Reference parts for calibration

---

## RANGE_LOCATION

Defines allowable position bounds for a part.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "RANGE_LOCATION",
  "value": {
    "bounds": {
      "x": {"min": 0, "max": 100},
      "y": {"min": 0, "max": 100},
      "z": {"min": 0, "max": 50}
    },
    "units": "MM"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| bounds.x | object | Min/max X-coordinates |
| bounds.y | object | Min/max Y-coordinates |
| bounds.z | object | Min/max Z-coordinates |
| units | string | Must be "MM", "CM", "IN" or "FT" |

### Usage Notes
- All coordinates are optional
- Allows flexible positioning within bounds
- Compatible with optimization strategies

### Example Use Cases
- Parts that must stay within specific build areas
- Height-restricted builds
- Thermal zone restrictions

---

# GD&T Tolerance Constraints

Added in version 1.1.0.

Tolerance constraints carry geometric dimensioning and tolerancing (GD&T) requirements for a feature of a part. They do not restrict where or how a part is placed. CAM software can use them to choose an orientation that keeps the toleranced features accurate, or to plan inspection.

All points, normals and directions use the coordinate frame of the model file, before the `referenceTransform` of the manifest entry is applied. All lengths use the `units` of the constraint value, which must be the same as the units of the manifest entry. A part can have many tolerance constraints, of one type or of both types.

## LINEAR_TOLERANCE

Specifies the allowed distance between 2 feature planes of a part, for example the thickness of a flange or the width of a slot.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "LINEAR_TOLERANCE",
  "value": {
    "name": "Flange thickness",
    "planes": [
      { "point": [0, 0, 0], "normal": [0, 0, -1] },
      { "point": [0, 0, 12.5], "normal": [0, 0, 1] }
    ],
    "nominal": 12.5,
    "upperDeviation": 0.1,
    "lowerDeviation": -0.05,
    "units": "MM"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| name | string | Optional label, for example the drawing callout |
| planes[].point | array[3] | A point on the plane |
| planes[].normal | array[3] | Unit normal of the plane, pointing out of the material |
| nominal | number | Nominal distance between the planes (0 or more) |
| upperDeviation | number | Signed upper deviation. The largest allowed distance is `nominal + upperDeviation` |
| lowerDeviation | number | Signed lower deviation. The smallest allowed distance is `nominal + lowerDeviation` |
| units | string | Must be "MM", "CM", "IN" or "FT" |

### Usage Notes
- `planes` has exactly 2 entries: plane A and plane B
- The distance is measured from plane A to plane B, along the normal of plane A
- The 2 planes are expected to be parallel
- `lowerDeviation` must not be greater than `upperDeviation`

### Example Use Cases
- Wall, flange or rib thickness
- Slot and step widths
- Distances between mating faces

---

## CIRCULARITY_TOLERANCE

Specifies a circularity (roundness) tolerance for a cylindrical feature, for example a bore or a shaft.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "CIRCULARITY_TOLERANCE",
  "value": {
    "name": "Bearing bore",
    "axis": {
      "point": [10, 20, 5],
      "direction": [0, 0, 1]
    },
    "radius": 11,
    "length": 10,
    "tolerance": 0.02,
    "units": "MM"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| name | string | Optional label, for example the drawing callout |
| axis.point | array[3] | A point on the axis. Where `length` is given, the center of the feature |
| axis.direction | array[3] | Unit direction of the axis |
| radius | number | Nominal radius of the feature (more than 0) |
| length | number | Optional length of the feature along the axis, centered on `axis.point` |
| tolerance | number | Width of the tolerance zone (more than 0) |
| units | string | Must be "MM", "CM", "IN" or "FT" |

### Usage Notes
- The tolerance zone is the radial space between 2 concentric circles, in each cross-section perpendicular to the axis
- The axis and the radius identify the feature. They do not set a size tolerance
- The direction vector must be a unit vector

### Example Use Cases
- Bores for bearings and bushings
- Shafts and pins
- Round sealing surfaces
