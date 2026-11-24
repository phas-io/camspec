---
title: Implementation notes
nav_order: 3
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
4. Verify all normal and direction vectors are unit vectors
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
6. Keep every LINEAR_TOLERANCE and CIRCULARITY_TOLERANCE constraint with its part. These constraints do not restrict placement

## Extensibility

- Future versions will maintain backward compatibility
- New constraint types may be added in future versions

## Versions

- 1.1.0: adds the GD&T tolerance constraints LINEAR_TOLERANCE and CIRCULARITY_TOLERANCE. A valid 1.0.0 file is also a valid 1.1.0 file
- 1.0.0: first version