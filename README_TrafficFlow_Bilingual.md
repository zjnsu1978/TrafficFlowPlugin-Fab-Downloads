# Traffic Flow Plugin / Traffic Flow 插件

UE version / UE 版本: Unreal Engine 5.5, 5.6, 5.7, 5.8  
Plugin folder / 插件目录: `TrafficFlow`

## Overview / 概览

Traffic Flow is a lightweight spline-based traffic system for Unreal Engine. It can generate moving road traffic along an editable spline path and is designed for city roads, background traffic, digital twin scenes, cinematic previews, driving environments, and quick road-population workflows.

Traffic Flow 是一个轻量级 Unreal Engine 样条交通系统。它可以沿可编辑样条路径生成道路车辆流，适用于城市道路、背景交通、数字孪生场景、影视预览、驾驶环境以及快速道路填充工作流。

## Included Assets / 包含资源

- `BP_TrafficFlowManager`
- `BP_TrafficVehicle_Sedan`
- `BP_TrafficVehicle_Van`
- `BP_TrafficVehicle_Truck`
- `TrafficVehicleActor` C++ base class / `TrafficVehicleActor` C++ 基类
- Runtime and editor modules / 运行时与编辑器模块

## Installation / 安装

Copy the `TrafficFlow` plugin folder into your Unreal Engine project:

将 `TrafficFlow` 插件文件夹复制到你的 Unreal Engine 项目中：

```text
YourProject/Plugins/TrafficFlow/TrafficFlow.uplugin
```

After copying:

复制后：

1. Open your Unreal Engine project. / 打开 Unreal Engine 项目。
2. Go to `Edit > Plugins`. / 进入 `Edit > Plugins`。
3. Search for `Traffic Flow`. / 搜索 `Traffic Flow`。
4. Enable the plugin. / 启用插件。
5. Restart the editor if prompted. / 如有提示，重启编辑器。

If the plugin content is not visible in the Content Browser:

如果内容浏览器中看不到插件内容：

1. Open the Content Browser settings. / 打开内容浏览器设置。
2. Enable `Show Plugin Content`. / 启用 `Show Plugin Content`。
3. After the plugin is enabled, the Content Browser should show a `TrafficFlow` plugin content folder. / 插件启用后，内容浏览器中应该会显示一个 `TrafficFlow` 插件内容文件夹。
4. Browse to `All > Plugins > TrafficFlow`, or search for `BP_TrafficFlowManager`. / 浏览到 `All > Plugins > TrafficFlow`，或搜索 `BP_TrafficFlowManager`。

## Quick Start / 快速开始

1. Drag `BP_TrafficFlowManager` into your level. / 将 `BP_TrafficFlowManager` 拖入关卡。
2. Select the manager actor in the level. / 在关卡中选中该管理器 Actor。
3. Edit the `TrafficPath` spline points. / 编辑 `TrafficPath` 样条点。
4. Configure vehicle count, lane count, lane spacing, speed, and spacing settings. / 配置车辆数量、车道数量、车道间距、速度和间距设置。
5. Press Play to generate traffic moving along the spline. / 点击 Play，生成沿样条移动的交通流。

Enable `Loop Traffic` if you want vehicles to continue from the end of the spline back to the beginning.

如果希望车辆到达样条终点后回到起点继续行驶，请启用 `Loop Traffic`。

## Key Settings / 关键设置

- `Loop Traffic`: loops vehicles back to the start after they reach the end. / 车辆到达终点后回到起点循环。
- `Reverse Traffic Direction`: reverses movement direction along the spline. / 反转沿样条的行驶方向。
- `Lane Count`: number of traffic lanes. / 车道数量。
- `Lane Spacing`: distance between lanes. / 车道间距。
- `Use Follow Avoidance`: slows vehicles behind traffic ahead. / 后车会根据前方车辆减速避让。
- `Minimum Gap`: minimum vehicle spacing. / 最小车辆间距。
- `Slowdown Distance`: distance at which vehicles begin slowing behind traffic. / 后车开始减速的距离。
- `Use Random Spacing`: randomizes spawn spacing. / 随机生成间距。
- `Use Random Speed`: randomizes initial speed. / 随机初始速度。
- `Use Dynamic Speed Variation`: varies speed during runtime. / 运行时动态变化速度。
- `Use Terrain Detection`: projects vehicles onto terrain using downward traces. / 使用向下射线将车辆投射到地形上。
- `Terrain Z Offset`: global height offset added after terrain detection. / 地形检测后添加的全局高度偏移。
- `Vehicle Ground Z Offsets`: per-vehicle-type height offsets. The array order matches `Vehicle Types`. Use negative values for vehicles floating above the terrain and positive values for vehicles sinking below the terrain. / 按车辆类型设置的高度偏移数组，顺序对应 `Vehicle Types`。车辆悬浮在地形上方时填负数，车辆陷入地面时填正数。
- `Show Editor Preview`: previews vehicle distribution without entering Play mode. / 不进入 Play 模式即可预览车辆分布。

## Vehicle Replacement / 替换车辆

Use the `Vehicle Types` array on `BP_TrafficFlowManager` to add or replace vehicle classes. Each vehicle type can define a name, actor class, mesh, scale, length, desired speed, and spawn weight.

使用 `BP_TrafficFlowManager` 上的 `Vehicle Types` 数组添加或替换车辆类型。每个车辆类型可以定义名称、Actor 类、网格、缩放、长度、期望速度和生成权重。

