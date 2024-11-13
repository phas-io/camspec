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
    "units": "deg"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| axis | array[3] | Vector defining rotation axis [x, y, z] |
| range.min | number | Minimum allowed rotation angle (-180 to 180) |
| range.max | number | Maximum allowed rotation angle (-180 to 180) |
| units | string | Must be "deg" or "rad" |

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
    "units": "deg"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| normals | array[array[3]] | Array of normal vectors that cannot point downward |
| tolerance | number | Allowed deviation in degrees (0-180) |
| units | string | Must be "deg" or "rad" |

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
    "units": "mm"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| x | number | X-coordinate in build volume |
| y | number | Y-coordinate in build volume |
| z | number | Z-coordinate in build volume |
| units | string | Must be "mm", "cm", "in" or "ft" |

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
    "units": "mm"
  }
}
```

### Properties

| Property | Type | Description |
|:---------|:-----|:------------|
| bounds.x | object | Min/max X-coordinates |
| bounds.y | object | Min/max Y-coordinates |
| bounds.z | object | Min/max Z-coordinates |
| units | string | Must be "mm", "cm", "in" or "ft" |

### Usage Notes
- All coordinates are optional
- Allows flexible positioning within bounds
- Compatible with optimization strategies

### Example Use Cases
- Parts that must stay within specific build areas
- Height-restricted builds
- Thermal zone restrictions