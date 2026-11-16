# PhunKeys

**Vehicle Ownership, Key Transmutation & Access Control for Project Zomboid Build 42**

Developed by **Raven**

## Overview

PhunKeys extends Project Zomboid's vehicle and key systems to support vehicle ownership, trading, and access control in multiplayer environments.

Built around UburGeek's Phun mod ecosystem, PhunKeys allows players to convert eligible vehicles into trade-in keys, create new vehicle keys through transmutation, and protect vehicles from unauthorized interactions.

The project focuses on extending existing game functionality while preserving as much vanilla vehicle behavior as possible.

## Features

### Vehicle Transmutation

Players can use the Transmutation Box to convert eligible vehicles into Vehicle Seller Keys for integration with a server economy.

The server validates the request before processing the conversion, including:

- Ownership of the required box and matching vehicle key
- Vehicle proximity and matching key identification
- Absence of vehicle occupants
- Empty vehicle storage
- Required item and interaction conditions

After validation, the system consumes the original key, produces the trade-in item, and removes the vehicle from the world.

### Vehicle Access Control

PhunKeys introduces vehicle protection and guest-access states.

- Protect a vehicle using the appropriate key.
- Restrict unauthorized vehicle interactions.
- Grant or revoke guest access.
- Preserve vanilla interaction behavior for authorized users.
- Apply protection checks to supported vehicle and mechanics actions.

Protection state is stored using vehicle-part ModData rather than a separate ownership registry.

### Vehicle Key Creation

A building key can be transmuted into a matching vehicle key when the required conditions are satisfied.

The implementation includes server-side checks for proximity, inventory contents, vehicle identity, and lock state.

The key-creation process includes rollback handling for failed local inventory mutations.

## Technical Architecture

The mod is organized into client, server, and shared Lua components.

| Component | Responsibility |
|---|---|
| `PhunKeys_Client.lua` | Player interactions, menus, and client/server requests |
| `PhunKeys_Mechanics.lua` | Vehicle and mechanics interface integration |
| `PhunKeys_Server.lua` | Server-side command handling and validation |
| `PhunKeys_Access.lua` | Vehicle protection and authorization rules |
| `PhunKeys_Actions.lua` | Timed-action validation and interaction guards |
| `PhunKeys_Shared.lua` | Shared identifiers, configuration, and utilities |

### API Integration and Hooks

PhunKeys extends existing Project Zomboid functionality rather than attempting to replace the underlying vehicle system.

The implementation investigates and integrates with:

- Vehicle objects and key identifiers
- Player and container inventories
- Vehicle-part ModData
- Vanilla vehicle and mechanics interfaces
- Timed-action lifecycle methods
- Client/server command events

Where appropriate, existing game functions are wrapped with additional authorization checks while retaining their original execution paths.

### Multiplayer and Validation

PhunKeys separates client interaction from server-side authority.

Clients initiate requests; the server independently checks the relevant inventory and vehicle state before executing supported operations.

Defensive measures include:

- Server-side validation of client requests
- Key and vehicle identity matching
- Interaction-distance checks
- Occupancy and inventory restrictions
- Revalidation of supported timed actions
- Diagnostic logging
- Rollback handling for selected inventory operations

These safeguards are designed to reduce invalid operations and unintended state changes. They do not constitute a formal security audit or guarantee protection against every modified-client exploit.

## Development Methodology

**PhunKeys was developed through an AI-assisted software development workflow directed by Raven.**

My primary responsibilities included:

1. Defining the intended gameplay behavior and functional requirements.
2. Investigating existing APIs, hooks, and game-engine behavior.
3. Designing how the new functionality should integrate with vanilla systems.
4. Directing AI-assisted implementation and revisions.
5. Testing results in the game environment.
6. Examining logs and runtime behavior to identify failures.
7. Refining requirements and implementation decisions based on observed results.

I do not represent this project as independently handwritten Lua software.

My principal contributions are requirements design, systems-level reasoning, API investigation, integration decisions, testing, debugging, and evaluation of AI-generated implementations.

## Known Limitations and Areas for Improvement

PhunKeys remains subject to the limitations of the underlying game engine and modding APIs.

Areas for further evaluation include:

- Atomicity of vehicle removal and trade-in item creation
- Failure recovery during multi-stage inventory operations
- Multiplayer synchronization under unusual disconnect conditions
- Compatibility with mods that replace vanilla vehicle methods
- Security testing against deliberately modified clients

These are meaningful engineering considerations, particularly for systems involving persistent multiplayer state and an in-game economy.

## Requirements

- Project Zomboid Build 42
- PhunMart 2 (`phunmart2`), declared as a dependency in the mod metadata

Compatibility may vary across Build 42 updates and other vehicle-related modifications.

## Installation

Install the mod using the supported Project Zomboid mod or Workshop installation process.

Enable PhunKeys and its required dependency. Multiplayer servers should load the relevant mod files for their Build 42 environment.

## Credits

**Project direction, feature design, integration research, testing, and AI-assisted development:** Raven

**Phun ecosystem:** UburGeek and the respective mod authors

**Game:** Project Zomboid by The Indie Stone

---

*PhunKeys is an independent modding project and is not affiliated with or endorsed by The Indie Stone.*
