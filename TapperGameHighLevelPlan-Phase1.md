# Phase One Development Plan: Single-Level Saloon Recreation

This plan focuses on building a minimum viable product (MVP) for the game, implementing only the saloon level based on the provided screenshot. The level will feature the core mechanics: serving drinks across 4 bars, handling customers (cowboys), sliding mugs, catching empties, collecting tips, and basic failure conditions. We'll aim for a playable prototype that captures the essence of Tapper, with scoring and lives, but without level progression, bonus rounds, or additional themes. Development assumes Python 3.x with Pygame installed (via pip install pygame). Total estimated time: 7-10 days for an intermediate developer working part-time.

## Step 1: Project Setup and Environment Preparation (1 day)

* Create a new project directory (e.g., tapper_clone/).

* Install dependencies: Run pip install pygame if not already installed. No other libraries are needed for phase one.

* Set up file structure:xa

  * main.py: Entry point for the game loop.

  * game.py: Main game class handling states (e.g., playing, game over).

  * entities.py: Classes for game objects (bartender, customers, mugs, tips).

  * constants.py: Define constants like screen dimensions (640x480), colors (e.g., blue background #00008B), bar positions (staggered vertically as in screenshot), speeds (customer walk: 1-2 pixels/frame), and assets paths.

  * assets/: Folder for images and sounds. Download or create pixel art sprites mimicking the screenshot (e.g., bartender, cowboy customer with green hat, mugs, taps, Budweiser logo—use free tools like Aseprite or find 8-bit assets on itch.io/OpenGameArt). Include basic sounds (pouring, sliding, breaking) from freesound.org.

* Initialize Git for version control: Run git init and commit the initial structure.

* Test setup: Write a simple main.py script to open a Pygame window with the blue background and static bar counters. Ensure it runs at 60 FPS using pygame.time.Clock().

## Step 2: Implement Basic Game Loop and Input Handling (1 day)

* In game.py, create a Game class with methods: __init__ (initialize Pygame, screen, clock, sprites), run (main loop with event handling, updates, drawing), handle_events (quit on escape, process inputs).

* Add keyboard controls:

  * Up/Down arrows: Switch between the 4 bars (bartender snaps to the right end of the selected bar).

  * Right arrow: Move bartender left along the current bar (to catch empties or collect tips).

  * Left arrow: Move bartender right (back toward taps).

  * Spacebar: Pour and slide a full mug leftward from the tap on the current bar.

* Implement state management: Use a simple state variable (e.g., "playing", "game_over") to control the loop.

* Draw the background: Load and blit the saloon elements (Budweiser sign at top, 4 brown bar counters staggered diagonally, taps on the right, doors on the left).

* Test: Run the game to navigate the bartender between bars and move along one bar without other elements.

## Step 3: Create Core Entities (2 days)

* In entities.py, define Pygame sprite classes:

  * Bartender (pygame.sprite.Sprite): Attributes: position (start at bottom bar's right end), current_bar (0-3), speed (e.g., 3 pixels/frame). Methods: update (handle movement bounds—can't go beyond bar ends), pour (create a new FullMug at tap position, animate filling briefly).

  * Customer (pygame.sprite.Sprite): Subclass for Cowboy. Attributes: bar_index, position (start at left door), speed (1 pixel/frame initially), state (walking, drinking, leaving). Methods: update (move right; if reaches right end, trigger failure).

  * Mug (pygame.sprite.Sprite): Subclasses FullMug and EmptyMug. Attributes: bar_index, position, speed (2-3 pixels/frame), direction (left for full, right for empty). Methods: update (move; if falls off end, trigger failure/break).

  * Tip (pygame.sprite.Sprite): Attributes: bar_index, position (starts where customer leaves it), speed (slides left). Methods: update (move left; disappear off end if not collected).

* Use sprite groups: all_sprites, customers, mugs, tips in Game class for easy updating/drawing.

* Load sprites: Assign images (e.g., bartender facing left/right, customer animations for walking/catching).

* Test: Spawn a static customer and mug on one bar; ensure they draw correctly and bartender can "interact" via position overlap (no collisions yet).

## Step 4: Implement Core Mechanics and Collisions (2-3 days)

* Spawning: In Game.update, randomly spawn customers on bars (e.g., 1-2 per 5-10 seconds, starting slow). Use a timer (pygame.time.get_ticks()) to control waves.

* Movement and Updates:

  * Customers advance right; if they catch a full mug (collision check via pygame.sprite.collide_rect), they "drink" (brief animation), slide an empty mug right, and retreat left (or leave, leaving a tip 20% chance).

  * Mugs slide in their direction; full mugs unpicked fall off left (failure), empties off right (if not caught by bartender).

  * Bartender catches empties or tips on collision (add points: +10 for empty, +50 for tip; on tip, trigger brief customer pause/distraction).

* Collision Detection: Use pygame.sprite.spritecollide for interactions (e.g., customer-full_mug, bartender-empty_mug/tip).

* Failure Conditions: Deduct life on customer reaching right, mug breaking, or full mug unpicked. Start with 3 lives; on 0, switch to game_over state.

* Scoring: +20 points per served customer, displayed top-left (like "550" in screenshot). Add high score tracking in a simple text file.

* Sounds: Play effects on pour, catch, break, etc.

* Test: Playthrough a full "level" (e.g., serve 10 customers without dying); debug issues like overlapping sprites or missed collisions.

## Step 5: Add UI, Polish, and Testing (1-2 days)

* HUD: Draw score (top-left), lives (top-right, e.g., beer mug icons starting at 3), level indicator (fixed as "1").

* Game Over Screen: Simple text overlay with score and restart option (e.g., press R).

* Animations: Basic frames for bartender pouring, customer catching/drinking, mug breaking (particle effect optional).

* Balance Tweaks: Adjust speeds and spawn rates so the level is challenging but completable (e.g., clear after 20 customers served, but for phase one, make it endless until game over).

* Testing:

  * Functional: Verify all mechanics (e.g., switch bars while mug sliding, multiple customers per bar up to 4).

  * Edge Cases: What if multiple mugs on one bar? Bartender at wrong bar during failure?

  * Performance: Ensure smooth 60 FPS on basic hardware.

  * Playtest: Time a full session; note fun factor and bugs.

* Documentation: Add comments in code; write a README.md with run instructions (python main.py).

## Step 6: Review and Iteration (0.5 day)

* Compare to Screenshot: Ensure visual fidelity (e.g., Budweiser sign, staggered bars, character positions).

* Gather Feedback: If possible, share prototype with others for input.

* Commit and Backup: Push to Git; prepare for phase two (adding levels) by modularizing code (e.g., level data in a dict for easy expansion).

* Next Steps Teaser: Note that phase two could add level progression, bonus rounds, and themes.

This plan ensures a focused, iterative build. Track progress daily, and adjust timelines as needed. If branding is an issue, replace Budweiser with a generic "Beer" theme.
