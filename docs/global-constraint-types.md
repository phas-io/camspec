---
title: Global Constraint types
nav_order: 5
---

# Global Constraints

## MIN_PART_DISTANCE

Defines the minimum required spacing between any two parts.

### Schema
```json
{
  "type": "MIN_PART_DISTANCE",
  "value": {
    "distance": 2.0,
    "units": "mm"
  }
}
```

### Properties
| Property | Type | Description |
|----------|------|-------------|
| distance | number | Minimum distance between parts (>0) |
| units | string | Must be "mm" |

### Usage Notes
- Applies to all parts regardless of size
- Measured from part boundaries
- Critical for post-processing access

### Example Use Cases
- Ensuring parts can be separated
- Allowing for thermal expansion
- Maintaining accessibility for support removal

---

## VOLUME_BASED_SPACING

Defines different spacing requirements based on part size.

### Schema
```json
{
  "type": "VOLUME_BASED_SPACING",
  "value": {
    "smallParts": {
      "maxVolume": 125000,
      "minSpacing": 2,
      "units": "mm"
    },
    "largeParts": {
      "minVolume": 1000000,
      "minSpacing": 5,
      "units": "mm"
    }
  }
}
```

### Properties
| Property | Type | Description |
|----------|------|-------------|
| smallParts.maxVolume | number | Maximum volume for small parts (mm³) |
| smallParts.minSpacing | number | Minimum spacing for small parts |
| largeParts.minVolume | number | Minimum volume for large parts (mm³) |
| largeParts.minSpacing | number | Minimum spacing for large parts |

### Usage Notes
- Parts between size ranges use linear interpolation
- Volumes are calculated after orientation
- Takes precedence over MIN_PART_DISTANCE

### Example Use Cases
- Thermal management for different part sizes
- Optimizing packing density
- Managing part removal complexity

---

## VERTICAL_DISTRIBUTION

Controls the distribution of parts across build height.

### Schema
```json
{
  "type": "VERTICAL_DISTRIBUTION",
  "value": {
    "maxPartsPerHeight": {
      "height": 50.0,
      "maxParts": 4
    },
    "units": "mm"
  }
}
```

### Properties
| Property | Type | Description |
|----------|------|-------------|
| height | number | Height interval for part counting |
| maxParts | integer | Maximum parts per interval |
| units | string | Must be "mm" |

### Usage Notes
- Height intervals start from build plate
- Parts counted in any interval they intersect
- Used to prevent dense clustering

### Example Use Cases
- Managing thermal distribution
- Controlling recoater load
- Optimizing build time vs. density