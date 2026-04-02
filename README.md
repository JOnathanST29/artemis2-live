# Artemis II — Live Trajectory Map

Real-time visualization of NASA's Artemis II crewed lunar flyby mission, built with React and powered by **real trajectory data** from JPL Horizons.

## Data Source

All trajectory data comes from the **JPL Horizons API** (`ssd.jpl.nasa.gov/api/horizons.api`):

- **Orion spacecraft** — `COMMAND='-1024'` (Artemis II / Integrity / Orion EM-2)
- **Moon** — `COMMAND='301'`
- **Frame** — Ecliptic J2000, Earth-centered
- **Source** — NASA/JSC Flight Dynamics Operations

The app ships with 299 pre-fetched Orion state vectors and 75 Moon position vectors covering the full 10-day mission (Apr 2–11, 2026). A manual **REFETCH** button attempts a live pull from Horizons to get the latest trajectory updates.

### What's real

| Element | Source | Update frequency |
|---------|--------|-----------------|
| Orion position (X, Y, Z) | JPL Horizons `-1024` | Embedded + manual refetch |
| Orion velocity (VX, VY, VZ) | JPL Horizons `-1024` | Embedded + manual refetch |
| Moon position | JPL Horizons `301` | Embedded + manual refetch |
| Distance to Earth/Moon | Computed from real vectors | Every second (interpolated) |
| Velocity readout | Computed from real vectors | Every second (interpolated) |
| MET clock | Wall clock since launch epoch | Every second |
| Mission milestones | JPL Horizons metadata record | Static (from mission plan) |

### CCSDS OEM format

The ephemeris data on Horizons follows the [CCSDS Orbital Ephemeris Message](https://public.ccsds.org/Pubs/502x0b2c1e2.pdf) standard. State vectors are in the Mean of J2000 reference frame with 4-minute intervals (or 2-second during maneuvers).

## Tech Stack

- React 18
- Vite
- Pure SVG rendering (no mapping library)
- Zero external dependencies beyond React

## Run locally

```bash
npm install
npm run dev
```

## Architecture

```
src/
  App.jsx          — Main component: data, state, fetch, render
  index.jsx        — Entry point
public/
  index.html       — Shell
```

The 2D projection works by:
1. Getting the Moon position vector at current time
2. Using the Earth→Moon direction as the X-axis
3. Computing a perpendicular Y-axis in the ecliptic plane
4. Projecting all 3D positions onto this 2D plane

This keeps the Earth-Moon line horizontal and shows the free-return trajectory curving above/below.

## Credits

- **Trajectory data**: NASA/JPL Horizons System — `https://ssd.jpl.nasa.gov/`
- **Mission operations**: NASA/JSC Flight Dynamics Operations
- **Built by**: [@jonathanst29](https://x.com/jonathanst29)

## License

MIT
