# Documentation for main.py

This is the main entry point for the game. It contains the main game loop, handles game state transitions (like from the main menu to the game), and brings all the other modules together.

## `create_impact(pos, color=(255, 255, 255), count=5)`

This function creates a visual effect of an impact at a given position. It adds a number of `Pow` particles and a single `Pop` particle to the respective particle groups. Note: This function is also present in `collisions.py`.

```python
def create_impact(pos, color=(255, 255, 255), count=5):
    for _ in range(count):
        config.pows.add(effects.Pow(pos.x, pos.y, 10, color))
    config.pops.add(effects.Pop(pos.x, pos.y, 20, color))
```

## `reset_game()`

This function resets the game to its initial state. It clears all sprite groups, creates a new player ship, and initializes the UI bars. It's called at the beginning of a new game and when the player restarts after a game over.

```python
def reset_game():
    config.screen_shake = 0
    config.asteroids.empty()
    # ... (other groups emptied) ...
    config.ship.add(entities.Ship())
    config.ui_bars.add(ui.Bar("hp"))
    config.ui_bars.add(ui.Bar("bomb cooldown"))
    return 30, 30, 0, True, 0.0, 0
```

## `draw_group(group, shake_offset)`

A helper function to draw all sprites in a group. It applies the screen shake offset to each sprite's position before drawing.

```python
def draw_group(group, shake_offset):
    for s in group:
        # ... (drawing logic with shake offset) ...
```

## `handle_input()`

This function processes all user input from Pygame events. It handles quitting the game and the escape key.

```python
def handle_input():
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN and event.key == pygame.K_ESCAPE:
            return False
    return True
```

## `update_game_state(...)`

This function updates the state of all game objects. It handles asteroid spawning, boss spawning, and calls the `update` method for all sprite groups.

```python
def update_game_state(score, frames_passed, can_gen, ast_cd, cacd):
    # ... (game state update logic) ...
    config.ship.update()
    config.asteroids.update()
    # ... (other groups updated) ...
    return score, frames_passed + 1, can_gen, ast_cd, cacd, bg_off
```

## `handle_collisions(score)`

This function delegates collision handling to the `collisions` module.

```python
def handle_collisions(score):
    collisions.handle_player_collisions()
    score = collisions.handle_asteroid_collisions(score)
    score = collisions.handle_boss_collisions(score)
    return score
```

## `render_screen(score, bg_off)`

This function handles all rendering. It fills the screen, draws the grid background, applies screen shake, and draws all the game objects and UI elements.

```python
def render_screen(score, bg_off):
    # ... (screen shake calculation) ...
    config.screen.fill((0, 0, 0))
    # ... (drawing background, groups, UI) ...
    pygame.display.flip()
```

## `handle_game_over()`

This function checks if the game is over (i.e., the player's ship is destroyed). If it is, it displays a "Play Again" button and handles the restart logic.

```python
def handle_game_over():
    if not config.ship.sprite:
        # ... (game over logic) ...
        for b in config.buttons:
            if b.kind == "Play Again" and b.clicked:
                return reset_game()
    return None
```

## `play()`

This is the main game loop. It initializes a new game, and then continuously handles input, updates the game state, handles collisions, and renders the screen, until the player quits.

```python
def play():
    pygame.mixer.music.play(-1)
    # ... (game initialization) ...
    while running:
        running = handle_input()
        # ... (update, collisions, render) ...
        game_over_state = handle_game_over()
        if game_over_state:
            # ... (reset game) ...
        config.clock.tick(60)
```

## `main_menu()`

This function displays the main menu of the game. It has a "Play" button that, when clicked, will exit the main menu loop and start the game.

```python
def main_menu():
    # ... (main menu setup) ...
    while running:
        # ... (event handling and drawing) ...
        for b in config.buttons:
            if b.kind == "Play" and b.clicked:
                return
        # ... (drawing) ...
```

## `main()`

This is the main entry point of the program. It initializes Pygame, loads assets, sets up the display, and then enters a loop that shows the main menu and then starts the game when the player chooses to play.

```python
def main():
    pygame.init()
    # ... (asset loading and setup) ...
    while True:
        main_menu()
        play()

if __name__ == "__main__":
    main()
```
