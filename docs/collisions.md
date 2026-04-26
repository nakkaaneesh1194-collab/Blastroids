# Documentation for collisions.py

This module handles all collision detection and responses in the game. It manages collisions between the player, asteroids, enemy lasers, and bosses.

## `create_impact(pos, color=(255, 255, 255), count=5)`

This function creates a visual effect of an impact at a given position. It adds a number of `Pow` particles and a single `Pop` particle to the respective particle groups.

```python
def create_impact(pos, color=(255, 255, 255), count=5):
    for _ in range(count):
        config.pows.add(effects.Pow(pos.x, pos.y, 10, color))
    config.pops.add(effects.Pop(pos.x, pos.y, 20, color))
```

## `handle_player_collisions()`

This function checks for and handles collisions between the player's ship and other game objects like asteroids and enemy lasers. When a collision occurs, it plays a hit sound, reduces the player's HP, and creates a screen shake effect and an impact visual effect.

```python
def handle_player_collisions():
    if config.ship.sprite:
        collided_asteroids = sprite.spritecollide(
            config.ship.sprite, config.asteroids, True
        )
        for _ in collided_asteroids:
            config.hit_sound.play()
            config.ship.sprite.hp -= 1
            config.screen_shake = 100
            create_impact(config.ship.sprite.pos, (255, 0, 0), 10)

        collided_enemy_lasers = sprite.spritecollide(
            config.ship.sprite, config.enemy_lasers, True
        )
        for _ in collided_enemy_lasers:
            config.hit_sound.play()
            config.ship.sprite.hp -= 1
            config.screen_shake = 100
            create_impact(config.ship.sprite.pos, (255, 255, 255), 10)
```

## `_on_asteroid_destroyed(ast, score)`

This is a helper function that is called when an asteroid is destroyed. It plays an explosion sound, removes the asteroid from the game, adds to the score, and has a chance to spawn an upgrade. It also creates a visual explosion effect.

```python
def _on_asteroid_destroyed(ast, score):
    config.boom_sound.play()
    ast.kill()
    config.screen_shake = 2
    score += 10
    if (
        config.ship.sprite
        and random.randint(1, 8 + config.ship.sprite.bosses_killed) == 1
    ):
        config.upgs.add(
            ui.Upgrade(
                random.randint(100, config.W - 100),
                random.randint(100, config.H - 100),
            )
        )
    config.pops.add(effects.Pop(ast.pos.x, ast.pos.y, 40, (255, 255, 255)))
    return score
```

## `_handle_asteroid_laser_collision(ast, laser, score)`

This function handles the collision between an asteroid and a laser. It creates an impact effect, applies damage to the asteroid, and if the asteroid's health drops to zero, it calls `_on_asteroid_destroyed`. Different laser types deal different amounts of damage.

```python
def _handle_asteroid_laser_collision(ast, laser, score):
    hit_color = laser.color

    create_impact(laser.pos, hit_color, 4)
    if config.ship.sprite:
        config.screen_shake = 1 + config.ship.sprite.laser_exp
    else:
        config.screen_shake = 1

    if config.ship.sprite:
        if isinstance(laser, entities.SinLaser):
            damage = config.ship.sprite.laser_dmg * 2
        elif isinstance(laser, entities.MainLaser) and config.ship.sprite.angular_lasers:
            damage = config.ship.sprite.laser_dmg * 2.5
        else:
            damage = config.ship.sprite.laser_dmg
    else:
        if isinstance(laser, entities.SinLaser):
            damage = 1.5
        else:
            damage = 1
    ast.hp -= damage

    ast.pos.y -= 1

    if config.ship.sprite and config.ship.sprite.laser_exp > 0:
        config.pops.add(
            effects.Pop(
                laser.pos.x, laser.pos.y, config.ship.sprite.laser_exp * 20, hit_color
            )
        )
        num_pows = (
            config.ship.sprite.laser_exp * 2
            if config.ship.sprite.laser_exp <= 4
            else 40
        )
        for _ in range(num_pows):
            config.pows.add(effects.Pow(laser.pos.x, laser.pos.y, 12, hit_color))

    if config.ship.sprite and isinstance(laser, entities.MainLaser):
        config.ship.sprite.bomb_cooldown = max(0, config.ship.sprite.bomb_cooldown - 1)

    if isinstance(laser, entities.Bomb):
        laser.explode()
    else:
        laser.kill()

    if ast.hp <= 0:
        score = _on_asteroid_destroyed(ast, score)
    return score
```

