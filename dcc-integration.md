# Kinesis Parallel — DCC Integration

How Kinesis connects to external digital content creation tools.

## Interchange format

glTF is the primary interchange: compact, web-native, supports PBR materials, morph targets, and skeletal animation. Every export path produces a glTF that loads in the Three.js atelier without conversion.

## Maya

Round-trip via FBX or Alembic for rigged characters. glTF for static mesh review. The facial rig exports as a joint hierarchy with weight maps that the écorché layer can consume directly.

## Blender

Primary sculpting and retopo target. glTF export with custom properties preserved for muscle-region metadata. The écorché muscle map can be authored as Blender vertex groups and imported back.

## Houdini

Procedural modeling and simulation. Vellum and Pyro outputs convert to glTF for the atelier. Houdini's attribute system maps to the swarm's confidence metadata — reconstruction residuals become point attributes.

## ZBrush

High-poly sculpting. Decimation exports feed the topology desk. Normal and displacement maps bake into the PBR texture set.

## COLMAP

Photogrammetry ground truth. Kinesis's FAST-based pipeline is a speed-optimized subset; COLMAP runs as the verification pass on critical assets. Its sparse reconstruction output validates Kinesis's bundle-adjusted poses.

## Metashape

Commercial photogrammetry alternative. Dense cloud and mesh exports import directly. Texture atlases from Metashape replace Kinesis's baking stage when source imagery is too large for in-browser processing.

## RealityCapture

Fast capture-to-mesh for on-site work. Exports feed the same import path as Metashape.

## Bundle adjustment

Shared math across all DCC paths. Kinesis's internal BA solver and COLMAP's output use compatible coordinate conventions so poses transfer without re-solving.

## Export contract

Every DCC export must: (1) produce valid glTF 2.0, (2) preserve PBR material parameters, (3) include morph target names matching the écorché muscle set, (4) carry confidence metadata as extras when available.