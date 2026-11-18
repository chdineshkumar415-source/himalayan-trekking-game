# Stage A — asset optimization (2026-11-17)

Nothing in the game changed. index.html was not touched. These files go in a NEW folder `assets/`,
so every file the current game uses stays exactly where it is.

Units: 1 = 1 metre. Every model (except traveler, rock, mountain and the tree packs) now stands on y = 0
and is centred, so Stage B can place it directly.

| File | Download | Triangles | Phone memory (textures) | Real size (W × H × D m) | What changed |
|---|---|---|---|---|---|
| traveler.glb | 3.67 → 1.45 MB | 12,118 | 16 → 12 MB | unchanged (game sizes it) | Textures re-encoded (rough/metal maps 512). Walk animation kept and tested |
| rabbit.glb | 2.15 → 0.53 MB | 8,896 | 10.7 → 2.7 MB | 0.20 × 0.38 × 0.45 | Scaled from 3.7 m to 45 cm. Textures 512. Skeleton kept |
| deer.glb | 4.54 → 1.03 MB | 39,392 → 16,867 | 16 → 8 MB | 1.90 × 1.65 × 0.76 | Scaled from 98 units. Simplified (gentle; a stronger setting caused texture patches and was rejected). Removed 3 unused UV sets. Lighter material |
| backpack.glb | 4.22 → 0.43 MB | 36,010 → 6,758 | 16 → 4 MB | 0.58 × 0.60 × 0.50 | Scaled from 1.9 m. Simplified. Textures 512 |
| campfire.glb | 2.92 → 0.26 MB | 2,832 | 10.7 → 2.7 MB | 1.20 × 0.13 × 1.20 | Scaled from 118 units. Textures 512 |
| helicopter.glb | 1.02 → 0.48 MB | 5,739 | 16 → 8 MB | 10.7 × 4.0 × 12.9 | Scaled from 0.11 units. Rotor pivots moved to their hubs (see below) |
| rock_w041.glb | 2.93 → 1.27 MB | 17,132 | 16 → 12 MB | 4.48 × 2.13 × 4.84 | Textures re-encoded (rough/metal 512) |
| mountain.glb | 15.24 → 8.99 MB | 260,642 | 85 → 21 MB | 1 × 0.14 × 1 (game scales it) | Texture 4096 → 2048 |
| tent.glb | 7.16 → 1.32 MB | 9,201 | 37 → 21 MB | 3.0 × 1.63 × 2.04 | Scaled from 418 units. Normal/rough maps 512 |
| cabin.glb | 13.95 → 1.97 MB | 8,776 | 85 → 21 MB | 6.0 × 4.30 × 4.22 | Scaled from 514 units. 16 textures → 512. Door hinge recorded (see below) |
| trees_fir_pack.glb | 16.45 → 10.69 MB | 110,986 (has LODs) | 59 → 47 MB | unchanged | Normal/rough 512. Lighter material (specular extension removed) |
| trees_pine_pack.glb | 4.40 → 2.12 MB | 1,246 | 13 → 9 MB | unchanged | Same as fir pack |
| dry_branches.glb | 3.10 → 0.39 MB | 16,803 → 4,263 | 16 → 4 MB | 1.06 × 0.34 × 1.30 | Simplified. Textures 512. Converted to a single .glb |
| bow.glb | 0.77 → 0.62 MB | 12,074 | 3.7 MB | 0.67 × 1.50 × 1.46 | Scaled from 520 units. Extra camera and light removed. NOT simplified (it damaged the bow tip) |
| ice_axe.glb | 3.28 → 0.43 MB | 3,311 | 32 → 8 MB | 0.47 × 0.78 × 0.42 | Textures 512 |
| knife_buck.glb | 3.69 → 0.40 MB | 3,100 | 32 → 8 MB | 0.37 × 0.12 × 0.03 | Textures 512 (knife + sheath) |
| knife_survival.glb | 2.37 → 0.51 MB | 5,140 | 20 → 8 MB | 0.03 × 0.06 × 0.32 | Scaled from 5.4 m to 32 cm. Compass-glass "transmission" + clearcoat replaced by simple see-through glass |
| lantern.glb | 1.35 → 0.36 MB | 33,902 → 5,899 | 21 → 5 MB | 0.12 × 0.29 × 0.10 | Simplified. "Transmission" removed from the brass (it was making the whole metal body use the expensive glass shader); the glass chimney keeps its cheap see-through material |
| hammer.glb | 1.44 → 0.19 MB | 2,124 | 16 → 4 MB | 0.10 × 0.30 × 0.02 | Textures 512. Single .glb |
| hacksaw.glb | 2.06 → 0.37 MB | 6,546 | 16 → 4 MB | 0.50 × 0.11 × 0.03 | Textures 512. Single .glb |
| spade.glb | 2.31 → 0.27 MB | 11,436 → 3,470 | 16 → 4 MB | 0.17 × 1.10 × 0.05 | Simplified. Textures 512. Single .glb |
| **21 models** | **99.0 → 34.1 MB** | | | | |
| snow013/014 color, normal, roughness .jpg | unchanged | | | 1024 px | Renamed only |
| sky_day.hdr, sky_night.hdr | unchanged | | | | Renamed only |

## Moving parts (tested)
- helicopter.glb: nodes `rotor_pales` (main) and `rotor_anticouple` (tail). Their pivots were at the
  helicopter's centre, so spinning them would swing them around in a 10 m circle. Each pivot was moved to
  the rotor hub without moving anything visually. Spin axis is stored in `userData.spinAxis`
  (main = y, tail = x). Tested: tail rotor drift 0 m; main rotor rendered from above at 4 angles and turns
  around the mast.
- cabin.glb: node `Door`. Rotate local **z** (not y) — it swings on its left hinge. −1.2 opens outward.
  Stored in `userData.hingeAxis` / `userData.openOutward`. Tested with renders.

## How it was tested
Every file was loaded with Three.js r170 GLTFLoader in headless Chrome (no errors), its real-world size and
triangle count were measured, and it was rendered next to its original. Simplified models were compared
visually; settings that caused visible damage were rejected.
Not yet tested on a real iPhone.
