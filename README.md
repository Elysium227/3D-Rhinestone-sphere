# Rhinestone Sphere

A disco ball made of code — 48,000 rhinestones, sequins, and jewel tesserae 
packed onto a rotating sphere, each catching its own light at its own angle.

**[Live demo →](#)**

---

## What it is

A WebGL experiment in making polygons feel physical. The challenge wasn't 
density — it was sparkle. Getting flat geometry to read as cut glass required 
three separate systems working together:

1. **Off-axis tilt** — every gem is rotated slightly off the sphere normal before 
   placement, so each one catches the light at a different moment as the sphere 
   turns. Without this, the whole surface lights up uniformly. With it, you get 
   the independent per-gem flash of a real rhinestone.

2. **Custom GLSL shader** — MeshPhongMaterial's specular tops out too soft. 
   The fragment shader runs Blinn-Phong at exponent 12,000 (mirror-tight) plus 
   a coloured secondary lobe and a Fresnel rim so gems glow at grazing angles.

3. **Star-glint overlay** — a separate `THREE.Points` layer sits on the same 
   48,000 positions with additive blending and a canvas-drawn 8-ray star sprite. 
   ~18 random gems flash per frame, independent of the shader layer.

## Stack

- [Three.js r128](https://threejs.org/) — instanced meshes, custom ShaderMaterial
- Vanilla JS, no build step
- Single HTML file

## Usage

Open `index.html` in a browser. Drag to spin; release for inertia.

## Technical notes

- 48,000 points on a Fibonacci lattice (golden-angle spiral) for even coverage
- Three gem types: OctahedronGeometry (tesserae), CylinderGeometry (sequins), 
  SphereGeometry (rhinestones) — sized to tile at this density with no gaps
- 6 coloured point lights orbit the sphere independently at different speeds 
  and tilts so the specular pattern never repeats
