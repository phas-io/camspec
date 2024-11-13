---
title: Optimization strategies
nav_order: 6
---

# Optimization Strategies

The `optimization` object defines how parts should be arranged when multiple valid solutions exist.

## Schema

```json
{
  "optimization": {
    "strategy": "MINIMIZE_HEIGHT",
    "weight": 1.0
  }
}
```

## Properties

| Property | Type | Description |
|----------|------|-------------|
| strategy | string | Primary optimization goal |
| weight | number | Strategy importance (0-1) |

## Available Strategies

### MINIMIZE_HEIGHT

Attempts to reduce the overall build height by prioritizing compact vertical arrangements.

#### Use Cases
- Reducing build time
- Minimizing material waste
- Optimizing machine utilization

#### Behavior
- Prefers wider, shorter arrangements
- May spread parts horizontally
- Considers part stacking where allowed

#### Example
```json
{
  "optimization": {
    "strategy": "MINIMIZE_HEIGHT",
    "weight": 0.8
  }
}
```

### MAXIMIZE_DENSITY

Attempts to pack parts as closely as possible while respecting spacing constraints.

#### Use Cases
- Maximizing build capacity
- Batch production
- High-volume manufacturing

#### Behavior
- Minimizes empty space between parts
- May increase build height
- Prioritizes efficient space utilization

#### Example
```json
{
  "optimization": {
    "strategy": "MAXIMIZE_DENSITY",
    "weight": 1.0
  }
}
```

### MINIMIZE_SUPPORT

Attempts to orient and position parts to minimize required support structures.

#### Use Cases
- Reducing post-processing time
- Improving surface quality
- Minimizing material waste

#### Behavior
- Prefers orientations with minimal overhangs
- May increase spacing for support accessibility
- Considers support removal access

#### Example
```json
{
  "optimization": {
    "strategy": "MINIMIZE_SUPPORT",
    "weight": 0.9
  }
}
```

## Weight Parameter

The `weight` parameter (0-1) determines how strongly to prioritize the chosen strategy:

- **1.0**: Maximum priority
  - Strictly follows optimization strategy
  - May sacrifice other considerations
  - Best theoretical optimization

- **0.5**: Balanced approach
  - Considers optimization alongside other factors
  - Balances multiple objectives
  - More flexible solutions

- **0.0**: Minimum priority
  - Minimal optimization influence
  - Focuses on constraint satisfaction
  - May result in suboptimal arrangements

## Implementation Notes

1. **Priority Order**
   - Part constraints take precedence over optimization
   - Global constraints must be satisfied
   - Optimization applies within remaining degrees of freedom

2. **Multiple Solutions**
   - When multiple valid solutions exist, optimization breaks ties
   - Higher weights reduce solution variety
   - Lower weights allow more variation

3. **Performance Impact**
   - Higher weights may increase computation time
   - More complex strategies (like MINIMIZE_SUPPORT) may take longer
   - Consider performance requirements when setting weights

## Example Scenarios

### High-Speed Production
```json
{
  "optimization": {
    "strategy": "MINIMIZE_HEIGHT",
    "weight": 1.0
  }
}
```
Prioritizes fastest possible build time

### Quality-Focused Build
```json
{
  "optimization": {
    "strategy": "MINIMIZE_SUPPORT",
    "weight": 0.9
  }
}
```
Focuses on part quality and minimal post-processing

### Balanced Batch Production
```json
{
  "optimization": {
    "strategy": "MAXIMIZE_DENSITY",
    "weight": 0.7
  }
}
```
Balances density with other considerations