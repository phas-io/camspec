---
title: .cams.json schema
nav_order: 4
---

# .cams.json schema

## Core Structure

A CAMSPEC document consists of four main sections:

```json
{
  "version": "1.0.0",
  "manifest": [...],
  "constraints": [...],
  "globalConstraints": [...],
  "optimization": {...}
}
```

## Version

The `version` field follows semantic versioning (X.Y.Z format):
```json
{
  "version": "1.0.0"
}
```

## Manifest

The `manifest` section lists all CAD models included in the specification. Each entry describes a single model file:

```json
{
  "manifest": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "file": "part.stl",
      "format": "STL_ASCII",
      "name": "Example Part",
      "priority": 75,
      "units": "mm",
      "referenceTransform": {
        "rotation": [1, 0, 0, 0, 1, 0, 0, 0, 1],
        "translation": [0, 0, 0]
      }
    }
  ]
}
```

### Manifest Properties

| Property | Required | Description |
|----------|----------|-------------|
| id | Yes | UUID for unique identification |
| file | Yes | Path to model file relative to models directory |
| format | Yes | One of: STEP, STL_ASCII, STL_BINARY, OBJ, BREP, IGES |
| name | No | Human-readable identifier |
| priority | Yes | Importance (0-100, higher = more important) |
| units | Yes | Model units (mm, in, m, cm) |
| referenceTransform | Yes | Transform to standard orientation |

## Constraints

The `constraints` array defines specific requirements for individual models, for example:

```json
{
  "constraints": [
    {
      "manifestId": "550e8400-e29b-41d4-a716-446655440000",
      "type": "FIXED_ORIENTATION",
      "value": {
        "rotation": [1, 0, 0, 0, 1, 0, 0, 0, 1]
      }
    }
  ]
}
```

## Global Constraints

The `globalConstraints` array defines requirements affecting all models, for example:

### MIN_PART_DISTANCE
Minimum spacing between any two parts:
```json
{
  "type": "MIN_PART_DISTANCE",
  "value": {
    "distance": 2.0,
    "units": "mm"
  }
}
```

### VOLUME_BASED_SPACING
Different spacing requirements based on part size:
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

## Optimization

The `optimization` object defines the primary optimization strategy, for example:

```json
{
  "optimization": {
    "strategy": "MINIMIZE_HEIGHT",
    "weight": 1.0
  }
}
```