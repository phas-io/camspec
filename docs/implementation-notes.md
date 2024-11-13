---
title: Implementation notes
nav_order: 2
---

# Implementation notes

### Coordinate System

- Right-handed coordinate system
- Units in millimeters unless otherwise specified
- Angles in degrees unless otherwise specified

### Priority Levels

- 0-24: Low priority (easily discarded if space constrained)
- 25-74: Normal priority
- 75-99: High priority
- 100: Critical (must be included)

## Validation Requirements

Implementations MUST:

1. Validate the JSON schema
2. Verify all referenced files exist in models directory
3. Validate all transformation matrices are valid
4. Verify all normal vectors are unit vectors
5. Check for UUID uniqueness in manifest
6. Validate all units are consistent
7. Verify all referenced manifestIds exist

## Processing Requirements

Implementations MUST:

1. Apply referenceTransform before applying any constraints
2. Process constraints in order of priority
3. Respect all FORBIDDEN_ORIENTATION constraints
4. Maintain minimum part distances
5. Apply optimization strategy if possible

## Extensibility

- Future versions will maintain backward compatibility
- New constraint types may be added in future versions