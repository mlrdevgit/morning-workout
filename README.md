# Morning Workout

A timer-driven web app that walks you through a progressive morning workout routine. Choose from 1, 3, 5, or 10 minutes -- each tier builds on the previous one. The key rule: you only have to do the 1-minute version.

## Usage

Open `index.html` in a browser. No build step, no dependencies, no server needed.

Pick a tier:

| Tier | Duration | Focus |
|------|----------|-------|
| 1 min | 3 exercises | Spine mobility (cat-cow, forward fold, overhead stretch) |
| 3 min | 7 exercises | + Foundational strength (squats, pushups, glute bridge, plank) |
| 5 min | 11 exercises | + Mobility & stability (world's greatest stretch, bird dog, deep squat hold, shoulder CARs) |
| 10 min | 16 exercises | + Full routine (reverse lunges, side plank, hip hinge, wall angels, breathing) |
| Ultra-minimal | 3 exercises | Just 5 squats, 5 pushups, 5 deep breaths |

## Features

- **Auto-advancing timer** with circular SVG progress ring
- **Audio cues** via Web Audio API -- beeps at 3-2-1 countdown, chime on exercise transitions, celebration on completion
- **Text-to-speech** announces exercise names so you know what's next without looking at the screen
- **3-second rest pause** between exercises with inline countdown
- **Continue to next tier** after completing a set -- only the new exercises, no repeats
- **YouTube links** next to each exercise name -- opens a search for a demo video in a new tab
- **Tap to pause/resume** anywhere on the timer area
- **Skip and Stop** controls
- **Dark/light theme** toggle (dark default, preference saved to localStorage)
- **Mobile-first** responsive design with fluid typography
- **Mute toggle** for audio (also silences text-to-speech)

## Architecture

Single self-contained HTML file with embedded CSS and JS. No external dependencies.

```
index.html
├── <style>
│   ├── CSS custom properties for dark/light theming
│   ├── Mobile-first responsive layout (flexbox + grid)
│   └── Fluid typography via clamp()
├── <body>
│   ├── Selection screen -- tier buttons + ultra-minimal option
│   ├── Countdown overlay -- 3-2-1 before workout starts
│   ├── Workout screen -- exercise name, YouTube link, tips, SVG ring timer, controls
│   └── Complete screen -- summary, restart, and continue-to-next-tier option
└── <script>
    ├── Exercise data -- flat array with tier tags, filtered at runtime
    ├── Timer engine -- requestAnimationFrame loop tracking elapsed time
    ├── Rest pause -- 3-second inline countdown between exercises
    ├── Audio -- Web Audio API oscillator for beeps (no external files)
    ├── Text-to-speech -- Web Speech API announces upcoming exercises
    ├── Ring progress -- SVG stroke-dashoffset animation
    ├── Tier progression -- continue to next tier with only new exercises
    └── State management -- screen transitions, pause/resume, skip/stop, rest
```

### Design decisions

- **requestAnimationFrame over setInterval**: Doesn't drift, pauses when tab is backgrounded, smooth visual updates.
- **Tier filtering**: Exercises have a `tier` property. Selecting "5 min" filters to all exercises where `tier <= 5`, so each tier naturally includes all exercises from shorter tiers.
- **Tier continuation**: After completing a tier, "continue" filters to exercises where `tier > previousTier && tier <= nextTier`, so you only get the new exercises.
- **Web Audio API**: Generates tones programmatically -- no audio files to load or host.
- **Web Speech API**: Text-to-speech announces exercise names during rest pauses so you can prepare without looking. Respects the mute toggle.
- **CSS custom properties**: Theme switching is just toggling a `.light` class on `<html>`, which swaps all color variables.