`Vehicle Class` can be a custom Actor class or a subclass of `TrafficVehicleActor`. Subclasses of `TrafficVehicleActor` support additional helper behavior such as mesh replacement, wheel rotation, vehicle length, and speed synchronization.

`Vehicle Class` 可以是自定义 Actor 类，也可以是 `TrafficVehicleActor` 的子类。`TrafficVehicleActor` 子类支持网格替换、车轮旋转、车辆长度和速度同步等辅助功能。

## Terrain Alignment / 地形对齐

Enable `Use Terrain Detection` to place vehicles on the detected road or terrain surface. The manager traces downward from each vehicle pivot and uses the hit point as the base ground height.

启用 `Use Terrain Detection` 后，车辆会放置到检测到的道路或地形表面。管理器会从每辆车的位置向下做射线检测，并使用命中点作为基础地面高度。

For the latest terrain alignment logic, the ground sample is taken from the vehicle center point. The final vehicle height is:

当前最新地形对齐逻辑中，地面采样取车辆中心点正下方。最终车辆高度为：

```text
Vehicle Z = Terrain Hit Z + Terrain Z Offset + Vehicle Ground Z Offsets[index]
车辆 Z = 地形命中 Z + Terrain Z Offset + Vehicle Ground Z Offsets[index]
```

When terrain detection hits a sloped surface, the vehicle rotation also aligns to the terrain normal while keeping the forward direction along the spline.

当射线命中坡面时，车辆角度也会根据地形法线对齐，同时保持车辆前进方向沿样条路径。

If a vehicle mesh has a different imported pivot, wheel baseline, or visual bottom offset, adjust `Vehicle Ground Z Offsets` on `BP_TrafficFlowManager`. This array is matched by index to `Vehicle Types`:

如果车辆模型的导入轴心、车轮基准线或视觉底部高度不同，请在 `BP_TrafficFlowManager` 上调整 `Vehicle Ground Z Offsets`。该数组按索引顺序对应 `Vehicle Types`：

```text
Vehicle Ground Z Offsets[0] -> Vehicle Types[0]
Vehicle Ground Z Offsets[1] -> Vehicle Types[1]
Vehicle Ground Z Offsets[2] -> Vehicle Types[2]
```

Typical values:

常用取值：

- Floating vehicle: use a negative offset, for example `-20`. / 车辆悬浮：使用负数，例如 `-20`。
- Sinking vehicle: use a positive offset, for example `20`. / 车辆陷入地面：使用正数，例如 `20`。
- Correctly aligned vehicle: keep `0`. / 已经对齐：保持 `0`。

Use `Terrain Z Offset` only when all vehicles need the same global height adjustment. Use `Vehicle Ground Z Offsets` when only specific vehicle meshes need correction.

当所有车辆都需要统一高度调整时，使用 `Terrain Z Offset`。当只有某些车型需要单独修正时，使用 `Vehicle Ground Z Offsets`。

If the array has no entry for a vehicle type, the plugin falls back to that vehicle type's `Ground Z Offset`.

如果数组中没有某个车辆类型的对应项，插件会回退使用该车辆类型自身的 `Ground Z Offset`。

## Overtaking / 超车

Vehicles can attempt steered overtaking when traffic ahead is blocked. The system checks adjacent lane clearance, turns the vehicle toward the passing lane, changes lanes smoothly, and returns when safe. Overtake chance and retry timing can be configured on the traffic manager.

当前方交通受阻时，车辆可以尝试带转向的超车。系统会检查相邻车道是否有足够空间，让车辆转向超车道、平滑变道，并在安全时返回原车道。超车概率和重试间隔可在交通管理器上配置。

## Wheel Animation / 车轮动画

Vehicles based on `TrafficVehicleActor` can use automatic wheel rotation. Configure `Animate Wheels`, `Auto Find Wheel Components`, `Wheel Components`, `Wheel Radius`, `Wheel Rotation Axis`, and `Invert Wheel Rotation` on the vehicle blueprint.

基于 `TrafficVehicleActor` 的车辆可以使用自动车轮旋转。在车辆蓝图中配置 `Animate Wheels`、`Auto Find Wheel Components`、`Wheel Components`、`Wheel Radius`、`Wheel Rotation Axis` 和 `Invert Wheel Rotation`。

## Notes / 注意事项

- UE5.5, UE5.6, UE5.7, and UE5.8 include precompiled Win64 editor binaries. / UE5.5、UE5.6、UE5.7、UE5.8 均包含预编译的 Win64 编辑器二进制文件。
- The UE5.6 download filename is retained for link compatibility, but the current UE5.6 package now includes precompiled binaries. / UE5.6 下载文件名为保持链接兼容而保留，但当前 UE5.6 包已经包含预编译二进制文件。
- You can install the matching single-version package under either your project `Plugins` folder or the engine `Engine/Plugins` folder. / 可以将匹配的单版本包安装到项目的 `Plugins` 目录，或引擎的 `Engine/Plugins` 目录。
- Use only the single-version package that matches your Unreal Engine version. / 只使用与你的 Unreal Engine 版本匹配的单版本包。
- The old AllVersions archive is not required. / 不再需要旧的 AllVersions 总包。
- Do not mix CN and EN packages in the same plugin folder. / 不要在同一个插件目录中混用 CN 和 EN 包。
- This plugin is intended for local simulation, visualization, cinematic scenes, editor-driven traffic population, and non-networked traffic scenes. / 本插件适用于本地模拟、可视化、影视场景、编辑器驱动的交通填充，以及非联网交通场景。
- No password or additional download software is required. / 不需要密码或额外下载软件。
