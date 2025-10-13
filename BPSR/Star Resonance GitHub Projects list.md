A compilation of open-source projects for Star Resonance (Blue Protocol).
These tools cover DPS meters, overlays, bots, asset extraction, and community utilities.
Use responsibly and follow the game’s terms of service.

---

### Core DPS and Data Tools

**[[dmlgzs/StarResonanceDamageCounter](https://github.com/dmlgzs/StarResonanceDamageCounter)](https://github.com/dmlgzs/StarResonanceDamageCounter)**
Real-time DPS and combat data sniffer for Star Resonance.
Provides live damage statistics, DPS calculations, and detailed combat analysis.
No modification of the game client is needed; relies on network packet capture.

**[[anying1073/StarResonanceDps](https://github.com/anying1073/StarResonanceDps)](https://github.com/anying1073/StarResonanceDps)**
Based on StarResonanceDamageCounter for real-time DPS analysis.
Tracks individual and team damage output during encounters.
Helps optimize gameplay by understanding DPS distribution and critical hits.

**[[balrogsxt/StarResonanceAPI](https://github.com/balrogsxt/StarResonanceAPI)](https://github.com/balrogsxt/StarResonanceAPI)**
Exposes detailed API endpoints for enemies, players, and scene data.
Provides JSON outputs for real-time combat statistics and mapping coordinates.
Built on top of DamageCounter to allow integration with other tools or overlays.

**[[Viemean/StarResonance.DPS](https://github.com/Viemean/StarResonance.DPS)](https://github.com/Viemean/StarResonance.DPS)**
Lightweight frontend for displaying combat data from DamageCounter.
Shows team DPS, healing, and skill usage in real-time floating windows.
Supports multi-player monitoring and skill distribution analysis.

**[[mrsnakke/BPSR-Meter](https://github.com/mrsnakke/BPSR-Meter)](https://github.com/mrsnakke/BPSR-Meter)**
English-language desktop DPS/HPS meter overlay.
Displays total damage, healing, contributions, critical hit rate, and max DPS.
Non-intrusive; overlays game window without affecting gameplay.

**[[tom228studio/StarResonanceDamageCounter-master](https://github.com/tom228studio/StarResonanceDamageCounter-master)](https://github.com/tom228studio/StarResonanceDamageCounter-master)**
Localized Russian fork of the original DamageCounter.
Provides combat analytics, DPS tracking, and skill statistics.
Focuses on reliability in real-time battle scenarios with packet capture.

---

### Overlays and Frontends

**[[ziqi-rgb/StarResonanceDamageCounter-overlay](https://github.com/ziqi-rgb/StarResonanceDamageCounter-overlay)](https://github.com/ziqi-rgb/StarResonanceDamageCounter-overlay)**
Displays live combat data in floating windows without capturing packets.
Syncs with DamageCounter for DPS, team stats, and skill information.
Supports nickname edits and multiple independent windows for characters.

**[[CKylinMC/StarResonanceDamageCounterOverlay](https://github.com/CKylinMC/StarResonanceDamageCounterOverlay)](https://github.com/CKylinMC/StarResonanceDamageCounterOverlay)**
Tauri-based frontend for displaying DPS and combat data.
Works as a companion to DamageCounter backend, visualizing real-time statistics.
Lightweight and responsive with simple UI controls for team data tracking.

**[[tom228studio/StarResonanceDamageCounter-overlay-main](https://github.com/tom228studio/StarResonanceDamageCounter-overlay-main)](https://github.com/tom228studio/StarResonanceDamageCounter-overlay-main)**
Russian version of DamageCounter overlay supporting team monitoring.
Allows UI customization like transparency, colors, and nickname edits.
Displays skill statistics, DPS contribution, and supports multi-window layouts.

**[[Bluefissure/StarResonanceACTPlugin](https://github.com/Bluefissure/StarResonanceACTPlugin)](https://github.com/Bluefissure/StarResonanceACTPlugin)**
Plugin for Advanced Combat Tracker (ACT) integration.
Captures encounters and overlays DPS, built from DamageCounter code.
Still WIP, may have packet loss with VPNs or UDP connections.

**[[ziqi-rgb/StarResonanceDamageCounter-overlay](https://github.com/ziqi-rgb/StarResonanceDamageCounter-overlay)](https://github.com/ziqi-rgb/StarResonanceDamageCounter-overlay)**
Floating window overlay for DamageCounter API data.
Shows team metrics, skill usage, and individual tracking.
Customizable window layout and nickname settings.

---

### Game Tools and Data Extraction

**[[PotRooms/StarResonanceTool](https://github.com/PotRooms/StarResonanceTool)](https://github.com/PotRooms/StarResonanceTool)**
Parses game asset files like Lua scripts, protobufs, and asset bundles.
Useful for modding, research, and analyzing game resources.
Supports extracting JSON tables and Unity asset bundles for offline analysis.

**[[whbyaoi/star-resonance-profit-tool](https://github.com/whbyaoi/star-resonance-profit-tool)](https://github.com/whbyaoi/star-resonance-profit-tool)**
Market and trade profit calculator for in-game items.
Requires Windows and 1080p resolution for accurate UI reading.
Tracks item prices, crafting efficiency, and profit over time.

**[[c0derceejay/unofficial-blueprotocol-star-resonance](https://github.com/c0derceejay/unofficial-blueprotocol-star-resonance)](https://github.com/c0derceejay/unofficial-blueprotocol-star-resonance)**
Community-driven build calculator and sharing platform.
Supports all classes, gear, and skill setups with local browser storage.
Provides real-time calculations and power rating for builds.

**[[RayneClouds/rayneclouds.github.io](https://github.com/RayneClouds/rayneclouds.github.io)](https://github.com/RayneClouds/rayneclouds.github.io)**
Web-based talent tree visualizer for planning character skills.
Supports multiple builds and shows detailed skill interactions.
Designed for easy use in browsers without additional software.

**[[Myazusa/star_resonance_library](https://github.com/Myazusa/star_resonance_library)](https://github.com/Myazusa/star_resonance_library)**
Offline library containing item info, crafting chains, and calculators.
Includes profit calculators for association and resource management.
Standalone executable, no installation required.

**[[fudiyangjin/StarResonanceAutoMod](https://github.com/fudiyangjin/StarResonanceAutoMod)](https://github.com/fudiyangjin/StarResonanceAutoMod)**
Automates mod selection for characters using live network data.
Optimizes attribute combinations with C++ algorithms.
Supports CPU/GPU modes and provides scoring for recommended mods.

**[[HuaChunOXO/StarResonanceModuleSolver](https://github.com/HuaChunOXO/StarResonanceModuleSolver)](https://github.com/HuaChunOXO/StarResonanceModuleSolver)**
Calculates optimal mod setups based on StarResonanceAutoMod.
Supports filtering by attribute and class-specific scoring.
Helps reduce trial-and-error in mod optimization.

---

### Bots and Automation

**[[EricHongXDD/nonebot-star-resonance-plugin](https://github.com/EricHongXDD/nonebot-star-resonance-plugin)](https://github.com/EricHongXDD/nonebot-star-resonance-plugin)**
NoneBot framework plugin for in-game interactions.
Supports querying player info, daily tasks, and generating videos.
Automates common Discord-like commands for Star Resonance communities.

**[[azmiao/StarResonanceNotice](https://github.com/azmiao/StarResonanceNotice)](https://github.com/azmiao/StarResonanceNotice)**
Bot plugin to provide notifications and event alerts.
Integrates with YuiChyanBot and updates players on in-game schedules.
Lightweight and easy to configure via JSON settings.

**[[Xuan-cc/ShiroFisher-StarResonanceSmartFishing](https://github.com/Xuan-cc/ShiroFisher-StarResonanceSmartFishing)](https://github.com/Xuan-cc/ShiroFisher-StarResonanceSmartFishing)**
Python script to automate fishing gameplay.
Detects fish bites, casts rods, collects fish, and manages bait automatically.
Uses OpenCV and PyAutoGUI for computer vision and input simulation.

**[[exneverbur/StarResonanceChatSender](https://github.com/exneverbur/StarResonanceChatSender)](https://github.com/exneverbur/StarResonanceChatSender)**
Automates sending long chat messages in-game.
Splits text into segments and simulates typing at intervals.
Useful for bulk messaging or roleplaying events.

**[[xxfttkx/StarResonanceAutoSwitchLine](https://github.com/xxfttkx/StarResonanceAutoSwitchLine)](https://github.com/xxfttkx/StarResonanceAutoSwitchLine)**
Automatically switches server channels to optimize farming.
Reduces lag and competition for resources during peak hours.
Simplifies routine gameplay navigation.

**[[xxfttkx/StarResonanceEnemyCapture](https://github.com/xxfttkx/StarResonanceEnemyCapture)](https://github.com/xxfttkx/StarResonanceEnemyCapture)**
Captures enemy data in real-time via packet analysis.
Logs positions, HP, templates, and attack targets for research.
Helps with encounter planning and enemy tracking.

**[[PotRooms/StarResonanceData](https://github.com/PotRooms/StarResonanceData)](https://github.com/PotRooms/StarResonanceData)**
Parses game data for Blue Protocol: Star Resonance.
Extracts combat, assets, and protocol information for analysis.
Supports research and tool development using official game files.

**[[xxfttkx/StarResonanceSimpleDamageDistribution](https://github.com/amoeet/StarResonanceSimpleDamageDistribution)](https://github.com/amoeet/StarResonanceSimpleDamageDistribution)**
Plots incremental damage over time for team analysis.
Visualizes DPS trends using DamageCounter logs.
Does not capture live data; works with existing datasets.

**[[xxfttkx/StarResonanceAutoHunt](https://github.com/xxfttkx/StarResonanceAutoHunt)](https://github.com/xxfttkx/StarResonanceAutoHunt)**
Automates hunting encounters in-game.
Switches lines, starts battles, and logs loot efficiently.
Designed to reduce repetitive manual actions during farming.

---

**[[Remering/starresonance_battle_data_statistics](https://github.com/Remering/starresonance_battle_data_statistics)](https://github.com/Remering/starresonance_battle_data_statistics)**
Flutter-based project for battle data statistics in Star Resonance.
Provides a starting point for building a mobile app with combat data visualization.
Includes resources for Flutter development, tutorials, and sample code for beginners.