## `handle_asteroid_collisions(score)`

This function checks for collisions between asteroids and lasers. It uses `pygame.sprite.groupcollide` to find all such collisions and then calls `_handle_asteroid_laser_collision` for each one.

```python
def handle_asteroid_collisions(score):
    hits = sprite.groupcollide(config.asteroids, config.lasers, False, False)
    for ast, laser_list in hits.items():
        config.hit_sound.play()
        for laser in laser_list:
            score = _handle_asteroid_laser_collision(ast, laser, score)
    return score
```

## `_on_boss_destroyed(score)`

This helper function is called when a boss is destroyed. It creates a large screen shake effect, plays an explosion sound, awards a significant number of points, and spawns multiple upgrades. It also increments the `bosses_killed` counter.

```python
def _on_boss_destroyed(score):
    config.screen_shake = 30
    config.boom_sound.play()
    score += 100 * (config.boss_group.sprite.phase + 1)
    for _ in range(50):
        config.pows.add(
            effects.Pow(
                config.boss_group.sprite.pos.x,
                config.boss_group.sprite.pos.y,
                12,
                (255, 50, 50),
            )
        )
    for _ in range(3):
        config.upgs.add(
            ui.Upgrade(
                random.randint(100, config.W - 100),
                random.randint(100, config.H - 100),
            )
        )
    if config.ship.sprite:
        config.ship.sprite.bosses_killed += 1
    config.boss_group.sprite.kill()
    return score
```

## `_handle_boss_laser_collision(laser, score)`

This function handles the collision between a boss and a laser. It's similar to the asteroid-laser collision handler but tailored for bosses. It creates impact effects and applies damage to the boss. If the boss's health drops to zero, it calls `_on_boss_destroyed`.

```python
def _handle_boss_laser_collision(laser, score):
    if not config.boss_group.sprite:
        return score

    hit_color = laser.color

    create_impact(laser.pos, hit_color, 6)

    if config.ship.sprite and config.ship.sprite.laser_exp > 0:
        config.pops.add(
            effects.Pop(
                laser.pos.x, laser.pos.y, config.ship.sprite.laser_exp * 20, hit_color
            )
        )
        num_pows = (
            config.ship.sprite.laser_exp * 10
            if config.ship.sprite.laser_exp <= 4
            else 40
        )
        for _ in range(num_pows):
            config.pows.add(effects.Pow(laser.pos.x, laser.pos.y, 10, hit_color))

    if config.ship.sprite:
        if isinstance(laser, entities.SinLaser):
            damage = config.ship.sprite.laser_dmg * 2
        else:
            damage = config.ship.sprite.laser_dmg
    else:
        if isinstance(laser, entities.SinLaser):
            damage = 1.5
        else:
            damage = 1
    config.boss_group.sprite.hp -= damage

    config.hit_sound.play()
    if config.ship.sprite and isinstance(laser, entities.MainLaser):
        bomb_cooldown_reduction = (
            1 / ((config.ship.sprite.bosses_killed + 1) / 2)
            if config.ship.sprite
            else 1
        )
        config.ship.sprite.bomb_cooldown = max(
            0,
            config.ship.sprite.bomb_cooldown - bomb_cooldown_reduction,
        )

    if config.boss_group.sprite.hp <= 0:
        score = _on_boss_destroyed(score)
    return score
```

## `handle_boss_collisions(score)`

This function checks for collisions between the boss and the player's lasers. It uses `pygame.sprite.spritecollide` and then calls `_handle_boss_laser_collision` for each collision.

```python
def handle_boss_collisions(score):
    if config.boss_group.sprite:
        hits = sprite.spritecollide(config.boss_group.sprite, config.lasers, True)
        for laser in hits:
            score = _handle_boss_laser_collision(laser, score)
    return score
```
