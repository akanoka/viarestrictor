# ViaRestrictor

**Block certain Minecraft versions (via ViaVersion) and display a custom message to the player.**

---

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Configuration](#configuration)
* [Usage](#usage)
* [Development & Build](#development--build)
* [Contributing](#contributing)
* [License](#license)
* [Credits](#credits)

---

## Overview

ViaRestrictor is a Java plugin for Minecraft servers (Spigot/Paper) that relies on **ViaVersion** to detect a player's client version and, depending on the configuration, prevent connection and/or display a custom message.

It is useful for:

* Enforcing a range of allowed versions.
* Blocking versions known to cause problems or be used by unwanted clients.

---

## Features

* Define a list of **forbidden** versions or an **allowed** list.
* Customizable message sent to the player attempting to connect with an unauthorized version (supports Minecraft color codes `&`).
* Option to automatically `kick` the player.
* Integration that detects the presence of ViaVersion on startup.

---

## Prerequisites

* Java 17+ (or the recommended version for your server)
* Compatible Spigot/Paper server
* ViaVersion installed (required to correctly detect client versions)

---

## Installation

1. Download the JAR (`ViaRestrictor.jar`) from the GitHub releases or compile from source.
2. Place `ViaRestrictor.jar` in your server's `plugins/` folder.
3. Restart the server. The plugin will automatically create `plugins/ViaRestrictor/config.yml`.

---

## Configuration

A `config.yml` file is generated on the first startup. Here is a usable example:

```yaml
# ViaRestrictor configuration
# ========================================
# VersionRestrictor - Configuration
# ========================================

# List of allowed protocol numbers
# (These numbers correspond to Minecraft versions)
# See https://wiki.vg/Protocol_version_numbers for the complete reference
# Examples:
# 758: 1.20.0
# 759: 1.20.1
# 760: 1.20.2
# 761: 1.20.3
# 765: 1.20.4
# 764: 1.21.1
# 767: 1.21.4
allowed_versions:
- 759   # 1.20.1
- 765   # 1.20.4
- 764   # 1.21.1
- 767   # 1.21.4

# Message displayed to players whose version is not allowed
kick_message: | 
&cSorry, your Minecraft version is not allowed on this server. 
&7Detected version: &e%mcversion% (&f%version%&7)
&aCompatible versions:
&21.20.x &7and &21.21.4

# Log kicks to the console
log_kicks: true
```

**Important fields:**

* `mode`: `whitelist` (only listed versions are allowed).
* `versions`: array of strings representing client versions.
* `message`: message sent to the player (supports `\n` for line breaks and color codes `&`).
* `kick`: if `true`, the player is kicked after the message is displayed.
* `log-blocked`: if `true`, blocked attempts are logged to the console.

---

## Usage

* Modify `config.yml` according to your needs. * Restart the server or reload the configuration (if you implement a `/vr reload` command).
* Test the connection with different versions to verify the behavior.

### Commands (suggestions)

> The plugin does not necessarily include these commands by default — this is an API suggestion to implement.

```
/vr reload   # reloads the configuration
```

---

## Development & Build

### Recommended Structure

```
src/main/java/akanoka/viarestrictor/
- ViaRestrictor.java (main class extends JavaPlugin)
resources/
- plugin.yml
- config.yml (example)
```

### Build (Maven)

Example of a minimal `pom.xml`:

```xml
<!-- add Spigot/Paper and ViaVersion dependencies if needed -->
```

To compile:

```bash
mvn clean package
```

---

## Contributing

Contributions are welcome — issues, suggestions, and pull requests. Please follow these rules:

1. Fork and create a branch for each feature or bug.
2. Tests and compatibility with ViaVersion are appreciated.
3. Document any major changes in the README.

---

## License

This project is licensed under the **Apache License 2.0**. See the `LICENSE` file for the full text.

---

## Credits

* Maintained by AkaNoka
* Inspired by the needs of Paper/Spigot servers to maintain consistent client versions

---

*Last updated: October 31, 2025*
