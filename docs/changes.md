# Blastroids Change Log

This document provides a beginner-friendly explanation of the changes made to the Blastroids project, commit by commit.

## Commit: `5782f85af02ddff1f2530d9fc9edcb01c65e1d32` - Clean up repository

### Changes

This commit removed the `.venv` directory from the project's repository. The `.venv` directory is a virtual environment that contains all of the Python packages and dependencies needed to run the project.

### Justification

Virtual environments should not be included in a project's repository for several reasons:

*   **They are specific to your machine:** A virtual environment is tied to the specific operating system and Python version you are using. If another developer tries to use your virtual environment on their machine, it may not work correctly.
*   **They can be very large:** Virtual environments can contain hundreds of files and take up a lot of space. Including them in the repository makes it larger and slower to download.
*   **They are not necessary:** The `pyproject.toml` file already lists all of the project's dependencies. Other developers can use this file to create their own virtual environments.

By removing the `.venv` directory from the repository, we make the project smaller, more portable, and easier for other developers to work with.

## Commit: `3bd9040137d442ed80f3ff1b9b98d14b1210f752` - Fixed bugs and completed major refactoring

### Changes

This commit was a major refactoring of the entire codebase. The main changes include:

*   **Project Structure:** The project was reorganized into a proper Python package with a `src` layout. This means that all of the game's code is now located in the `src/blastroids` directory.
*   **Modularization:** The code was broken down into several modules, each with a specific responsibility:
    *   `main.py`: The main entry point of the game.
    *   `config.py`: Contains all of the game's configuration variables.
    *   `entities.py`: Contains the classes for all of the game's objects (e.g., the ship, asteroids, lasers).
    *   `effects.py`: Contains classes for visual effects like explosions and screen shake.
    *   `ui.py`: Contains classes for user interface elements like buttons and bars.
*   **Bug Fixes:** Several bugs were fixed, including issues with laser collisions and the upgrade system.

### Justification

This refactoring was a critical step in making the Blastroids project more maintainable and easier to work on. Here's why these changes were so important:

*   **Improved Organization:** The new project structure and modularization make the code much easier to navigate and understand. It's now clear where to find the code for each part of the game.
*   **Reduced Complexity:** By breaking the code down into smaller, more focused modules, we've reduced the overall complexity of the project. This makes it easier to fix bugs and add new features without breaking existing code.
*   **Increased Reusability:** The new modular design makes it easier to reuse code in other parts of the project or even in other projects.
*   **Better Collaboration:** A well-organized project is much easier for multiple developers to work on at the same time.

While this refactoring was a lot of work, it was a necessary step to ensure the long-term health and success of the Blastroids project.

## Commit: `fc76e1e841d0fcfbae0e13c0dc882d7de419550a` - refactor: Overhaul project structure and simplify codebase

### Changes

This commit was another major refactoring that built upon the previous one. It introduced a number of improvements to the project's structure, readability, and maintainability.

*   **Modularization:** The `main.py` file was further decomposed into smaller, more focused modules, including `collisions.py` to handle all collision-handling logic.
*   **Complexity Reduction:** The main `play` function was refactored, reducing its complexity significantly. The main game loop was broken down into `handle_input`, `update_game_state`, `handle_collisions`, and `render_screen` functions.
*   **Polymorphism:** The `Laser` class was refactored to use a polymorphic design, with a base `Laser` class and subclasses for each laser type. This eliminated a large number of `if/elif/else` statements.
*   **Code Readability & Conventions:** Class and variable names were corrected to adhere to Python's standard naming conventions. Ternary operators and inline conditionals were replaced with more explicit `if-else` blocks.
*   **Project Configuration:** A `pyproject.toml` file was added to define project metadata and dependencies. A `.gitignore` file was also included to exclude virtual environments and other unnecessary files from version control.
*   **Bug Fixes & Gameplay Adjustments:** Asset pathing issues were fixed by using absolute paths, the game was set to launch in fullscreen mode by default, and the player's ship hitbox was reduced by 20% to improve gameplay.

### Justification

This refactoring was another important step in making the Blastroids project more professional and easier to work on. Here's why these changes were so important:

*   **Improved Organization:** The new modules and functions make the code even easier to navigate and understand.
*   **Reduced Complexity:** The reduction in complexity of the `play` function makes it much easier to debug and add new features.
*   **Improved Readability:** The new naming conventions and the removal of ternary operators make the code more readable and easier to understand for new developers.
*   **Better Collaboration:** The `pyproject.toml` and `.gitignore` files make it much easier for other developers to contribute to the project.
*   **Improved Gameplay:** The bug fixes and gameplay adjustments make the game more fun and enjoyable to play.

## Commit: `33af1da270951ffe8682c1824cd8bd1f00e0a8a3` - style: Format code with ruff and fix linting issues

### Changes

This commit focused on improving the code's style and quality. The main changes include:

*   **Code Formatting:** The entire codebase was formatted using a tool called `ruff`. This ensures that the code has a consistent and clean style.
*   **Linting:** `ruff` was also used to identify and fix linting issues, such as unused imports and other non-compliant code.
*   **Print Statement Removal:** All "hello world" print statements were removed from the codebase.

### Justification

Code style and quality are important for a number of reasons:

*   **Readability:** Consistent formatting and a lack of linting errors make the code much easier to read and understand.
*   **Maintainability:** Clean code is much easier to maintain and debug.
*   **Collaboration:** When all developers on a project follow the same code style, it makes it much easier to collaborate and review each other's code.

By using a tool like `ruff` to automatically format and lint the code, we can ensure that the codebase remains clean and consistent over time.

## Commit: `a20e66af7338311f6da049504f0e095b8781bc5b` - feat: Refactor codebase for simplicity, readability, and maintainability

### Changes

This commit was the culmination of all the previous refactoring efforts. It introduced a number of final touches to the codebase to make it as clean, readable, and maintainable as possible.

*   **Polymorphic Upgrade System:** The `Upgrade` class was refactored to use a polymorphic approach for applying upgrades. This was achieved by creating an `UpgradeEffect` base class and a series of subclasses, each representing a specific upgrade. This makes the system more modular, extensible, and easier to maintain.
*   **Absolute Imports:** All relative imports were converted to absolute imports to improve maintainability and clarity.
*   **Module-based Imports:** All import statements were refactored to be module-based, improving namespacing and code clarity.
*   **`.gitignore` Update:** The `.venv/` directory was added to the `.gitignore` file to prevent the virtual environment from being tracked by git.

### Justification

This final refactoring was the last step in transforming the Blastroids project into a professional, maintainable, and extensible codebase. Here's why these changes were so important:

*   **Improved Extensibility:** The new polymorphic `Upgrade` system makes it much easier to add new upgrades to the game without having to modify the existing code.
*   **Improved Readability:** The new absolute and module-based imports make it much easier to see where each function and class is coming from.
*   **Improved Maintainability:** The new `.gitignore` file ensures that the repository remains clean and free of unnecessary files.

With this final refactoring, the Blastroids project is now in a great state to be further developed and expanded upon.
