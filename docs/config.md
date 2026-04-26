# Documentation for config.py

This module centralizes all global configuration variables and sprite groups for the game. This approach makes it easy to access and modify game-wide settings and collections of game objects from any other module.

## Game Config

This section contains general game settings.

- `W`, `H`: These variables store the width and height of the game screen. They are initialized to 0 and set to the actual screen dimensions at runtime.
- `screen`: This holds the main display surface for the game, provided by Pygame.
- `screen_shake`: A value that determines the intensity and duration of the screen shake effect.
- `boss_image_asset`: This variable holds the image asset for the boss.
- `shoot_sound`, `boom_sound`, `hit_sound`: These variables hold the sound effects for shooting, explosions, and hits, respectively.
- `lv_req`: The experience points required to level up.
- `clock`: The Pygame clock object, used to control the frame rate.

```python
from pygame import sprite


# ---- Game Config ----
W, H = 0, 0
screen = None
screen_shake = 0
boss_image_asset = None
shoot_sound, boom_sound, hit_sound = None, None, None
lv_req = 2
clock = None
```

## Sprite Groups

This section defines all the sprite groups used in the game. Using sprite groups makes it easier to manage and update related game objects.

- `ship`: A `GroupSingle` to hold the player's ship. `GroupSingle` can only hold one sprite at a time.
- `asteroids`: A group for all the asteroid sprites.
- `lasers`: A group for the player's laser sprites.
- `enemy_lasers`: A group for the enemy and boss laser sprites.
- `pops`: A group for "pop" particle effects.
- `pows`: A group for "pow" particle effects.
- `buttons`: A group for all UI button elements.
- `ui_bars`: A group for UI bars, like health and experience bars.
- `upgs`: A group for upgrade items that appear on the screen.
- `effects`: A group for miscellaneous visual effects.
- `boss_group`: A `GroupSingle` for the boss sprite.
- `overlay_ui`: A group for overlay UI elements.

```python
# ---- Sprite Groups ----
ship = sprite.GroupSingle()
asteroids = sprite.Group()
lasers = sprite.Group()
enemy_lasers = sprite.Group()
pops = sprite.Group()
pows = sprite.Group()
buttons = sprite.Group()
ui_bars = sprite.Group()
upgs = sprite.Group()
effects = sprite.Group()
boss_group = sprite.GroupSingle()
overlay_ui = sprite.Group()
```
