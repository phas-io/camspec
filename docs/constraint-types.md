---
title: Constraint types
nav_order: 5
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

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
    "rotation": [1, 0, 0, 0, 1, 0, 0, 0, 1]
  }
}
```

### Properties
| Property | Type | Description |
|----------|------|-------------|
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

## FORBIDDEN_ORIENTATION

Defines directions that cannot face downward, useful for avoiding support structures or maintaining surface quality.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "FORBIDDEN_ORIENTATION",
  "value": {
    "normals": [[0, 0, 1], [0, 1, 0]],
    "tolerance": 10,
    "units": "deg"
  }
}
```

### Properties
| Property | Type | Description |
|----------|------|-------------|
| normals | array[array[3]] | Array of normal vectors that cannot point downward |
| tolerance | number | Allowed deviation in degrees (0-180) |
| units | string | Must be "deg" |

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
|----------|------|-------------|
| x | number | X-coordinate in build volume |
| y | number | Y-coordinate in build volume |
| z | number | Z-coordinate in build volume |
| units | string | Must be "mm" |

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
|----------|------|-------------|
| bounds.x | object | Min/max X-coordinates |
| bounds.y | object | Min/max Y-coordinates |
| bounds.z | object | Min/max Z-coordinates |
| units | string | Must be "mm" |

### Usage Notes
- All coordinates are optional
- Allows flexible positioning within bounds
- Compatible with optimization strategies

### Example Use Cases
- Parts that must stay within specific build areas
- Height-restricted builds
- Thermal zone restrictions

---

## NESTING_PREFERENCE

Indicates preferred positioning strategy for a part.

### Schema
```json
{
  "manifestId": "550e8400-e29b-41d4-a716-446655440000",
  "type": "NESTING_PREFERENCE",
  "value": {
    "preferredLocation": "center",
    "preferredHeight": "bottom"
  }
}
```

### Properties
| Property | Type | Description |
|----------|------|-------------|
| preferredLocation | string | One of: "edge", "center", "corner" |
| preferredHeight | string | One of: "bottom", "top", "middle" |

### Usage Notes
- Suggestions rather than strict requirements
- May be overridden by optimization strategies
- Used to influence part placement

### Example Use Cases
- Thermal considerations (edge vs center)
- Parts sensitive to recoater direction
- Build plate adhesion requirements