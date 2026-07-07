# Traffic Flow Plugin

UE version: Unreal Engine 5.8
Plugin folder: `TrafficFlow`

## Overview

Traffic Flow is a lightweight spline-based traffic system for Unreal Engine. It can generate moving road traffic along an editable spline path and is designed for city roads, background traffic, digital twin scenes, cinematic previews, driving environments, and quick road-population workflows.

## Included Assets

- `BP_TrafficFlowManager`
- `BP_TrafficVehicle_Sedan`
- `BP_TrafficVehicle_Van`
- `BP_TrafficVehicle_Truck`
- `TrafficVehicleActor` C++ base class
- Runtime and editor modules

## Installation

Copy the `TrafficFlow` plugin folder into your Unreal Engine project:

```text
YourProject/Plugins/TrafficFlow/TrafficFlow.uplugin
```

After copying:

1. Open your Unreal Engine project.
2. Go to `Edit > Plugins`.
3. Search for `Traffic Flow`.
4. Enable the plugin.
5. Restart the editor if prompted.

If the plugin content is not visible in the Content Browser:

1. Open the Content Browser settings.
2. Enable `Show Plugin Content`.
3. Browse to `All > Plugins > TrafficFlow`, or search for `BP_TrafficFlowManager`.

## Quick Start

1. Drag `BP_TrafficFlowManager` into your level.
2. Select the manager actor in the level.
3. Edit the `TrafficPath` spline points.
4. Configure vehicle count, lane count, lane spacing, speed, and spacing settings.
5. Press Play to generate traffic moving along the spline.

Enable `Loop Traffic` if you want vehicles to continue from the end of the spline back to the beginning.

## Key Settings

- `Loop Traffic`: loops vehicles back to the start after they reach the end.
- `Reverse Traffic Direction`: reverses movement direction along the spline.
- `Lane Count`: number of traffic lanes.
- `Lane Spacing`: distance between lanes.
- `Use Follow Avoidance`: slows vehicles behind traffic ahead.
- `Minimum Gap`: minimum vehicle spacing.
- `Slowdown Distance`: distance at which vehicles begin slowing behind traffic.
- `Use Random Spacing`: randomizes spawn spacing.
- `Use Random Speed`: randomizes initial speed.
- `Use Dynamic Speed Variation`: varies speed during runtime.
- `Use Terrain Detection`: projects vehicles onto terrain using downward traces.
- `Terrain Z Offset`: global height offset added after terrain detection.
- `Vehicle Ground Z Offsets`: per-vehicle-type height offsets. The array order matches `Vehicle Types`. Use negative values for vehicles floating above the terrain and positive values for vehicles sinking below the terrain.
- `Show Editor Preview`: previews vehicle distribution without entering Play mode.

## Vehicle Replacement

Use the `Vehicle Types` array on `BP_TrafficFlowManager` to add or replace vehicle classes. Each vehicle type can define a name, actor class, mesh, scale, length, desired speed, and spawn weight.

`Vehicle Class` can be a custom Actor class or a subclass of `TrafficVehicleActor`. Subclasses of `TrafficVehicleActor` support additional helper behavior such as mesh replacement, wheel rotation, vehicle length, and speed synchronization.

## Terrain Alignment

Enable `Use Terrain Detection` to place vehicles on the detected road or terrain surface. The manager traces downward from each vehicle pivot and uses the hit point as the base ground height.

If a vehicle mesh has a different imported pivot, wheel baseline, or visual bottom offset, adjust `Vehicle Ground Z Offsets` on `BP_TrafficFlowManager`. This array is matched by index to `Vehicle Types`:

```text
Vehicle Ground Z Offsets[0] -> Vehicle Types[0]
Vehicle Ground Z Offsets[1] -> Vehicle Types[1]
Vehicle Ground Z Offsets[2] -> Vehicle Types[2]
```

Typical values:

- Floating vehicle: use a negative offset, for example `-20`.
- Sinking vehicle: use a positive offset, for example `20`.
- Correctly aligned vehicle: keep `0`.

Use `Terrain Z Offset` only when all vehicles need the same global height adjustment. Use `Vehicle Ground Z Offsets` when only specific vehicle meshes need correction.

## Overtaking

Vehicles can attempt steered overtaking when traffic ahead is blocked. The system checks adjacent lane clearance, turns the vehicle toward the passing lane, changes lanes smoothly, and returns when safe. Overtake chance and retry timing can be configured on the traffic manager.

## Wheel Animation

Vehicles based on `TrafficVehicleActor` can use automatic wheel rotation. Configure `Animate Wheels`, `Auto Find Wheel Components`, `Wheel Components`, `Wheel Radius`, `Wheel Rotation Axis`, and `Invert Wheel Rotation` on the vehicle blueprint.

## Notes

- UE5.5, UE5.7, and UE5.8 packages include compiled Win64 binaries.
- UE5.6 is provided as SourceOnly and requires a complete UE5.6 C++ build environment.
- This plugin is intended for local simulation, visualization, cinematic scenes, editor-driven traffic population, and non-networked traffic scenes.
- No password or additional download software is required.
