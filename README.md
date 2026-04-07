# Ansible Quest

A medieval-themed interactive learning experience for [Ansible Development Tools](https://ansible.readthedocs.io/projects/dev-tools/). Explore a 3D kingdom, take quizzes on 9 tools, and master the Ansible dev workflow — from creation to testing to deployment.

**[Play it live on GitHub Pages](https://seanmcne.github.io/ansible-quest/)**

![Ansible Quest](https://img.shields.io/badge/Ansible-Quest-ffd700?style=for-the-badge&logo=ansible&logoColor=white)

## What is this?

Ansible Quest teaches the Ansible development toolchain through a gamified medieval experience. The kingdom is divided into three districts, each mapping to a phase of the dev workflow:

| District | Phase | Tools |
|----------|-------|-------|
| **Village** | Create | `ansible-creator`, `ansible-lint`, `ade` |
| **Township** | Test | `molecule`, `pytest-ansible`, `tox-ansible` |
| **Fortress** | Deploy | `ansible-builder`, `ansible-sign`, `ansible-navigator` |

### Two ways to play

1. **Map Mode** — Click districts, read tool descriptions, and take quizzes to unlock all 9 knowledge chests
2. **Explore Mode** — Walk through the kingdom as a knight, find chests in the world, fight ogres, and chop trees

## Running locally

No build step required. Just serve the files:

```bash
# Python
python -m http.server 8000

# Node
npx serve .

# Then open http://localhost:8000
```

> The game loads `questions.json` via XHR, so you need an HTTP server — opening `index.html` directly as a file won't load the quiz questions.

## Project structure

```
ansible-quest/
├── index.html          # The entire game (HTML + CSS + JS + Three.js)
├── questions.json      # Quiz questions — easy to edit and contribute to
├── assets/
│   └── background_song.mp3
├── .github/
│   └── workflows/
│       └── static.yml  # GitHub Pages deployment
└── README.md
```

## Contributing

Contributions are welcome! Here are the easiest ways to help:

### Add or improve quiz questions

Edit `questions.json`. Each tool has an array of questions with this format:

```json
{
  "q": "What is the primary purpose of ansible-lint?",
  "opts": [
    "To execute Ansible playbooks faster",
    "To validate YAML syntax only",
    "To enforce best practices and detect issues in Ansible content",
    "To manage Ansible inventories dynamically"
  ],
  "ans": 2
}
```

| Field | Description |
|-------|-------------|
| `q` | The question text |
| `opts` | Array of 4 answer options |
| `ans` | Zero-based index of the correct answer |

**Guidelines for good questions:**
- Each tool should have at least 3 questions (more is better)
- Questions should test practical knowledge, not trivia
- Wrong answers should be plausible but clearly wrong to someone who knows the tool
- Cover different aspects: purpose, commands, configuration, best practices

### Improve tool descriptions

Tool descriptions live in `districtInfoContent` inside `index.html`. They use HTML with these formatting helpers:

- `C('ansible-lint')` — renders as inline code
- `CMD([{ cmd: 'ansible-lint --fix', comment: 'auto-fix issues' }])` — renders as a terminal command block with `$` prompt

### Other contributions

- **Performance improvements** — the game runs entirely client-side with Three.js
- **New game mechanics** — ideas for additional interactive elements
- **Accessibility** — keyboard navigation, screen reader support, color contrast
- **Mobile support** — touch controls for explore mode

### Development tips

- Everything is in a single `index.html` to keep deployment simple (GitHub Pages, no build)
- The 3D scene uses [Three.js r160](https://threejs.org/) loaded from CDN
- Audio files under 100KB are Base64-encoded inline; larger files go in `assets/`
- Districts are centered at x = -28 (Village), 0 (Township), 28 (Fortress)
- Buildings occupy a grid from x = -10 to 10, z = -9 to 9 relative to each district center
- The horizontal road runs at z = 14 (world coordinates)

## Tech stack

- **Three.js r160** — 3D rendering
- **Vanilla JS** — no frameworks, no build step
- **GitHub Pages** — static hosting
- **Google Fonts** — Cinzel + MedievalSharp for medieval typography

## License

MIT
