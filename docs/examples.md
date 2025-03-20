---
title: Examples
nav_order: 8
---

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Example 1: Basic Multi-Part Print

```json
{
  "version": "0.1.0",
  "manifest": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "file": "critical_part.stl",
      "format": "STL_ASCII",
      "name": "Precision Surface Part",
      "priority": 100,
      "quantity": 5,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "file": "support_bracket.stl",
      "format": "STL_ASCII",
      "name": "Support Bracket",
      "priority": 50,
      "quantity": 1,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    }
  ],
  "constraints": [
    {
      "manifestId": "550e8400-e29b-41d4-a716-446655440000",
      "type": "FORBIDDEN_ORIENTATION",
      "value": {
        "normals": [[0, 0, 1]],
        "tolerance": 10,
        "units": "deg"
      }
    }
  ],
  "globalConstraints": [
    {
      "type": "MIN_PART_DISTANCE",
      "value": {
        "distance": 2.0,
        "units": "mm"
      }
    }
  ],
  "optimization": {
    "strategy": "MINIMIZE_HEIGHT",
    "weight": 1.0
  }
}

```

## Example 2: Mixed Priority Parts

```json
{
  "version": "0.1.0",
  "manifest": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440002",
      "file": "housing.step",
      "format": "STEP",
      "name": "Main Housing",
      "priority": 100,
      "quantity": 5,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440003",
      "file": "cover.step",
      "format": "STEP",
      "name": "Cover Plate",
      "priority": 100,
      "quantity": 5,
      "units": "in",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440004",
      "file": "spacer.stl",
      "format": "STL_ASCII",
      "name": "Optional Spacer",
      "priority": 25,
      "quantity": 5,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    }
  ],
  "constraints": [
    {
      "manifestId": "550e8400-e29b-41d4-a716-446655440002",
      "type": "FIXED_LOCATION",
      "value": {
        "x": 0,
        "y": 0,
        "z": 0,
        "units": "mm"
      }
    }
  ],
  "globalConstraints": [
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
  ],
  "optimization": {
    "strategy": "MAXIMIZE_DENSITY",
    "weight": 1.0
  }
}

```

## Example 3: Complex Production Build

```json
{
  "version": "0.1.0",
  "manifest": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440005",
      "file": "precision_gear.stl",
      "format": "STL_ASCII",
      "name": "Precision Gear 1",
      "priority": 100,
      "quantity": 1,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [10, 0, 0]
      }
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440006",
      "file": "precision_gear.stl",
      "format": "STL_ASCII",
      "name": "Precision Gear 2",
      "priority": 100,
      "quantity": 1,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [-10, 0, 0]
      }
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440007",
      "file": "housing_base.stl",
      "format": "STL_ASCII",
      "name": "Housing Base",
      "priority": 90,
      "quantity": 1,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440008",
      "file": "filler_block.stl",
      "format": "STL_ASCII",
      "name": "Filler Block",
      "priority": 10,
      "quantity": 1,
      "units": "mm",
      "referenceTransform": {
        "rotation": [
	        1, 0, 0, 
	        0, 1, 0, 
	        0, 0, 1
        ],
        "translation": [0, 0, 0]
      }
    }
  ],
  "constraints": [
    {
      "manifestId": "550e8400-e29b-41d4-a716-446655440005",
      "type": "FORBIDDEN_ORIENTATION",
      "value": {
        "normals": [
          [0, 0, 1], 
          [0, 1, 0]
          ],
        "tolerance": 5,
        "units": "deg"
      }
    },
    {
      "manifestId": "550e8400-e29b-41d4-a716-446655440006",
      "type": "FORBIDDEN_ORIENTATION",
      "value": {
        "normals": [
          [0, 0, 1], 
          [0, 1, 0]
          ],
        "tolerance": 5,
        "units": "deg"
      }
    }
  ],
  "globalConstraints": [
    {
      "type": "MIN_PART_DISTANCE",
      "value": {
        "distance": 3.0,
        "units": "mm"
      }
    }
  ],
  "optimization": {
    "strategy": "MINIMIZE_SUPPORT",
    "weight": 0.8
  }
}

```