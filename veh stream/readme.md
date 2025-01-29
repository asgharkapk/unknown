# SA-MP/Open.MP Vehicle Streamer Filterscript

![GitHub License](https://img.shields.io/github/license/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript?style=flat-square)
![GitHub Stars](https://img.shields.io/github/stars/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript?style=flat-square)
![GitHub Forks](https://img.shields.io/github/forks/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript?style=flat-square)
![GitHub Issues](https://img.shields.io/github/issues/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript?style=flat-square)
![GitHub Last Commit](https://img.shields.io/github/last-commit/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript?style=flat-square)

---

## Table of Contents

1. [About the Project](#about-the-project)
2. [Features](#features)
3. [Built With](#built-with)
4. [Installation](#installation)
5. [Configuration](#configuration)
6. [Usage](#usage)
7. [Roadmap](#roadmap)
8. [Contributing](#contributing)
9. [License](#license)
10. [Credits](#credits)
11. [Support](#support)

---

## About the Project

This project is a highly optimized and customizable **vehicle streamer filterscript** for **SA-MP (San Andreas Multiplayer)** and **Open.MP** servers. It dynamically streams vehicles based on player proximity, reducing server load by only spawning vehicles when players are nearby. The script supports a wide range of vehicle properties, including mods, colors, health, and more.

The goal of this project is to provide server owners with a lightweight and efficient solution for managing large numbers of vehicles without compromising server performance.

---

## Features

- **Dynamic Vehicle Streaming**: Vehicles are only spawned when players are within a specified range, reducing server load.
- **Customizable Configuration**: Easily configure the maximum number of vehicles, stream distance, respawn delay, and more.
- **Vehicle Properties**: Supports vehicle mods, colors, health, license plates, and other properties.
- **Efficient Resource Management**: Uses timers and loops to efficiently manage vehicle spawning and despawning.
- **Debug Mode**: Enable debug logging to monitor vehicle streaming behavior in real-time.
- **Randomization**: Functions to randomize vehicle colors and license plates for variety.

---

## Built With

- **[Pawn Language](https://www.compuphase.com/pawn/pawn.htm)**: The scripting language used for SA-MP/Open.MP.
- **[open.mp SDK](https://www.open.mp/)**: The multiplayer mod for GTA San Andreas, providing enhanced scripting capabilities.
- **[YSI Library](https://github.com/pawn-lang/YSI-Includes)**: A collection of useful Pawn libraries for SA-MP/Open.MP development.
- **[crashdetect Plugin](https://github.com/Zeex/samp-plugin-crashdetect)**: A plugin for debugging runtime errors in Pawn scripts.

---

## Installation

1. **Download the Script**: Clone or download the repository to your server's `filterscripts` directory.
   ```bash
   git clone https://github.com/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript.git
   ```

2. **Compile the Script**: Use the Pawn compiler to compile the script into an `.amx` file.
   ```bash
   pawncc vehicle_streamer.pwn
   ```

3. **Add to Server Configuration**: Add the compiled `.amx` file to your `server.cfg` under `filterscripts`.
   ```plaintext
   filterscripts vehicle_streamer
   ```

4. **Configure the Script**: Modify the constants at the top of the script to suit your server's needs (e.g., `MAX_STREAM_VEHICLES`, `VEHICLE_STREAM_DISTANCE`, etc.).

5. **Start the Server**: Launch your SA-MP or Open.MP server, and the vehicle streamer will be active.

---

## Configuration

The script is highly configurable through the following constants:

- **`MAX_STREAM_VEHICLES`**: Maximum number of vehicles that can be streamed (default: `3000`).
- **`VEHICLE_STREAM_DISTANCE`**: Distance at which vehicles are streamed to players (default: `100` units).
- **`RESPAWN_DELAY`**: Delay in seconds before a vehicle respawns (default: `20000` seconds).
- **`VEHICLE_SEEKER_DELAY`**: Delay between vehicle streaming checks (default: `100` milliseconds).
- **`MAX_ALLOWED_MODS`**: Maximum number of mods a vehicle can have (default: `12`).

---

## Usage

### Creating a Streamed Vehicle
Use the `CreateStreamVehicle` function to add a vehicle to the streamer:
```pawn
CreateStreamVehicle(modelid, Float:spawnX, Float:spawnY, Float:spawnZ, Float:angle, colour1, colour2, respawnDelay = RESPAWN_DELAY, bool:addSiren = true, Stream_Distance = VEHICLE_STREAM_DISTANCE);
```

Example:
```pawn
CreateStreamVehicle(411, 0.0, 0.0, 3.0, 90.0, 1, 2); // Creates an Infernus at the specified location.
```

### Randomizing Vehicle Colors
Use the `RandomColor` function to randomize a vehicle's primary and secondary colors:
```pawn
RandomColor(vehicleid);
```

### Randomizing License Plates
Use the `RandomPlate` function to assign a random license plate to a vehicle:
```pawn
RandomPlate(vehicleid);
```

### Destroying a Vehicle
Use the `DestroyTheVehicle` function to remove a vehicle from the game world:
```pawn
DestroyTheVehicle(vehicleid);
```

### Respawning a Vehicle
Use the `RespawnTheVehicle` function to reset a vehicle's position and state:
```pawn
RespawnTheVehicle(vehicleid);
```

---

## Roadmap

- **v1.0.0 (Current)**
  - Dynamic vehicle streaming based on player proximity.
  - Support for vehicle mods, colors, health, and license plates.
  - Debug mode for real-time monitoring.

- **v1.1.0 (Planned)**
  - Add support for vehicle attachments (e.g., nitro, hydraulics).
  - Improve performance for large numbers of vehicles.
  - Add more randomization options (e.g., random mods, paint jobs).

- **v1.2.0 (Future)**
  - Integrate with MySQL for persistent vehicle data.
  - Add admin commands for managing vehicles in-game.
  - Support for custom vehicle models.

---

## Contributing

Contributions are welcome! If you have suggestions, bug reports, or feature requests, please open an issue or submit a pull request.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Credits

- **Kingvornex**: Original author and maintainer.
- **SA-MP/Open.MP Community**: For inspiration and support.

---

## Support

If you encounter any issues or have questions, feel free to reach out via the [GitHub Issues](https://github.com/Kingvornex/SA-MP-Open.MP-Vehicle-Streamer-FilterScript/issues) page.

---

Enjoy streaming vehicles efficiently on your SA-MP/Open.MP server! 🚗💨
