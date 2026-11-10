# Changed from the plan

No architectural or gameplay changes. Implemented the complete game in `index.html` with inline CSS and JavaScript, one canvas, real DOM tell badges, a per-bot finite-state machine, scrolling decision feed, score, health, and wave spawning. No build step, dependencies, accounts, or network requests.

Seeded 18 bots across all active states and 19 initial log entries, including aiming, fleeing, and calling-help targets. Each wave adds 18 bots. Eliminated badges remain in the DOM with `data-tell="eliminated"`. Calls boost bots within 250 arena units for 10 seconds. Added pause/resume and restart controls for demonstrations. The static page includes meaningful instructions, HUD labels, and the legend before JavaScript executes.

Verification: executed the inline script with a Node DOM/canvas harness. Passed initial badge/log counts, all three required tell types, legend count, health reduction after four simulated seconds, elimination and score increments for all three vulnerable states, wave growth, pause, successful help calls, and restart. Checked that the document has no asset references to resolve. Canvas drawing calls and frame updates execute in the harness.

# Could not do

None of the planned functionality was blocked.

# Omitted for lack of a service

None. This implementation needs no external service or generated images.
