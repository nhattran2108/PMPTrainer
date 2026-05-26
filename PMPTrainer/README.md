# PMP Exam Trainer

A comprehensive, offline-first PMP (Project Management Professional) exam practice application built as a single HTML file. No server required — just open `index.html` in your browser.

![PMP Trainer](https://img.shields.io/badge/Questions-1387-blue) ![Drag Drop](https://img.shields.io/badge/Drag%20%26%20Drop-15-purple) ![License](https://img.shields.io/badge/License-MIT-green)

## Features

### Practice Mode
- **1,327 single-choice questions** — pick one correct answer from A/B/C/D
- **60 multi-select questions** — choose 2-3 correct answers from A/B/C/D/E with smart UI that tracks your selection count and only enables submit when you've picked the right number
- **Community vote distribution** — see how other exam takers voted after answering
- **Instant explanations** — topic-based explanations covering 11 PMP domains (Agile, Risk, EVM, Stakeholder, Schedule, Quality, Procurement, Team, Change Control, Communications, General)

### Drag & Drop
- **15 matching exercises** covering key PMP topics:
  - Tuckman Ladder stages
  - Risk response strategies
  - Agile/Scrum ceremonies
  - Conflict resolution techniques
  - PMBOK Process Groups order
  - Power types
  - Communication model
  - Organizational structures
  - Earned Value formulas
  - Leadership styles
  - Stakeholder engagement levels
  - Quality tools (7 basic tools)
  - Procurement contract types
  - Change request types
  - Agile roles and responsibilities

### Navigation & Modes
- **Jump to any question** by entering its number
- **3 practice modes:** Sequential, Random, Wrong-Only
- **Filter by type:** All, Single Choice, Multi-select
- **Progress tracking** with visual progress bar

### Import Custom Questions
- Upload your own JSON question files
- Supports multiple formats (array options, object options)
- Auto-deduplication
- Imported questions persist across sessions via localStorage

### Dashboard
- Overall accuracy and progress
- Single choice vs Multi-select breakdown
- Visual progress bar (correct/wrong/unanswered)
- Quick access to wrong questions for review

## Getting Started

### Option 1: Open directly
```
Just double-click index.html in your browser.
```

### Option 2: Serve locally
```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve .
```

Then open `http://localhost:8080` in your browser.

## Import Custom Questions

Go to **Import JSON** in the sidebar and upload a `.json` file with this format:

```json
[
  {
    "question": "What should the project manager do?",
    "options": [
      "A. Facilitate a workshop to gather input from all stakeholders",
      "B. Define the difference between essential and non-essential features",
      "C. Involve design consultant to mediate stakeholder discussions",
      "D. Identify the root cause of the stakeholder inability to agree"
    ],
    "answer": "D. Identify the root cause of the stakeholder inability to agree"
  }
]
```

Also supports object-style options:

```json
[
  {
    "question": "Which EVM metric indicates cost efficiency?",
    "options": {
      "A": "CPI",
      "B": "SPI",
      "C": "CV",
      "D": "SV"
    },
    "correct": "A"
  }
]
```

## Data

- `index.html` — Complete self-contained application (HTML + CSS + JS + embedded data)
- `pmp_questions.json` — Raw extracted question data (1,387 questions) for external use

## Tech Stack

- **Zero dependencies** — Pure HTML/CSS/JS, no framework
- **Single file** — Everything in one `index.html` (~1.1MB)
- **Offline-first** — No network requests, works without internet
- **localStorage** — Progress and imported questions persist across sessions
- **Dark theme** — Easy on the eyes for long study sessions

## Data Source

Questions extracted from PMP Exam practice materials (ExamTopics, 527 pages PDF). Community vote distributions included where available.

## Project Structure

```
PMPTrainer/
├── index.html            # Main application (self-contained)
├── pmp_questions.json    # Raw question data for external use
├── README.md             # This file
└── CLAUDE.md             # AI development instructions
```

## License

MIT
