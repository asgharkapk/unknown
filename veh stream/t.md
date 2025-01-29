how to implement crashdetect plugin in this:
script code:
// Stream Vehicles from file
#include <open.mp>
#include <crashdetect>
#include <YSI-Includes\YSI_Data\y_iterate>
// Define constants for configuration.
#define MAX_STREAM_VEHICLES 3000  // Maximum number of streamable vehicles.
/*
// Declare the iterator for vehicles.
new Iterator:VehicleIter<MAX_STREAM_VEHICLES>;
*/
#define VEHICLE_STREAM_DISTANCE 100  // Distance at which vehicles are streamed to players.
#define RESPAWN_DELAY 20000  // Vehicle respawn delay in seconds (30 minutes).
#define VEHICLE_SEEKER_DELAY 100  // Delay between vehicle streaming checks (in milliseconds).
#define MAX_ALLOWED_MODS 12
// Macro to simplify creating loops.
#define Loop(%0,%1) for(new %0 = 0; %0 < %1; %0++)
// Debugging flag.
new bool:VSD = false; // Enable/disable debug logging.
// Enumeration for vehicle properties stored in the streamable vehicle array.
enum Stream_vehicle
{
	vModel,         // Vehicle model ID.
	vID,            // Vehicle ID (unique in-game ID).
	vColor1,        // Primary color of the vehicle.
	vColor2,        // Secondary color of the vehicle.
	Float:vSpawnX,  // Vehicle spawn X coordinate.
	Float:vSpawnY,  // Vehicle spawn Y coordinate.
	Float:vSpawnZ,  // Vehicle spawn Z coordinate.
	Float:vSpawnA,  // Vehicle spawn rotation angle.
	Float:vLastX,   // Vehicle's last X position.
	Float:vLastY,   // Vehicle's last Y position.
	Float:vLastZ,   // Vehicle's last Z position.
	Float:vLastA,   // Vehicle's last rotation angle.
	vPlate[32],     // Vehicle's license plate.
	vPaintJob,      // Vehicle's paint job.
	bool:FirstSpawn,// Indicates if the vehicle is being spawned for the first time.
	bool:IsSpawned, // Indicates if the vehicle is currently spawned in the game world.
	bool:IsActive,  // Indicates if the vehicle is active (in range of players).
	bool:IsPopulated, // Indicates if the vehicle is populated with data.
	bool:IsOccupied, // Indicates if the vehicle is Ocupied with data.
	vInterior,      // Interior ID for the vehicle.
	vWorld,         // Virtual world ID for the vehicle.
	vStream,        // Stream distance for the vehicle.
	vSiren,         // Indicates if the vehicle has a siren.
	vRespawn,		// Respawn delay for the vehicle.
	vMods[12], 		// Array to store Vehicle Mods associated with the vehicle.      
	vPlayerID[9],	// Array to store player IDs associated with the vehicle. Bus has 9 seats.
	Float:vHealth,   // Vehicle health.
VEHICLE_PANEL_STATUS:panels,
        VEHICLE_DOOR_STATUS:doors,
        VEHICLE_LIGHT_STATUS:lights,
        VEHICLE_TYRE_STATUS:tyres,
TrailerID,
CabID,
DriverID,
LastDriver,
LANDING_GEAR_STATE:state,
Float:rightX,//GetVehicleMatrix
    Float:rightY,
    Float:rightZ,
    Float:upX,
    Float:upY,
    Float:upZ,
    Float:atX,
    Float:atY,
    Float:atZ,
//https://www.open.mp/docs/scripting/resources/vehicleinformationtypes
    bool:engine, bool:lights, bool:alarm, bool:doors, bool:bonnet, bool:boot, bool:objective,
seats,
Float:w,//GetVehicleRotationQuat
    Float:x,
    Float:y,
    Float:z
//https://www.open.mp/docs/scripting/functions/GetVehicleParamsCarWindows
//https://www.open.mp/docs/scripting/functions/GetVehicleParamsCarDoors
//https://www.open.mp/docs/scripting/functions/GetVehicleParamsSirenState
}
new vInfo[MAX_STREAM_VEHICLES][Stream_vehicle]; // Array to store vehicle properties.
new GetvMods[MAX_STREAM_VEHICLES][12];
// Variables to keep track of vehicle data.
new lastsvid = 0; // Last streamable vehicle ID.
new total_vehicles_from_files = 0; // Total vehicles loaded from file.
new Seekingid; // ID for the seeker timer.
//new ActiveVehicles[MAX_PLAYERS][MAX_STREAM_VEHICLES];
// Forward declaration for the VehicleSeeker function.
forward VehicleSeeker();
// Start the vehicle seeker, which streams vehicles near players.
stock StartVehicleSeeking()
{
	Seekingid = SetTimer("VehicleSeeker", VEHICLE_SEEKER_DELAY, true); // Set a repeating timer for the seeker.
	if(VSD == true) { printf("[Vehicle Streamer] [DEBUG]: Seekingid (%d) = SetTimer(VehicleSeeker, VEHICLE_SEEKER_DELAY (%d), true);", Seekingid, VEHICLE_SEEKER_DELAY); }
	return 1;
}
// Stop the vehicle seeker by killing the timer.
stock StopVehicleSeeking()
{
	KillTimer(Seekingid); // Kill the seeker timer.
	if(VSD == true) { printf("[Vehicle Streamer] [DEBUG]: KillTimer(Seekingid (%d));", Seekingid); }
	return 1;
}
new Respawnerid; // ID for the Respawner timer.
// Forward declaration for the VehicleRespawner function.
forward VehicleRespawner();
// Start the vehicle Respawner, which changes vehicles locations back to spawn locations.
stock StartVehicleRespawner()
{
	Respawnerid = SetTimer("VehicleRespawner", RESPAWN_DELAY, true); // Set a repeating timer for the Respanwer.
	if(VSD == true) { printf("[Vehicle Streamer] [DEBUG]: Respawnerid (%d) = SetTimer(VehicleRespawner, RESPAWN_DELAY (%d), true);", Respawnerid, RESPAWN_DELAY); }
	return 1;
}
// Stop the vehicle Respawner by killing the timer.
stock StopVehicleRespawner()
{
	KillTimer(Respawnerid); // Kill the Respawner timer.
	if(VSD == true) { printf("[Vehicle Streamer] [DEBUG]: KillTimer(VehicleRespawner: Respawnerid (%d));", Respawnerid); }
	return 1;
}
public VehicleRespawner()
{
	//foreach (new vehicleid : VehicleIter)
	Loop(vs, MAX_STREAM_VEHICLES)
	{
		if (vs < 0 || vs >= MAX_STREAM_VEHICLES) continue;
		if (vInfo[vs][IsPopulated] == false) continue;  // Skip unpopulated vehicles
		if (vInfo[vs][IsActive] == true) continue;
		vInfo[vs][vLastX] = vInfo[vs][vSpawnX];
		vInfo[vs][vLastY] = vInfo[vs][vSpawnY];
		vInfo[vs][vLastZ] = vInfo[vs][vSpawnZ];
		vInfo[vs][vLastA] = vInfo[vs][vSpawnA];
		vInfo[vs][FirstSpawn] = true;
	}
}
// Check if a vehicle is in range of any player.
stock bool:IsInRangeOfAnyPlayer(vv)
{
	foreach(new i : Player) // Iterate through all players.
	{
		// Validate the player ID and check if the player is connected
//        if (i < 0 || i >= MAX_PLAYERS || !IsPlayerConnected(i) || IsPlayerNPC(i)) continue;

		new Float:posx, Float:posy, Float:posz; // Player position variables.
		GetPlayerPos(i, posx, posy, posz); // Get the player's position.
		new Float:tempposx, Float:tempposy, Float:tempposz; // Temporary variables for position differences.
		tempposx = (vInfo[vv][vLastX] - posx); // X difference.
        tempposy = (vInfo[vv][vLastY] - posy); // Y difference.
        tempposz = (vInfo[vv][vLastZ] - posz); // Z difference.

		new radi = vInfo[vv][vStream]; // Stream distance for the vehicle.
        if( ((tempposx < radi) && (tempposx > -radi)) && ((tempposy < radi) && (tempposy > -radi)) && ((tempposz < radi) && (tempposz > -radi)) )
        {
			if(VSD == true) { printf("IsInRangeOfAnyPlayer(PID: %d, SVID: %d, VID: %d) = true", i, vv, vInfo[vv][vID]); }
            return true; // Vehicle is within range of a player.
        }
    }
    return false; // Vehicle is not in range of any player.
}
// Vehicle seeker function, which checks all vehicles and spawns or destroys them based on proximity to players
public VehicleSeeker()
{
	//foreach (new vehicleid : VehicleIter)
    Loop(v, MAX_STREAM_VEHICLES)  // Loop through all vehicles
    {
		if (v < 0 || v >= MAX_STREAM_VEHICLES) continue;
		if (vInfo[v][IsPopulated] == false) continue;  // Skip unpopulated vehicles
		
		// Cache the result of proximity check
        new bool:isInRange = IsInRangeOfAnyPlayer(v);
        // If vehicle is not spawned and a player is in range, create the vehicle
        if (vInfo[v][IsSpawned] == false && isInRange == true)
        {
            if(VSD == true) { printf("[DEBUG] Spawning vehicle: StreamID: %d, VehicleID: %d", v, vInfo[v][vID]); }
            CreateTheVehicle(v);
            vInfo[v][IsActive] = true;  // Mark vehicle as active
        }
		// If the vehicle is already spawned
        if (vInfo[v][IsSpawned] == true)
        {
            // If no player is in range, destroy the vehicle
            if (isInRange == false)
            {
                if(VSD == true) { printf("[DEBUG] Destroying vehicle: StreamID: %d, VehicleID: %d", v, vInfo[v][vID]); }
                DestroyTheVehicle(v);
                vInfo[v][IsActive] = false;  // Mark vehicle as inactive
            }
            else  // If a player is in range, update vehicle properties
            {
                if(VSD == true) { printf("[DEBUG] Updating vehicle: StreamID: %d, VehicleID: %d", v, vInfo[v][vID]); }

                // Update the vehicle's position, angle, and health
                GetVehiclePos(vInfo[v][vID], vInfo[v][vLastX], vInfo[v][vLastY], vInfo[v][vLastZ]);
                GetVehicleZAngle(vInfo[v][vID], vInfo[v][vLastA]);
                GetVehicleHealth(vInfo[v][vID], vInfo[v][vHealth]);

                vInfo[v][IsActive] = true;  // Mark vehicle as active again
            }
        }
    }
    return 1;
}
// Function to create a vehicle at a specific location
stock CreateStreamVehicle(modelid, Float:spawnX, Float:spawnY, Float:spawnZ, Float:angle, colour1, colour2, respawnDelay = RESPAWN_DELAY, bool:addSiren = true, Stream_Distance = VEHICLE_STREAM_DISTANCE)
{
	if(lastsvid > MAX_STREAM_VEHICLES) return printf("[Vehicle Streamer]: %d > %d", lastsvid, MAX_STREAM_VEHICLES);   
	// Assign vehicle data to the VehicleInfo array
	vInfo[lastsvid][vModel] = modelid;
	vInfo[lastsvid][vColor1] = colour1;
	vInfo[lastsvid][vColor2] = colour2;
	vInfo[lastsvid][vSpawnX] = spawnX;
	vInfo[lastsvid][vSpawnY] = spawnY;
	vInfo[lastsvid][vSpawnZ] = spawnZ;
	vInfo[lastsvid][vSpawnA] = angle;
	vInfo[lastsvid][vLastX] = spawnX;
	vInfo[lastsvid][vLastY] = spawnY;
	vInfo[lastsvid][vLastZ] = spawnZ;
	vInfo[lastsvid][vLastA] = angle;

	vInfo[lastsvid][FirstSpawn] = true;
	vInfo[lastsvid][IsSpawned] = false;
	vInfo[lastsvid][IsActive] = false;
	vInfo[lastsvid][vHealth] = 1000;
	vInfo[lastsvid][vID] = INVALID_VEHICLE_ID;
	vInfo[lastsvid][IsPopulated] = true;
	vInfo[lastsvid][vStream] = Stream_Distance;
	vInfo[lastsvid][vSiren] = addSiren;
	vInfo[lastsvid][vRespawn] = respawnDelay;
	if(VSD == true) { printf("CreateStreamVehicle(%d, %f, %f, %f, %f, %d, %d, %d, %d, bool:addSiren, lastsvid: %d)", 
       vInfo[lastsvid][vModel], 
       vInfo[lastsvid][vSpawnX], 
       vInfo[lastsvid][vSpawnY], 
       vInfo[lastsvid][vSpawnZ], 
       vInfo[lastsvid][vSpawnA], 
       vInfo[lastsvid][vColor1], 
       vInfo[lastsvid][vColor2], 
       vInfo[lastsvid][vRespawn], 
       vInfo[lastsvid][vStream], 
       lastsvid);
	}	
	lastsvid++; // Increment the last stream vehicle ID.
	//Iter_Add(VehicleIter, i);
	return lastsvid;
}
// Function to actually create the vehicle in the game world
stock CreateTheVehicle(vv)
{
    // Check if the vehicle is already spawned
    if(vInfo[vv][IsSpawned] == true) 
        return printf("[DEBUG] [Vehicle Streamer] Vehicle already Spawed!!! (VehicleID: %d)", vInfo[vv][vID]); 
    // Create the vehicle and store its ID
    vInfo[vv][vID] = CreateVehicle(
        vInfo[vv][vModel],      // Vehicle model
        vInfo[vv][vLastX],      // Last known X position
        vInfo[vv][vLastY],      // Last known Y position
        vInfo[vv][vLastZ],      // Last known Z position
        vInfo[vv][vLastA],      // Last known angle/rotation
        vInfo[vv][vColor1],     // Primary color
        vInfo[vv][vColor2],     // Secondary color
        vInfo[vv][vRespawn],    // Respawn state
        bool:vInfo[vv][vSiren]  // Siren enabled/disabled
    );
    // Update vehicle metadata
    vInfo[vv][FirstSpawn] = false;  // Mark vehicle as not being spawned for the first time
    vInfo[vv][IsSpawned] = true;    // Mark vehicle as spawned

    // Set the health of the vehicle
    SetVehicleHealth(vInfo[vv][vID], vInfo[vv][vHealth]);

	for(new m = 0; m < 12; m++)
	{
		if(GetvMods[vv][m] > 0)
		{
			AddVehicleComponent(vv, GetvMods[vv][m]);
		}
	}
    // Debug logging if VSD is enabled
    if(VSD == true)
    { 
        // Log vehicle creation details
        printf("CreateTheVehicle(VehicleID: %d, %d, %.0f, %.0f, %.0f, %.0f, %d, %d, %d, %d, %d)", 
            vInfo[vv][vID],         // Vehicle ID
            vInfo[vv][vModel],      // Vehicle model
            vInfo[vv][vLastX],      // Last X position
            vInfo[vv][vLastY],      // Last Y position
            vInfo[vv][vLastZ],      // Last Z position
            vInfo[vv][vLastA],      // Last angle/rotation
            vInfo[vv][vColor1],     // Primary color
            vInfo[vv][vColor2],     // Secondary color
            vInfo[vv][vRespawn],    // Respawn state
            vInfo[vv][vStream],     // Streaming state
            lastsvid                      // Last server-side ID (custom field)
        ); 
    }

    // Return the newly created vehicle's ID
    return vInfo[vv][vID];
}
stock DestroyTheVehicle(vv)
{
    // Check if the VSD flag is enabled
    if(VSD == true) 
    { 
        // Log the details of the vehicle being destroyed
        // Format specifiers:
        // %d - Integer values (Vehicle ID, Model, Color1, Color2, Respawn, Stream, Lastsvid)
        // %.0f - Floating-point values rounded to 0 decimal places (LastX, LastY, LastZ, LastA)
        printf("DestroyVehicle(VehicleID: %d, %d, %.0f, %.0f, %.0f, %.0f, %d, %d, %d, %d, %d)", 
            vInfo[vv][vID],         // Vehicle ID
            vInfo[vv][vModel],      // Vehicle Model
            vInfo[vv][vLastX],      // Last X position of the vehicle
            vInfo[vv][vLastY],      // Last Y position of the vehicle
            vInfo[vv][vLastZ],      // Last Z position of the vehicle
            vInfo[vv][vLastA],      // Last A (angle/rotation) of the vehicle
            vInfo[vv][vColor1],     // Primary color of the vehicle
            vInfo[vv][vColor2],     // Secondary color of the vehicle
            vInfo[vv][vRespawn],    // Respawn state of the vehicle
            vInfo[vv][vStream],     // Streaming state of the vehicle
            lastsvid                      // Last server-side ID (custom field)
        ); 
    }
    // Destroy the vehicle with the given Vehicle ID
    DestroyVehicle(vInfo[vv][vID]);
    // Update vehicle information to reflect its destruction
    vInfo[vv][FirstSpawn] = false;  // Mark the vehicle as no longer first spawned
    vInfo[vv][IsSpawned] = false;   // Mark the vehicle as not spawned
    vInfo[vv][vID] = INVALID_VEHICLE_ID;  // Set the vehicle ID to invalid
    vInfo[vv][IsActive] = false;    // Mark the vehicle as inactive
}
stock RespawnTheVehicle(vv)
{
	vInfo[vv][vLastX] = vInfo[vv][vSpawnX];
	vInfo[vv][vLastY] = vInfo[vv][vSpawnY];
	vInfo[vv][vLastZ] = vInfo[vv][vSpawnZ];
	vInfo[vv][vLastA] = vInfo[vv][vSpawnA];
}
/*
stock RandomVehicleMods(vehicleid)
{
    // Define possible mods (example mod IDs, replace with actual mod IDs).
    new possibleMods[] = {1000, 1001, 1002, 1003, 1004, 1005, 1006, 1007, 1008, 1009, 1010, 1011};
    new numPossibleMods = sizeof(possibleMods);

    // Assign random mods to the vehicle.
    for (new i = 0; i < MAX_ALLOWED_MODS; i++)
    {
        new randomMod = possibleMods[random(numPossibleMods)];
        AddVehicleComponent(vehicleid, randomMod);
    }
}
*/
stock RandomColor(svid)
{
	new randnum1 = 0 + random(10);
	new randnum2 = 0 + random(10);
	switch (randnum1)
	{
		case 0:
		{
			vInfo[svid][vColor1] = 1;
		}
		case 1:
		{
			vInfo[svid][vColor1] = 130;
		}
		case 2:
		{
			vInfo[svid][vColor1] = 131;
		}
		case 3:
		{
			vInfo[svid][vColor1] = 132;
		}
		case 4:
		{
			vInfo[svid][vColor1] = 146;
		}
		case 5:
		{
			vInfo[svid][vColor1] = 147;
		}
		case 6:
		{
			vInfo[svid][vColor1] = 148;
		}
		case 7:
		{
			vInfo[svid][vColor1] = 151;
		}
		case 8:
		{
			vInfo[svid][vColor1] = 6;
		}
		case 9:
		{
			vInfo[svid][vColor1] = 128;
		}
		case 10:
		{
			vInfo[svid][vColor1] = 0;
		}
	}
	switch (randnum2)
	{
		case 0:
		{
			vInfo[svid][vColor2] = 1;
		}
		case 1:
		{
			vInfo[svid][vColor2] = 130;
		}
		case 2:
		{
			vInfo[svid][vColor2] = 131;
		}
		case 3:
		{
			vInfo[svid][vColor2] = 132;
		}
		case 4:
		{
			vInfo[svid][vColor2] = 146;
		}
		case 5:
		{
			vInfo[svid][vColor2] = 147;
		}
		case 6:
		{
			vInfo[svid][vColor2] = 148;
		}
		case 7:
		{
			vInfo[svid][vColor2] = 151;
		}
		case 8:
		{
			vInfo[svid][vColor2] = 6;
		}
		case 9:
		{
			vInfo[svid][vColor2] = 128;
		}
		case 10:
		{
			vInfo[svid][vColor2] = 0;
		}
	}
}
stock RandomPlate(svid)
{
	new randnumb = 1 + random(9);
	new strrr[32];
    format(strrr, sizeof(strrr), "OPEN MP %d", randnumb);
	vInfo[svid][vPlate] = strrr;
    SetVehicleNumberPlate(svid, vInfo[svid][vPlate]);
//	SetVehicleToRespawn(svid);
}
crashdetect include:
[CrashDetect plugin][github]
============================

