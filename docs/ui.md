# Documentation for ui.py

This module contains all the classes related to the user interface (UI). This includes the main menu animation, buttons, health bars, upgrade selection panels, and the boss introduction text.

## `Mmanim`

This class handles the animation for the main menu. It features several shapes and text elements that move into place using easing functions to create a smooth and dynamic intro.

### `__init__(self, W, H)`
Initializes the animation with the screen dimensions `W` and `H`. It sets up the initial and target positions for the animated elements.

### `update(self)`
Updates the positions of the animated elements each frame, moving them towards their target positions.

### `draw(self, screen)`
Draws the animated elements on the screen.

```python
class Mmanim:
    def __init__(self, W, H):
        # ...

    def update(self):
        # ...

    def draw(self, screen):
        # ...
```

## `Button(sprite.Sprite)`

The `Button` class creates interactive UI buttons. They can be clicked by the mouse and change size when hovered over.

### `__init__(self, x, y, kind)`
Initializes a button at position `(x, y)` with a specific `kind` (e.g., "Play", "Play Again"), which is also used as the button's text.

### `update(self)`
Checks for mouse interaction. If the mouse is over the button, it grows in size. If the mouse is clicked, the `clicked` attribute is set to `True`.

### `draw(self, screen)`
Draws the button and its text on the screen.

```python
class Button(sprite.Sprite):
    def __init__(self, x, y, kind):
        # ...

    def update(self):
        # ...

    def draw(self, screen):
        # ...
```

## `Bar(sprite.Sprite)`

The `Bar` class is used to display status bars, such as the player's health and bomb cooldown.

### `__init__(self, kind)`
Initializes a bar of a specific `kind` ("hp" or "bomb cooldown"). The kind determines the bar's position and the data it represents.

### `draw(self, screen)`
Draws the bar on the screen. The fill of the bar is determined by the corresponding value in the `config.ship.sprite` (e.g., HP or bomb cooldown).

```python
class Bar(sprite.Sprite):
    def __init__(self, kind):
        # ...

    def draw(self, screen):
        # ...
```

## `Upgrade(sprite.Sprite)`

The `Upgrade` class represents the panels that appear when the player can choose an upgrade. These panels display the name of the upgrade and can be clicked to apply the upgrade to the player's ship.

### `__init__(self, x, y)`
Initializes an upgrade panel at `(x, y)`. It randomly selects an upgrade from a list of available upgrades.

### `update(self)`
Handles mouse interaction with the panel. When the mouse hovers over it, the panel grows. When clicked, it applies the selected upgrade and then removes itself.

### `draw(self, screen)`
Draws the upgrade panel and its text.

```python
class Upgrade(sprite.Sprite):
    def __init__(self, x, y):
        # ...

    def update(self):
        # ...

    def draw(self, screen):
        # ...
```

## `BossName(sprite.Sprite)`

This class displays the name of the boss when it appears. The name fades in and then fades out after a short time.

### `__init__(self)`
Initializes the boss name display. It loads the boss name image from the assets or creates a text surface if the image is not available.

### `update(self)`
Updates the lifetime and alpha of the boss name, causing it to fade out and eventually be removed.

### `draw(self, screen)`
Draws the boss name on the screen.

```python
class BossName(sprite.Sprite):
    def __init__(self):
        # ...

    def update(self):
        # ...

    def draw(self, screen):
        # ...
```
