# uBlue-os Homebrew Tap Copilot Instructions

This repository is the official staging area for uBlue-os Homebrew casks and formulas, focusing on Linux desktop applications.

## Repository Overview

- **Role**: Provides applications that are better managed in userspace (like IDEs) rather than baked into the OS image.
- **Goal**: Test Linux casks with the intent of upstreaming them or deleting them once better alternatives (like Flatpaks) exist.

## Structure

- `Formula/`: Homebrew formula definitions (Ruby).
- `Casks/`: Homebrew cask definitions (Ruby).
- `*.Brewfile`: Collections of packages for specific use cases (IDE, common, artwork).

## Key Context7 Libraries

- `/homebrew/brew`: Internal Homebrew logic.
- `/homebrew/homebrew-cask`: Cask DSL and management.
- `/ublue-os/packages`: uBlue-os custom RPM package definitions and strategies.

## Workflow

1. **Cask naming**: Follow `-linux` suffix conventions if applicable to distinguish from macOS variants.
2. **Testing**: Casks should be verified against the target uBlue images (Bluefin, Aurora, Bazzite).
3. **Contribution**: This is a production-level tap for uBlue-os. Use conventional commits.

## Common Tasks

- Adding a new CLI tool: Create a file in `Formula/`.
- Adding a GUI app: Create a file in `Casks/`.
- Updating collection: Edit the relevant `.Brewfile`.

## 🛠️ Local Development & Debugging

When testing casks or formulas locally before pushing, you can add the repository as a local tap.

### 1. Add Local Tap
To link your working folder to Homebrew:
```bash
brew tap local/test $(pwd)
```
This allows you to install packages directly from your local filesystem.

### 2. Managing Conflicts
If a package exists in multiple taps (e.g., `local/test` and `ublue-os/tap`), Homebrew will return an ambiguity error. Always use the **fully qualified name** to avoid tracking issues:

```bash
# RECOMMENDED: Use qualified names for everything
brew install ublue-os/tap/antigravity-linux
brew install local/test/antigravity-linux
```

> [!IMPORTANT]
> Homebrew links the installed package to the source Tap. If you install from `local/test`, you will not be able to remove the tap (`untap`) without first uninstalling the packages linked to it.

### 3. Reverting / Removing Local Tap
To remove the local tap without errors, follow this order:

1. **Uninstall** the packages that were installed via `local/test`:
   ```bash
   brew uninstall --cask local/test/[cask-name]
   ```
2. **Untap** the local repository:
   ```bash
   brew untap local/test
   ```
3. **Reinstall** (if necessary) the official version:
   ```bash
   brew install --cask ublue-os/tap/[cask-name]
   ```

### 4. Sync Tip
If you changed files in the tap and Homebrew doesn't seem to "see" the changes, try forcing an update or reinstalling using the qualified name.
