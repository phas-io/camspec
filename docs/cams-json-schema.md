---
title: .camspec.json schema
nav_order: 4
---

# .camspec.json schema

## Core Structure

A CAMSPEC document consists of four main sections:

```json
{
  "$schema": "https://camspec.org/schema/1.0.0/camspec.json",
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
      "quantity": 1,
      "units": "MM",
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
| quantity | Yes | The quantity of parts to produce |
| units | Yes | Model units (MM, CM, FT, IN) |
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

```json
{
  "globalConstraints": [
    {
      "type": "MIN_PART_DISTANCE",
      "value": {
        "distance": 2.0,
        "units": "MM"
      }
    }
  ]
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