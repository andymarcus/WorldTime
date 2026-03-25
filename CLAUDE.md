# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

World Time is a single-file HTML application (`world-time.html`) that displays a Gantt-style timezone comparison tool. It shows the current time across multiple timezones with color-coded 24-hour bars representing time-of-day periods (night, dawn, work hours, dusk, etc.).

## Development

No build system, dependencies, or tooling — just open `world-time.html` in a browser. All CSS, HTML, and JavaScript live in that one file.

## Architecture

- **CSS variables** in `:root` control theming (colors, layout column width `--lw`)
- **Fonts**: Cormorant (serif, title) and Outfit (sans-serif, UI) loaded from Google Fonts
- **`TZ_LIST`**: hardcoded array of ~45 timezone entries (`{label, zone}`) using IANA timezone identifiers
- **`TIME_BANDS`**: defines hour ranges mapped to colors and labels (Night, Dawn, Work hours, etc.)
- **`zones` array**: runtime state tracking displayed timezones; the user's local timezone is auto-detected via `Intl.DateTimeFormat` and always shown first with a "local" badge
- **Rendering**: imperative DOM construction — `buildRow()` creates each timezone row with label, clock, gradient bar, now-indicator, tooltip, and remove button. `render()` rebuilds all rows from the `zones` array
- **Live updates**: `tick()` runs every 1s to update clocks and now-indicator positions; gradients and ticks refresh every 60s
- **Time calculations**: all time math uses `Intl.DateTimeFormat` with explicit `timeZone` option — no external date libraries
- **Add/remove**: users can add timezones from a `<select>` grouped by region; duplicates are silently rejected; the local timezone cannot be removed
