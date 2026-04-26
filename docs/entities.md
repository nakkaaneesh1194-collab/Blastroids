# Documentation for entities.py

This module defines all the game entities as classes. This includes the player's ship, asteroids, various types of lasers, and the boss. Each class is a Pygame `Sprite` and has its own `update` and `draw` methods.

## `Ship(sprite.Sprite)`

The `Ship` class represents the player's ship. It handles player input for movement and shooting, manages health, and tracks upgrades.

### Attributes
- `pos`, `vel`, `speed`: Control the ship's position and movement.
- `max_hp`, `hp`: Manage the ship's health.
- `cooldown`, `shoot_delay`: Control the firing rate of the main weapon.
- `max_bomb_cooldown`, `bomb_cooldown`: Control the firing rate of the bomb.
- `laser_vel`, `laser_dmg`, `multishot`, `shrapnel`, `laser_exp`, `sin_lasers`, `angular_lasers`, `ray_bomb`: These attributes represent the various upgrades the player can acquire.
- `color`, `is_rainbow`, `hue`, `rainbow_color`: Control the visual appearance of the ship.

### Methods
- `update()`: Reads player input, updates the ship's position, handles shooting, and manages particle effects for the ship's trail. It also checks for boundary collisions.
- `draw(screen)`: Draws the ship on the screen. The ship's color and appearance can change based on the upgrades collected.

```python
class Ship(sprite.Sprite):
    def __init__(self):
        super().__init__()
        # ... (attributes omitted for brevity) ...

    def update(self):
        # ... (implementation omitted for brevity) ...

    def draw(self, screen):
        # ... (implementation omitted for brevity) ...
```

## `Asteroid(sprite.Sprite)`

The `Asteroid` class represents the basic enemies in the game. They spawn at the top of the screen and move downwards.

### Attributes
- `pos`, `speed`: Control the asteroid's position and movement.
- `hp`: The health of the asteroid, which scales with the number of bosses killed.

### Methods
- `update()`: Updates the asteroid's position and removes it if it goes off-screen.
- `draw(screen)`: Draws the asteroid as a rectangle.

```python
class Asteroid(sprite.Sprite):
    def __init__(self):
        super().__init__()
        # ... (attributes omitted for brevity) ...

    def update(self):
        self.pos.y += self.speed
        if self.pos.y > config.H + 100:
            self.kill()
        self.rect.center = self.pos

    def draw(self, screen):
        pygame.draw.rect(screen, (255, 255, 255), self.rect, 10, 15)
```

## `Laser(sprite.Sprite)`

This is the base class for all laser projectiles. It's a simple `Sprite` with a position and velocity.

```python
class Laser(sprite.Sprite):
    def __init__(self, pos, vel):
        super().__init__()
        self.pos = Vector2(pos)
        self.vel = vel
        self.timer = 0
        self.color = (255, 255, 255)
```

## `MainLaser(Laser)`

The `MainLaser` is the player's primary weapon. It's a straight-moving projectile. Its appearance can change based on the `angular_lasers` upgrade.

```python
class MainLaser(Laser):
    def __init__(self, pos, vel):
        super().__init__(pos, vel)
        # ...

    def update(self):
        # ...

    def draw(self, screen):
        # ...
```

## `Bomb(Laser)`

The `Bomb` is a special weapon that explodes into shrapnel or rays. It moves slowly and then explodes when its velocity drops below a threshold.

### `explode()`
This method creates the explosion, spawning a number of `Shrapnel` or `Ray` projectiles, depending on whether the `ray_bomb` upgrade is active.

```python
class Bomb(Laser):
    def __init__(self, pos, vel):
        super().__init__(pos, vel)
        # ...

    def update(self):
        # ...

    def explode(self):
        # ...

    def draw(self, screen):
        # ...
```

## `Shrapnel(Laser)`

`Shrapnel` projectiles are created by the `Bomb`'s explosion. They are small, circular projectiles that move outwards from the explosion point.

```python
class Shrapnel(Laser):
    # ...
```

## `SinLaser(Laser)`

The `SinLaser` is a special type of laser that moves in a sine wave pattern. It's associated with the `sin_lasers` upgrade.

```python
class SinLaser(Laser):
    # ...
```

## `Ray(Laser)`

`Ray` projectiles are an alternative to `Shrapnel` for the `Bomb`'s explosion, enabled by the `ray_bomb` upgrade. They are instantaneous lines that deal damage in a straight line.

```python
class Ray(Laser):
    # ...
```

## `EnemyLaser(sprite.Sprite)`

This class represents the lasers fired by the boss. They are simple, downward-moving projectiles.

```python
class EnemyLaser(sprite.Sprite):
    # ...
```

## `Boss(sprite.Sprite)`

The `Boss` is a major enemy with multiple attack phases and a large amount of health.

### Attributes
- `pos`: The boss's position.
- `max_hp`, `hp`: The boss's health.
- `timer`, `phase`, `next_phase`: Control the boss's attack patterns and phase transitions.

### Methods
- `update()`: Manages the boss's movement and attacks based on its current phase. It also handles phase transitions.
- `draw(screen)`: Draws the boss on the screen, along with its health bar.

```python
class Boss(sprite.Sprite):
    def __init__(self):
        super().__init__()
        # ...

    def update(self):
        # ...

    def draw(self, screen):
        # ...
```
