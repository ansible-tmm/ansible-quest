# Contributing to Ansible Quest

Thanks for your interest in contributing! This project teaches Ansible Dev Tools through a gamified medieval experience. Here's how to get involved.

## Quick start

```bash
git clone https://github.com/seanmcne/ansible-quest.git
cd ansible-quest
python -m http.server 8000
# Open http://localhost:8000
```

> You need an HTTP server because the game loads `questions.json` via XHR.

## Easiest contribution: Quiz questions

The single most impactful contribution is adding or improving quiz questions. Edit `questions.json`:

```json
{
  "tool-id": [
    {
      "q": "Your question here?",
      "opts": ["Option A", "Option B", "Option C", "Option D"],
      "ans": 2
    }
  ]
}
```

### Question guidelines

- **Minimum 3 questions per tool**, more is better (the game picks randomly)
- Test **practical knowledge**: commands, config files, workflows
- Wrong answers should be **plausible** — not joke answers
- Cover different angles: purpose, CLI usage, config, best practices, troubleshooting
- Keep questions **concise** — one clear thing being tested per question

### Tool IDs

| Tool ID | Full Name |
|---------|-----------|
| `ansible-creator` | ansible-creator |
| `ansible-lint` | ansible-lint |
| `ade` | Ansible Dev Environment |
| `molecule` | Molecule |
| `pytest-ansible` | pytest-ansible |
| `tox-ansible` | tox-ansible |
| `ansible-builder` | ansible-builder |
| `ansible-sign` | ansible-sign |
| `ansible-navigator` | ansible-navigator |

## Tool descriptions

Tool descriptions are in `index.html` inside the `districtInfoContent` object. They use HTML with helper functions:

```javascript
C('ansible-lint')           // Renders as inline <code>
CMD([                       // Renders as terminal command block
  { cmd: 'ansible-lint --fix', comment: 'auto-fix issues' }
])
```

## Game mechanics

If you want to add new features, here's the architecture:

### File structure

| File | Purpose |
|------|---------|
| `index.html` | Entire game — HTML + CSS + JS + Three.js scene |
| `questions.json` | Quiz data — loaded at runtime via XHR |
| `assets/background_song.mp3` | Background music |

### Key code sections in index.html

| Section | What it does |
|---------|-------------|
| `<style>` block | All CSS |
| `MedievalKingdom` IIFE | 3D scene, game logic, player controller |
| UI Logic IIFE (bottom) | DOM events, menus, overlays, quiz flow |

### 3D coordinate system

```
Districts:  Village (x=-28)   Township (x=0)   Fortress (x=28)
Buildings:  cx ± 10, cz ± 9 relative to district center
Road:       z = 14 (horizontal, world coords)
Campfires:  cx ± 13, z offset ± 3
```

### Important conventions

1. **No global `.hidden` class** — every element needs `#my-id.hidden { display: none; }`
2. **Dead NPCs** — set `v.dead = true` + `v.mesh.visible = false`, never splice arrays
3. **Audio** — Base64 inline if < 100KB, `assets/` folder if larger
4. **Overlays** — always reset state (buttons, panels) when opening

## Pull request process

1. Fork the repo and create a branch
2. Make your changes
3. Test locally with an HTTP server
4. Verify the game loads, quizzes work, and explore mode is functional
5. Submit a PR with a clear description of what changed

## Cursor IDE users

This repo includes Cursor skills and rules in `.cursor/` that provide context about the codebase architecture, coordinate system, and common patterns. They activate automatically when you're editing `index.html`.
