# Documentation for upgrades.py

This module defines the various upgrades that the player can acquire for their ship. It uses an abstract base class `UpgradeEffect` to define a common interface for all upgrades. Each specific upgrade is then implemented as a subclass of `UpgradeEffect`.

## `UpgradeEffect(ABC)`

This is the abstract base class for all upgrade effects. It defines two abstract methods that all subclasses must implement:

- `name`: A property that returns the name of the upgrade as a string.
- `apply(self, ship)`: A method that applies the upgrade's effect to the given `ship` object.

```python
from abc import ABC, abstractmethod

from blastroids import config


class UpgradeEffect(ABC):
    @property
    @abstractmethod
    def name(self):
        raise NotImplementedError

    @abstractmethod
    def apply(self, ship):
        raise NotImplementedError
```

## Concrete Upgrade Classes

The following classes are concrete implementations of `UpgradeEffect`, each providing a specific enhancement to the player's ship.

### `ShootSpeedUpgrade`
Increases the ship's firing speed by decreasing the `shoot_delay`.

### `LaserVelocityUpgrade`
Increases the velocity of the ship's lasers.

### `ShipRepairUpgrade`
Fully restores the ship's health.

### `ExplodingLasersUpgrade`
Increases the damage and explosion size of the ship's lasers.

### `MultishotUpgrade`
Increases the number of lasers fired at once.

### `MoreBombShrapnelUpgrade`
Increases the number of shrapnel fragments produced by bombs.

### `SinLasersUpgrade`
Grants the ship the ability to fire sine-wave lasers. This is a powerful upgrade that is only available after a certain number of bosses have been defeated.

### `AngularLasersUpgrade`
Grants the ship the ability to fire angular lasers. This is a powerful upgrade that is only available after a certain number of bosses have been defeated.

### `RayBombsUpgrade`
Changes the bomb's explosion from shrapnel to powerful rays. This is a powerful upgrade that is only available after a certain number of bosses have been defeated.

Here is the implementation of the concrete upgrade classes:

```python
class ShootSpeedUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "shoot speed"

    def apply(self, ship):
        if ship.shoot_delay >= 12:
            ship.shoot_delay -= 2


class LaserVelocityUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "laser velocity"

    def apply(self, ship):
        if ship.laser_vel.y >= -26:
            ship.laser_vel *= 1.5


class ShipRepairUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "ship repair"

    def apply(self, ship):
        ship.hp = ship.max_hp


class ExplodingLasersUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "exploding lasers"

    def apply(self, ship):
        if ship.laser_dmg < 2:
            ship.laser_dmg += 1
            ship.laser_exp += 1


class MultishotUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "multishot"

    def apply(self, ship):
        if ship.multishot < 2:
            ship.multishot += 1


class MoreBombShrapnelUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "more bomb shrapnel"

    def apply(self, ship):
        if ship.shrapnel < 48:
            ship.shrapnel += 4


class SinLasersUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "sin lasers"

    def apply(self, ship):
        if ship.bosses_killed >= config.lv_req:
            ship.sin_lasers = True


class AngularLasersUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "angular lasers"

    def apply(self, ship):
        if ship.bosses_killed >= config.lv_req:
            ship.angular_lasers = True


class RayBombsUpgrade(UpgradeEffect):
    @property
    def name(self):
        return "ray bombs"

    def apply(self, ship):
        if ship.bosses_killed >= config.lv_req:
            ship.ray_bomb = True
```
