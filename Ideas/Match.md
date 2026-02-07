# Match — The SparkOS Package Manager

## 1. Purpose
Match installs self-contained .match packages into either UserFS or RootFS, depending on user choice.

## 2. Package Format
.match files are ZIP archives with a strict structure:
- Main.exec (required)
- Help.txt (required)
- Icon.png (required for Applications)
- DependencyURLs.txt (optional)
- Other files (optional)

## 3. Install Locations
- User Install: installs into UserFS, no root required
- Root Install: installs into RootFS, requires superuser

## 4. Dependency Handling
Match reads DependencyURLs.txt and prompts the user for each dependency.

## 5. Philosophy
Match is explicit, predictable, and user-controlled.
