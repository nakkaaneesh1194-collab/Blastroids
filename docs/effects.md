# Documentation for effects.py

This module contains classes for creating visual effects in the game, such as particle explosions and screen transitions. These effects are purely cosmetic and do not affect gameplay directly.

## `Pop(sprite.Sprite)`

The `Pop` class creates an expanding and then contracting circle effect, like a bubble popping. It's used for impacts and explosions.

### `__init__(self, x, y, r, c=(255, 255, 255))`
Initializes a `Pop` effect at position `(x, y)` with an initial radius `r` and color `c`.

### `update(self)`
Updates the `Pop` effect. The radius increases and then decreases. When the radius is less than or equal to 1, the effect is removed.

### `draw(self, screen)`
Draws the `Pop` effect on the screen as a circle.

```python
class Pop(sprite.Sprite):
    def __init__(self, x, y, r, c=(255, 255, 255)):
        super().__init__()
        self.radius = r
        self.pos = Vector2(x, y)
        self.color = c
        self.v = 5

    def update(self):
        self.radius += self.v
        self.v -= 1
        if self.radius <= 1:
            self.kill()

    def draw(self, screen):
        if self.radius > 0:
            pygame.draw.circle(
                screen,
                self.color,
                self.pos,
                int(self.radius),
                max(1, int(self.radius // 5)),
            )
```

## `Pow(sprite.Sprite)`

The `Pow` class creates a particle explosion effect. It spawns a number of small, decaying particles that move outwards from a central point.

### `__init__(self, x, y, m, c, d=Vector2(0, 0))`
Initializes a `Pow` particle at position `(x, y)`. `m` controls the initial velocity range, `c` is the color, and `d` is an optional initial direction vector.

### `update(self)`
Updates the particle's position and life. The particle fades over time and is removed when its life is depleted. Its velocity is also updated to create a dynamic effect.

### `draw(self, screen)`
Draws the `Pow` particle on the screen as a circle. The size of the circle is proportional to the particle's remaining life.

```python
class Pow(sprite.Sprite):
    def __init__(self, x, y, m, c, d=Vector2(0, 0)):
        super().__init__()
        self.pos = Vector2(x, y)
        if d == Vector2(0, 0):
            self.vel = Vector2(random.uniform(-m, m), random.uniform(-m, m))
        else:
            self.vel = d
        self.color = c
        self.life = 1.0
        self.decay = random.uniform(0.02, 0.05)
        self.size = random.randint(8, 16)
        self.direction = d

    def update(self):
        self.life -= self.decay
        if self.life <= 0:
            self.kill()
        self.pos += self.vel
        self.vel *= 0.95
        if self.direction == Vector2(0, 0):
            self.vel += Vector2(random.randint(-3, 3), random.randint(-3, 3))
        else:
            if self.direction.x != 0:
                self.vel.y += random.randint(-1, 1)
                if self.direction.y > 0:
                    self.vel.y += random.randint(0, 5)
                elif self.direction.y < 0:
                    self.vel.y += random.randint(-5, 0)
            elif self.direction.y != 0:
                self.vel.x += random.randint(-1, 1)
                if self.direction.x > 0:
                    self.vel.x += random.randint(0, 5)
                elif self.direction.x < 0:
                    self.vel.x += random.randint(-5, 0)

    def draw(self, screen):
        s = int(self.size * self.life)
        if s > 0:
            pygame.draw.circle(screen, self.color, self.pos, s)
```

## `ScreenEffect(sprite.Sprite)`

The `ScreenEffect` class creates a full-screen color overlay that can be used for fade-in/fade-out transitions.

### `__init__(self, color=(0, 0, 0), initial_alpha=0, fade_speed=0)`
Initializes a full-screen surface with a given `color`. `initial_alpha` sets the starting transparency, and `fade_speed` controls how quickly it fades in or out.

### `update(self)`
Updates the alpha (transparency) of the overlay. If the alpha becomes fully transparent (less than 0), the effect is removed.

### `draw(self, screen)`
Draws the overlay on the screen.

```python
class ScreenEffect(sprite.Sprite):
    def __init__(self, color=(0, 0, 0), initial_alpha=0, fade_speed=0):
        super().__init__()
        self.image = pygame.Surface((config.W, config.H))
        self.image.fill(color)
        self.alpha = initial_alpha
        self.fade_speed = fade_speed
        self.rect = self.image.get_rect()
        self.image.set_alpha(self.alpha)

    def update(self):
        self.alpha += self.fade_speed
        if self.alpha < 0:
            self.kill()
        elif self.alpha > 255:
            self.alpha = 255
        self.image.set_alpha(int(self.alpha))

    def draw(self, screen):
        screen.blit(self.image, self.rect)
```