[![Version][version_badge]][version]
[![Build Status][build_status]][build]

CrashDetect helps you debug runtime errors and server crashes. When something
goes wrong you will get a detailed error message with error description, stack
trace, and other useful information that will make it easier to quickly find
and fix the issue.

Installation
------------

1. Download a binary package from the [Releases][download] page on Github or
   build it yourself from source code (see
   [Building from source code](#building-from-source-code)).
2. Extract and copy `crashdetect.so` or `crashdetect.dll` to `<sever>/plugins/`.
3. Add `crashdetect` (Windows) or `crashdetect.so` (Linux) to the `plugins`
   line of your server.cfg.

Binary packages come with an include file (`crashdetect.inc`) that contains
some helper functions that you may find useful. But **you don't have to
include** it to be able to use CrashDetect.

Usage
-----

Apart from installing the plugin you don't have to do anything further to
start receiving errors reports. By default all errors will be saved in your
`server_log.txt`, but this can be changed
(see [Configuration](#configuration)).

For better debugging experience, make sure that you
[compile your script with debug info enabled][debug_info]. Doing this will let
you see more information in stack traces such as function names, parameter names
and values, source file names and line numbers.

Please be aware that when using this plugin your code WILL run slower due
to the overhead associated with detecting errors and providing accurate
error information (for example, some runtime optimizations are disabled).
Usually this is fine during development, but it's not recommended to load
CrashDetect on a production (live) server with many players.

Configuration
-------------

CrashDetect reads settings from server.cfg, the server configuration file. This
is done during plugin loading, so if you change any settings you will probably
need to restart your server.

Available settings:

* `trace <flags>`

  Enables function call tracing.

  If enabled, CrashDetect will show information about every function call in
  all running scripts, such as the name of the function being called and the
  values of its parameters.

  `flags` may be one or combination of the following:

  * `n` - trace native functions
  * `p` - trace public functions
  * `f` - trace normal functions (i.e. all non-public functions)

  For example, `trace pn` will trace both public and native calls, and
  `trace pfn` will trace all functions.

* `trace_filter <regexp>`

  Filters `trace` output based on a regular expression.

  Examples:

  * `trace_filter Player` - output functions whose name contains `Player`
  * `trace_filter playerid=0` - show functions whose `playerid` parameter is 0

* `crashdetect_log <filename>`

  Use a custom log file for output.

  By default all diagnostic information is printed to the server log. This
  option lets you redirect output to a separate file.

* `long_call_time <us>`

  How long a top-level callback call should last before CrashDetect prints a
  warning. This can be set very high (for example `1000000`) to only detect
  functions that have totally hung, or very low (`500`) to detect functions that
  complete, but are just slow (thus affecting overall server execution and
  sync). Default value is `5000` (5 milliseconds).

  Use `0` to disable this check.

Address Naught
--------------

Often writing to address naught is a mistake, it indicates that some target
address is unset, especially in larger modes. `address_naught` mode detects all
writes to this address and gives a new (to VM) error in that case -
`AMX_ERR_ADDRESS_0`. Note that this isn't always an error, it is a valid
address and real data can be stored there, so if this detection is enabled the
mode must ensure that nothing important will be written there (fixes.inc does
this by defining and not using the anonymous automata). This is also the reason
why there is no global config option for this detection - it is by necessity
done on a per-mode basis, is off by default, and can only be enabled by
functions (technically by registers).

Functions
---------

There are several functions defined in `crashdetect.inc` to access plugin
information. These are all just wrappers around direct register accesses, but
provide a much nicer API.

* `bool:IsCrashDetectPresent()` - Is the CrashDetect plugin loaded?
* `SetCrashDetectLongCallTime(us_time)` - Set the long call warning threshold.
* `GetCrashDetectLongCallTime()` - Get the long call warning threshold.
* `DisableCrashDetectLongCall()` - Disable the long call warning.
* `EnableCrashDetectLongCall()` - Enable the long call warning.
* `ResetCrashDetectLongCallTime()` - Reset the long call threshold to the
   default (from `server.cfg`).
* `RestartCrashDetectLongCall()` - Restart the long call timer.
* `bool:IsCrashDetectLongCallEnabled()` - Is long function call detection
   enabled?
* `bool:HasCrashDetectLongCall()` - Does the current version of CrashDetect
   support this feature?
* `GetCrashDetectDefaultTime()` - Get the default long call time threshold.
* `DisableCrashDetectAddr0()` - Disable address naught write detection in this
   mode.
* `EnableCrashDetectAddr0()` - Enable address naught write detection in this
   mode.
* `bool:IsCrashDetectAddr0Enabled()` - Is the error currently enabled?
* `bool:HasCrashDetectAddr0()` - Does the current version of CrashDetect
   support this feature?

Registers
---------

The plugin adds two control registers accessed via `LCTRL` and `SCTRL` - `0xFF`
for general flags and `0xFE` for long call time values:

```pawn
// Get the current long call time (even when it is disabled).
#emit ZERO.pri          // Always set pri to 0 before `LCTRL` calls.
#emit LCTRL        0xFE // Will set `pri` if the plugin is loaded.
#emit STOR.pri     var  // Save the result.
```

```pawn
// Enable address naught write detection.
#emit CONST.pri    192  // 64 (address_naught control bit) | 128 (address_naught enable).
#emit SCTRL        0xFF // Set the register.
```

The flags are:

* `1` - CrashDetect is present (read only).
* `2` - long_call_time checks enabled (write ignored when `server.cfg` has
  `long_call_time 0`).
* `4` - long_call_time reset to default time (write `1` only).
* `8` - long_call_time restart check from now (write `1` only).
* `16` - Error with the CrashDetect user data.
* `32` - long_call_time control bit.
* `64` - address_naught control bit.
* `128` - address_naught detection enabled.

When read-only values are set, they are ignored. When write-only bits are
returned they are always `0`. When write-1-only bits are `0` they are ignored,
the `1` is a signal to trigger something. To set most registers the relevant
control bit must also be set. So to enable address naught detection requires
bit `6` (`64`) and bit `7` (`128`). To disable it just requires bit `6`.
Note that while many of these bits would seem to be independent you cannot
disable address naught detection, enable long call detection, and reset and
restart the timer all at once with `0x6E`; only one command at once will work.

Building from source code
-------------------------

If you want to build CrashDetect from source code, e.g. to fix a bug and
submit a pull request, simply follow the steps below. You will need a C++
compiler and CMake.

### Linux

Install gcc and g++, make and cmake. On Ubuntu you would do that like so:

```
sudo apt-get install gcc g++ make cmake
```

If you're on a 64-bit system you'll need additional packages for compiling
for 32-bit:

```
sudo apt-get install gcc-multilib g++-multilib
```

For CentOS:

```
yum install gcc gcc-c++ cmake28 make
```

Now you're ready to build CrashDetect:

```
cd crashdetect
mkdir build && cd build
cmake ../ -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF
make
```

### Windows

You'll need to install CMake and Visual Studio (Express edition will suffice).
After that, either run cmake from the command line:

```
cd crashdetect
mkdir build && cd build
path/to/cmake.exe ../ -DBUILD_TESTING=OFF
```

or do the same from cmake-gui. This will generate a Visual Studio project in
the build folder.

To build the project:

```
path/to/cmake.exe --build . --config Release
```

You can also build it from within Visual Studio: open build/crashdetect.sln
and go to menu -> Build -> Build Solution (or just press F7).

License
-------

Licensed under the 2-clause BSD license. See [LICENSE.txt](LICENSE.txt).

[github]: https://github.com/Zeex/samp-plugin-crashdetect
[version]: http://badge.fury.io/gh/Zeex%2Fsamp-plugin-crashdetect
[version_badge]: https://badge.fury.io/gh/Zeex%2Fsamp-plugin-crashdetect.svg
[build]: https://ci.appveyor.com/project/Zeex/samp-plugin-crashdetect/branch/master
[build_status]: https://ci.appveyor.com/api/projects/status/nay4h3t5cu6469ic/branch/master?svg=true
[download]: https://github.com/Zeex/samp-plugin-crashdetect/releases
[debug_info]: https://github.com/Zeex/samp-plugin-crashdetect/wiki/Compiling-scripts-with-debug-info
