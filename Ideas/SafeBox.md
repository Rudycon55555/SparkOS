# SafeBox — The SparkOS User-Level Sandbox

## 1. Purpose
SafeBox is a user-space sandbox that intercepts:
- filesystem access
- network access
- I/O operations
- shell commands
- program execution
- inter-process communication

Everything requires explicit user permission.

## 2. Design Principles
- Runs as the user, not as root
- No system privileges required
- No hidden behavior
- Human-readable permission prompts
- Reversible decisions
- No silent escalation
- No global access unless granted

## 3. How SafeBox Works
- Wraps program execution
- Intercepts system calls at the user level
- Uses capability tokens
- Maintains a permission ledger
- Logs all access attempts

## 4. Permission Model
Each request must specify:
- what is being accessed
- why it is being accessed
- what the program wants to do with it

User can choose:
- Allow Once
- Allow Until Exit
- Allow Always
- Deny Once
- Deny Always

## 5. Filesystem Permissions
SafeBox mediates:
- read access
- write access
- execute access
- directory traversal
- metadata access

## 6. Network Permissions
SafeBox mediates:
- outbound connections
- inbound connections
- local sockets
- DNS queries

## 7. I/O Permissions
SafeBox mediates:
- camera
- microphone
- USB devices
- sensors
- clipboard
- screen capture

## 8. Shell & Execution Permissions
SafeBox mediates:
- launching programs
- spawning shells
- running scripts
- accessing environment variables

## 9. User Experience
- Clear, readable prompts
- No jargon
- No “mystery behavior”
- Logs are human-readable
- Decisions are reversible

## 10. Philosophy
SafeBox embodies SparkOS’s core belief:
**The user is the ultimate authority, not the system.**
