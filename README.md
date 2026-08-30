D4T Dual Live SPT Overlay
Dedicated web overlay repository for the D4T BGMI DualPC live standings
system.
Overview
This repository contains the HTML/CSS/JavaScript and visual assets
required for the live BGMI standings overlay.
The intended data flow is:
``` text
Link PC 1 ──┐
            ├──> DualPC Server ──> Merged Live State ──> Web Overlay ──> OBS
Link PC 2 ──┤
Main PC  ───┘
```
Main Features
Live BGMI team standings
Player status display
Team kills
Alive player count
Finished/eliminated player indication
Team logos
Live match information
OBS Browser Source support
DualPC support for Link PC 1 and Link PC 2
GitHub Pages compatible static hosting
Repository Structure
``` text
dual-livespt/
├── index.html
├── data/
│   └── map/
├── logos/
├── match end/
├── wwcd/
└── README.md
```
Data Flow
Both Link PC sources must remain identifiable at the backend:
``` text
Link PC 1 -> source_pc = link_pc
Link PC 2 -> source_pc = link_pc_2
                     |
                     v
               DualPC Server
                     |
                     v
                Merge Engine
                     |
                     v
              Master Live State
                     |
                     v
                  Overlay
```
The overlay should consume the authoritative merged state and must not
silently display only Link PC 1.
Important Data Rules
The overlay must not assume that:
Link PC 1 is the only source.
Link PC 2 uses the same team ordering.
Missing OCR temporarily means a player was eliminated.
A slot number can be shortened or normalized.
A temporary zero-kill value should overwrite a previously confirmed
kill.
Slot identifiers must remain exact:
``` text
16 != 6
18 != 8
```
The frontend must never truncate or substitute slot numbers.
Player Status
Player status should distinguish between:
Alive
Eliminated / finished
Unknown or temporarily unavailable
A temporary missing observation should not erase a previously confirmed
valid state unless the authoritative backend explicitly confirms the
change.
Team Status
Each team can display:
Slot
Team name
Total kills
Alive count
Finished count
Player list
Individual player kills/status
Team totals should remain consistent with the authoritative player data.
Identity
Use stable team/player identifiers whenever they are available.
Display names are presentation values and should not be the only
mechanism used to decide whether two records represent the same player.
OBS Usage
The final published overlay URL can be added to OBS as a Browser Source.
Recommended workflow:
Open the published overlay URL in a browser.
Confirm live data is updating.
Add the URL to an OBS Browser Source.
Set the required resolution.
Refresh/reload the source when required.
Verify both Link PC sources are represented.
Development
Keep this repository focused on the web overlay.
Do not copy the Python application, `.venv`, PyInstaller build folders,
executable files, or private credentials into this repository.
Deployment
Recommended deployment:
``` text
GitHub Repository
       |
       v
GitHub Pages
       |
       v
Public Overlay URL
       |
       v
OBS Browser Source
```
DualPC Migration
The existing production overlay must remain untouched until the new
overlay is tested.
Migration sequence:
``` text
1. Prepare the new repository
2. Copy only required overlay assets
3. Push to GitHub
4. Enable and test GitHub Pages
5. Test the overlay independently
6. Connect it to the DualPC live endpoint
7. Verify Link PC 1
8. Verify Link PC 2
9. Verify merged players, kills, alive and finished state
10. Replace the production overlay URL only after successful testing
```
Testing Checklist
Before production use:
[ ] Link PC 1 data arrives
[ ] Link PC 2 data arrives
[ ] Both sources appear in the merged state
[ ] No source is silently filtered
[ ] Slot 16 remains 16
[ ] Slot 18 remains 18
[ ] Players do not disappear unexpectedly
[ ] Player kills do not randomly reset to zero
[ ] Eliminated players do not become alive again
[ ] Finished indicators match authoritative state
[ ] Team totals match player totals
[ ] Team logos load correctly
[ ] Overlay works in OBS
[ ] Overlay survives browser refresh
[ ] A new match/session does not show stale data
Security
Never commit:
API secrets
Private keys
License secrets
Customer hashes
Machine identifiers
`.env` files containing credentials
Private server credentials
Status
This is the dedicated DualPC live overlay repository.
The existing production overlay should remain unchanged until this
repository has passed full testing.
