---
name: ansible-quest-dev
description: >-
  Develop and extend the Ansible Quest 3D game. Use when editing index.html,
  questions.json, adding game mechanics, fixing UI/UX issues, or working with
  Three.js scene objects in this project.
---

# Ansible Quest Development

## Architecture

Single-file game (`index.html`) with Three.js r160 (CDN). No build step. Deployed to GitHub Pages.

### Key modules inside index.html

- **MedievalKingdom IIFE** (line ~1378) — 3D scene, game logic, player controller, all Three.js code
- **UI Logic IIFE** (bottom) — DOM event handlers, district menus, quiz flow, overlays

### External files

- `questions.json` — quiz questions, loaded via XHR at startup
- `assets/background_song.mp3` — background music

## 3D Scene Layout

Districts centered at: Village (x=-28, z=0), Township (x=0, z=0), Fortress (x=28, z=0).

- Buildings: grid from cx-10 to cx+10, cz-9 to cz+9
- Horizontal road: z=14 (world coords)
- Vertical roads: x=0 relative to each district center
- Campfires: cx-13 and cx+13, offset z by ±3

When placing new objects, avoid building zones and campfire positions.

## Common patterns

### Adding a new 3D object
1. Create geometry + material
2. Position using world coordinates (check building/road zones)
3. Add to `scene` or a district `group`
4. If interactive, push collision data to `_buildingBoxes` array

### Adding UI overlays
1. Add HTML inside `#aspect-ratio-container`
2. Use `.hidden` class pattern — define `#my-el.hidden { display: none; }` in CSS
3. Toggle via `classList.add/remove("hidden")`

### CSS class gotcha
Every element using `.hidden` needs its own CSS rule: `#my-element.hidden { display: none; }`. There is no global `.hidden` class.

### Audio
- Files < 100KB: Base64-encode and embed as `new Audio("data:audio/mp4;base64,...")`
- Files > 100KB: place in `assets/` folder

### Quiz questions
Questions are in `questions.json`. The game loads them via XHR. Format:
```json
{ "q": "Question text", "opts": ["A", "B", "C", "D"], "ans": 0 }
```
`ans` is zero-based index of the correct option.

## Performance considerations

- Use `InstancedMesh` for repeated objects (trees already use this)
- Avoid adding `PointLight` per object — use shared/ambient lighting
- Skip animation for dead NPCs (`if (v.dead) continue`)
- Minimize `scene.traverse()` calls in the render loop
- Cache DOM lookups outside of hot loops

## Explore mode state

Key variables: `exploreActive`, `playerMesh`, `playerDead`, `playerLaunched`, `playerHealth`, `playerJumpY`, `exploreKeys`, `_nearChestIdx`, `_activeChestIdx`.

Player faces +Z when camera theta=0. WASD movement uses camera-relative vectors.

## Testing changes

Serve locally with `python -m http.server 8000` — required for XHR to load `questions.json`.
