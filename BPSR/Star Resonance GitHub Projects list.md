A compilation of open-source projects for Star Resonance (Blue Protocol).
These tools cover DPS meters, overlays, bots, asset extraction, and community utilities.
Use responsibly and follow the game’s terms of service.

---

### Core DPS and Data Tools

**[dmlgzs/StarResonanceDamageCounter](https://github.com/dmlgzs/StarResonanceDamageCounter)**
- Real-time DPS and combat data sniffer for Star Resonance.
- Provides live damage statistics, DPS calculations, and detailed combat analysis.
- No modification of the game client is needed; relies on network packet capture.

**[anying1073/StarResonanceDps](https://github.com/anying1073/StarResonanceDps)**
- Based on StarResonanceDamageCounter for real-time DPS analysis.
- Tracks individual and team damage output during encounters.
- Helps optimize gameplay by understanding DPS distribution and critical hits.

**[balrogsxt/StarResonanceAPI](https://github.com/balrogsxt/StarResonanceAPI)**
- Exposes detailed API endpoints for enemies, players, and scene data.
- Provides JSON outputs for real-time combat statistics and mapping coordinates.
- Built on top of DamageCounter to allow integration with other tools or overlays.

**[Viemean/StarResonance.DPS](https://github.com/Viemean/StarResonance.DPS)**
- Lightweight frontend for displaying combat data from DamageCounter.
- Shows team DPS, healing, and skill usage in real-time floating windows.
- Supports multi-player monitoring and skill distribution analysis.
![应用截图](https://raw.githubusercontent.com/Viemean/StarResonance.DPS/refs/heads/master/Assets/img.png)

**[mrsnakke/BPSR-Meter](https://github.com/mrsnakke/BPSR-Meter)**
- English-language desktop DPS/HPS meter overlay.
- Displays total damage, healing, contributions, critical hit rate, and max DPS.
- Non-intrusive; overlays game window without affecting gameplay.
![Meter](https://raw.githubusercontent.com/mrsnakke/BPSR-Meter/master/portada.png)

**[tom228studio/StarResonanceDamageCounter-master](https://github.com/tom228studio/StarResonanceDamageCounter-master)**
- Localized Russian fork of the original DamageCounter.
- Provides combat analytics, DPS tracking, and skill statistics.
- Focuses on reliability in real-time battle scenarios with packet capture.

---

### Overlays and Frontends

**[ziqi-rgb/StarResonanceDamageCounter-overlay](https://github.com/ziqi-rgb/StarResonanceDamageCounter-overlay)**
- Displays live combat data in floating windows without capturing packets.
- Syncs with DamageCounter for DPS, team stats, and skill information.
- Supports nickname edits and multiple independent windows for characters.

**[CKylinMC/StarResonanceDamageCounterOverlay](https://github.com/CKylinMC/StarResonanceDamageCounterOverlay)**
- Tauri-based frontend for displaying DPS and combat data.
- Works as a companion to DamageCounter backend, visualizing real-time statistics.
- Lightweight and responsive with simple UI controls for team data tracking.
![StarResonanceDamageCounterOverlay](https://raw.githubusercontent.com/ckylinmc/StarResonanceDamageCounterOverlay/main/assets/scrshot.png)

**[tom228studio/StarResonanceDamageCounter-overlay-main](https://github.com/tom228studio/StarResonanceDamageCounter-overlay-main)**
- Russian version of DamageCounter overlay supporting team monitoring.
- Allows UI customization like transparency, colors, and nickname edits.
- Displays skill statistics, DPS contribution, and supports multi-window layouts.

**[Bluefissure/StarResonanceACTPlugin](https://github.com/Bluefissure/StarResonanceACTPlugin)**
- Plugin for Advanced Combat Tracker (ACT) integration.
- Captures encounters and overlays DPS, built from DamageCounter code.
- Still WIP, may have packet loss with VPNs or UDP connections.

**[ziqi-rgb/StarResonanceDamageCounter-overlay](https://github.com/ziqi-rgb/StarResonanceDamageCounter-overlay)**
- Floating window overlay for DamageCounter API data.
- Shows team metrics, skill usage, and individual tracking.
- Customizable window layout and nickname settings.

---

### Game Tools and Data Extraction

**[PotRooms/StarResonanceTool](https://github.com/PotRooms/StarResonanceTool)**
- Parses game asset files like Lua scripts, protobufs, and asset bundles.
- Useful for modding, research, and analyzing game resources.
- Supports extracting JSON tables and Unity asset bundles for offline analysis.

**[whbyaoi/star-resonance-profit-tool](https://github.com/whbyaoi/star-resonance-profit-tool)**
- Market and trade profit calculator for in-game items.
- Requires Windows and 1080p resolution for accurate UI reading.
- Tracks item prices, crafting efficiency, and profit over time.

**[c0derceejay/unofficial-blueprotocol-star-resonance](https://github.com/c0derceejay/unofficial-blueprotocol-star-resonance)**
- Community-driven build calculator and sharing platform.
- Supports all classes, gear, and skill setups with local browser storage.
- Provides real-time calculations and power rating for builds.

**[RayneClouds/rayneclouds.github.io](https://github.com/RayneClouds/rayneclouds.github.io)**
- Web-based talent tree visualizer for planning character skills.
- Supports multiple builds and shows detailed skill interactions.
- Designed for easy use in browsers without additional software.

**[Myazusa/star_resonance_library](https://github.com/Myazusa/star_resonance_library)**
- Offline library containing item info, crafting chains, and calculators.
- Includes profit calculators for association and resource management.
- Standalone executable, no installation required.

**[fudiyangjin/StarResonanceAutoMod](https://github.com/fudiyangjin/StarResonanceAutoMod)**
- Automates mod selection for characters using live network data.
- Optimizes attribute combinations with C++ algorithms.
- Supports CPU/GPU modes and provides scoring for recommended mods.

**[HuaChunOXO/StarResonanceModuleSolver](https://github.com/HuaChunOXO/StarResonanceModuleSolver)**
- Calculates optimal mod setups based on StarResonanceAutoMod.
- Supports filtering by attribute and class-specific scoring.
- Helps reduce trial-and-error in mod optimization.

---

### Bots and Automation

**[EricHongXDD/nonebot-star-resonance-plugin](https://github.com/EricHongXDD/nonebot-star-resonance-plugin)**
- NoneBot framework plugin for in-game interactions.
- Supports querying player info, daily tasks, and generating videos.
- Automates common Discord-like commands for Star Resonance communities.

**[azmiao/StarResonanceNotice](https://github.com/azmiao/StarResonanceNotice)**
- Bot plugin to provide notifications and event alerts.
- Integrates with YuiChyanBot and updates players on in-game schedules.
- Lightweight and easy to configure via JSON settings.
![StarResonanceNotice](https://raw.githubusercontent.com/azmiao/StarResonanceNotice/main/截图.jpg)

**[Xuan-cc/ShiroFisher-StarResonanceSmartFishing](https://github.com/Xuan-cc/ShiroFisher-StarResonanceSmartFishing)**
- Python script to automate fishing gameplay.
- Detects fish bites, casts rods, collects fish, and manages bait automatically.
- Uses OpenCV and PyAutoGUI for computer vision and input simulation.

**[exneverbur/StarResonanceChatSender](https://github.com/exneverbur/StarResonanceChatSender)**
- Automates sending long chat messages in-game.
- Splits text into segments and simulates typing at intervals.
- Useful for bulk messaging or roleplaying events.

**[xxfttkx/StarResonanceAutoSwitchLine](https://github.com/xxfttkx/StarResonanceAutoSwitchLine)**
- Automatically switches server channels to optimize farming.
- Reduces lag and competition for resources during peak hours.
- Simplifies routine gameplay navigation.

**[xxfttkx/StarResonanceEnemyCapture](https://github.com/xxfttkx/StarResonanceEnemyCapture)**
- Captures enemy data in real-time via packet analysis.
- Logs positions, HP, templates, and attack targets for research.
- Helps with encounter planning and enemy tracking.

**[PotRooms/StarResonanceData](https://github.com/PotRooms/StarResonanceData)**
- Parses game data for Blue Protocol: Star Resonance.
- Extracts combat, assets, and protocol information for analysis.
- Supports research and tool development using official game files.

**[xxfttkx/StarResonanceSimpleDamageDistribution](https://github.com/amoeet/StarResonanceSimpleDamageDistribution)**
- Plots incremental damage over time for team analysis.
- Visualizes DPS trends using DamageCounter logs.
- Does not capture live data; works with existing datasets.
![StarResonanceSimpleDamageDistribution](https://private-user-images.githubusercontent.com/69706187/474307262-935a6edc-f511-4d1f-93c7-4ad54dc26572.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjAzNjQzMDUsIm5iZiI6MTc2MDM2NDAwNSwicGF0aCI6Ii82OTcwNjE4Ny80NzQzMDcyNjItOTM1YTZlZGMtZjUxMS00ZDFmLTkzYzctNGFkNTRkYzI2NTcyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEwMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMDEzVDE0MDAwNVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJjNWFlYmJiNmIwYmZmZDAxN2FmZDRmYWE1YjdmOWUxODA0OTZkZGVmNTQwYTZkOWZlNWZkMmEyZjI1NzQzMTImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.2VMZFeDxLY6sgGUfXpn1dbneUZI-_VS8yJzDR9NR-tI)

**[xxfttkx/StarResonanceAutoHunt](https://github.com/xxfttkx/StarResonanceAutoHunt)**
- Automates hunting encounters in-game.
- Switches lines, starts battles, and logs loot efficiently.
- Designed to reduce repetitive manual actions during farming.
![StarResonanceAutoHunt](https://raw.githubusercontent.com/xxfttkx/StarResonanceAutoHunt/main/gui.png)

---

**[Remering/starresonance_battle_data_statistics](https://github.com/Remering/starresonance_battle_data_statistics)**
- Flutter-based project for battle data statistics in Star Resonance.
- Provides a starting point for building a mobile app with combat data visualization.
- Includes resources for Flutter development, tutorials, and sample code for beginners.

---

[**bk360/BPSR-AutoFisher-V1**](https://github.com/bk360/BPSR-AutoFisher-V1)
- An automated fishing assistant for *Blue Protocol: Star Resonance (BPSR)*.

* Allows the player to start fishing automatically through a simple Python script (`python main.py`).
* Detects when the fishing rod breaks and handles the “Continue Fishing” button automatically, reducing manual interaction.
* Designed for quick setup: navigate to the directory in CMD or PowerShell, and run the main file to begin automation.

---

[**Down98/BPSREngPatcher**](https://github.com/Down98/BPSREngPatcher)
- A patching utility to enable English language support for *Blue Protocol: Star Resonance*.

* Requires installation of the .NET 8.0 Runtime environment.
* Users must place the patcher executable inside the `AppData\LocalLow\bokura\Star\localsave` folder (where `localsave.bytes` exists).
* Once executed, it modifies local save data to enable English translation, simplifying localization for non-Chinese users.

---

[**narrow-gua/BpsrDetector**](https://github.com/narrow-gua/BpsrDetector)
- A C#-based packet-sniffing and detection tool for BPSR.

* Inspired by the original open-source project by Dimole (`StarResonanceDamageCounter`).
* Allows users to monitor BPSR data packets in real time, useful for developers and mod creators analyzing game behavior.
* Provides a foundation for future tools involving automated performance tracking and data capture.

---

[**fishing-dev-sm/bpsr_automodule**](https://github.com/fishing-dev-sm/bpsr_automodule)
- A comprehensive OCR-powered optimizer for *BPSR* modules and attributes.

* Uses Python, Flask, and OpenCV to analyze screenshots and optimize mod setups.
* Offers a modern Web UI for drag-and-drop image uploads, real-time recognition, and result visualization.
* Employs multi-strategy OCR recognition, multi-objective optimization, and intelligent filtering to find best attribute combinations.
* Cross-platform support for Windows, macOS, and Linux with one-click setup scripts and JSON result exports.
* Designed for players and theorycrafters seeking efficient builds without manually comparing modules.

---

[**scylian/bpsr-autofish**](https://github.com/scylian/bpsr-autofish)
- A computer vision automation framework tailored for BPSR’s fishing mechanics.

* Implements precise mouse and keyboard control combined with OpenCV-based image detection.
* Features advanced safety checks, fail-safe conditions, and coordinate validation for reliable automation.
* Provides modular Python APIs for easy integration into custom scripts, along with over 60 unit tests to ensure robustness.
* Can be adapted for other automation tasks beyond fishing through its extensible design.

---

[**fishing-dev-sm/bpsr_autofishing**](https://github.com/fishing-dev-sm/bpsr_autofishing)
- An improved, vision-based automatic fishing script for *Star Resonance*.

* Uses pixel color analysis (HSV/BGR) to detect bites, reel timing, and “fish escaped” conditions.
* Adapts to any 16:9 resolution with DPI scaling and automatic region-of-interest (ROI) adjustment.
* Features dynamic mode switching between fast tapping and stable long-press reeling based on color detection.
* Includes timeout protection, intelligent recovery (auto-refishing, reconnecting, or re-equipping rods), and detailed logging.
* Designed to run safely without altering game files or memory, ensuring non-invasive automation.

---

[**viemahc/bpsr-countdowns**](https://github.com/viemahc/bpsr-countdowns)
- An event countdown tracker for the *Blue Protocol: Star Resonance* community.

* Developed specifically for the STARWIND Guild to monitor upcoming in-game events.
* Displays real-time countdowns for limited-time content, raids, and special seasonal events.
* Helps guild members coordinate participation and stay synchronized with in-game schedules.

---

[**ArtjomsBogatirjovs/bpsr-fishing**](https://github.com/ArtjomsBogatirjovs/bpsr-fishing)
- An external auto-fishing system for *Star Resonance* based on image recognition.

* Interacts only through the visible game UI using Windows API calls—no game memory or file modifications.
* Uses AI-based fish detection and auto-casting for accurate and efficient fishing cycles.
* Compatible with all 16:9 resolutions in both fullscreen and windowed mode.
* Includes detailed troubleshooting for display settings, antivirus conflicts, and input remapping.
* Focused on safe, educational use—prohibits any commercial redistribution or paid automation services.

---

[**s-now25/bpsr-skyward-guide**](https://github.com/s-now25/bpsr-skyward-guide)
- A detailed gameplay guide focused on the *Skyward Wind Knight* class in *BPSR*.

* Covers builds, rotations, skill setups, and aerial combat strategies.
* Offers comprehensive explanations for wind-based mechanics and optimal stat balancing.
* Aimed at helping players maximize airborne combat performance through advanced techniques.

---

[**phiyuki/BPSR-translate**](https://github.com/phiyuki/BPSR-translate)
- A steganographic text encryption and translation utility for *Star Resonance*.

* Uses zero-width characters for invisible data embedding, ensuring secure, hidden message transfer.
* Every encryption produces unique randomized results, enhancing data obfuscation.
* Can be used online via the “星痕共鸣内鬼翻译器” for quick text conversions.
* Fully customizable with editable phrases, keys, salts, and versioning for personalized encryption schemes.

---

[**zaichiki/bpsr_guild_roster**](https://github.com/zaichiki/bpsr_guild_roster)
- A forked version of the *StarResonanceDamageCounter* with added guild management tools.

* Introduces a Guild Roster window for member tracking, data visualization, and performance analysis.
* Maintains all original DPS tracking and combat analytics while adding export and management features.
* Designed to promote transparency, collaboration, and fair play within guilds.
* Fully open-source under AGPL v3, with respect to community standards and non-invasive data collection.

---

[**MrDustyBowl/bpsr-skyward-wind-knight-guide-fr**](https://github.com/MrDustyBowl/bpsr-skyward-wind-knight-guide-fr)
- The French-language version of the *Skyward Wind Knight* comprehensive guide.

* Offers localized explanations of class builds, wind mechanics, and skill rotations.
* Helps French-speaking players master aerial combat strategies in *Blue Protocol: Star Resonance*.
* Maintains all content depth and quality of the English version with linguistic adaptation for clarity and accessibility.
