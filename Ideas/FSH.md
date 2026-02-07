⚡ **SparkOS FileSystem Hierarchy (FSH)**
========================================

*A human‑centered, permission‑aware filesystem for clarity, safety, and predictability.*

1\. Goals
---------

SparkOS's filesystem is designed around a few core principles:

-   **Human‑readable** --- no cryptic legacy directories

-   **Predictable** --- everything has a clear purpose

-   **Safe by default** --- permissions protect users, not the other way around

-   **Self‑contained apps** --- no dependency spaghetti

-   **User autonomy** --- users can restrict even root inside their own space

-   **No hidden magic** --- everything is visible and understandable

SparkOS separates the world into two layers:

-   **RootFS** --- system‑level, protected, stable

-   **UserFS** --- user‑level, writable, personal, sandboxed

2\. RootFS (System-Level Filesystem)
====================================

RootFS is mounted read‑only (except for a few controlled areas). Even the **root user cannot create new top‑level directories**. Root can only write inside directories that explicitly allow it.

Anything with strict permissions **cannot be overridden by root**.

### **/Program**

For **non‑GUI programs** that don't appear in the launcher.

Structure:
```
/Program/<ProgramName>/<Version>/
    Main.exec      (required)
    Help.txt       (required)
    ...other files (optional)

```

Rules:

-   Programs are **self‑contained** --- everything they need lives in their folder

-   All users: **Read + Execute** (cannot be revoked, even by root)

-   Root: full access

SparkOS auto‑selects the **latest version** unless a specific version is requested.

### **/Application**

Same model as `/Program`, but for **GUI apps** that appear in the launcher.

Structure:
```
/Application/<AppName>/<Version>/
    Main.exec      (required)
    Icon.png       (required)
    Help.txt       (required)
    ...other files (optional)

```

Rules:

-   Appears in every user's launcher

-   All users: Read + Execute

-   Root: full access

### **/Targets/Libraries**

Holds system‑level libraries.

Structure:
```
/Targets/Libraries/<LibraryName>/<Version>/
    Dynamic/*.so
    Static/*.a

```

Rules:

-   Readable + linkable by all

-   Writable only by root

### **/Targets/Headers**

Same structure as `/Targets/Libraries`, but contains `.h` header files.

### **/System**

The core of SparkOS.

Contains:

-   kernel

-   system services

-   boot logic

-   runtime

-   UI system

-   everything required for the OS to function

Rules:

-   Mounted **read‑only**

-   No user (not even root) can modify it during normal operation

### **/Root**

Root's home directory.

Rules:

-   Only root can access it

-   The *folder itself* has strict permissions

-   Inside it, root can set any permissions

-   Internally, it mirrors the UserFS structure

### **/User**

Contains all user home directories.

Rules:

-   Anyone can *see* `/User/<username>`

-   The user who owns it can restrict access to anyone --- **including root**

-   UserFS lives inside each user's folder

3\. UserFS (User-Level Filesystem)
==================================

UserFS is where the user has **maximum autonomy**. Users can restrict root, apps, other users --- anyone.

### **/Home**

Primary workspace. User has full control.

### **/Space**

A dedicated area for **projects**. Same permissions as `/Home`.

### **/Program**

User‑installed non‑GUI programs.

Rules:

-   Only the user may read/write/execute

-   Others (including root) may read/execute **only if the user allows it**

### **/Application**

User‑installed GUI apps.

Structure:
```
Main.exec
Icon.png
Help.txt
...other files

```

Rules:

-   Same permissions as UserFS `/Program`

-   Appears in the user's launcher

### **/Store/Config**

App configuration files.

Rules:

-   Read/write by the user

-   Completely restricted from everyone else (including root)

### **/Store/Settings**

Same as `/Store/Config`, but for settings.

### **/Store/Age**

("Storage" pun --- clever.)

Used for:

-   media

-   large files

-   general storage

Same permissions as Config/Settings.

### **/Mount/Device**

Where physical devices (USB, etc.) are mounted.

Rules:

-   Readable only by the user

-   Writable by **no one**, not even the user

-   OS manages the mount interface

### **/Mount/FS**

Syncs a RootFS with another machine via SSH.

Rules:

-   Read/write only by the user

### **/Mount/Sync**

Same as `/Mount/FS`, but specifically for syncing files/folders to:

-   another device

-   cloud storage

### **/Public**

Files visible to everyone on the LAN.

Rules:

-   User can write

-   Everyone can read

### **/Media**

User's media library (images, videos, etc.)

4\. Philosophy Behind the Structure
===================================

SparkOS's FSH is built on a few core beliefs:

-   **Users deserve autonomy** --- even over root

-   **Programs should be self‑contained** --- no dependency hell

-   **Permissions should be explicit** --- no silent privilege escalation

-   **The filesystem should be readable** --- no `/etc`, `/usr`, `/var` clutter

-   **Safety should be the default** --- not an afterthought

-   **Everything should be reversible** --- no hidden state

This filesystem isn't just storage --- it's a reflection of SparkOS's values.
