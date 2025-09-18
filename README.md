# Artemis Plugins

[![Zig Version](https://img.shields.io/badge/zig-0.15.1-orange)](https://ziglang.org/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)]()

**Plugin system and community registry for Artemis Engine with hot-reloading support.**

> **🔌 Extensible**: Design your game engine exactly how you want it with modular plugins.

## ✨ What is Artemis Plugins?

The Artemis Plugin system allows you to extend the engine with custom functionality while maintaining modularity and performance. It provides:

### Key Features

- **🔄 Hot Reloading** - Modify plugins without restarting your game
- **📦 Plugin Registry** - Community-driven plugin marketplace
- **🏗️ Modular Design** - Use only what you need
- **🔒 Type Safety** - Compile-time plugin validation
- **⚡ High Performance** - Minimal overhead plugin architecture
- **👥 Community Driven** - Share and discover plugins

## 🚀 Quick Start

### Prerequisites
- **Zig 0.15.1+**
- **Artemis Engine** (automatically handled as dependency)

### Installation

Add to your `build.zig.zon`:

```zig
.{
    .name = "my-game",
    .version = "0.1.0",
    .dependencies = .{
        .artemis_plugins = .{
            .url = "https://github.com/terminatable/artemis-plugins/archive/main.tar.gz",
            .hash = "...", // zig will provide this
        },
    },
}
```

### Creating Your First Plugin

```zig
const std = @import("std");
const artemis = @import("artemis-engine");
const plugins = @import("artemis-plugins");

const MyPlugin = struct {
    pub fn build(plugin: *plugins.PluginBuilder) !void {
        plugin.setName("My Awesome Plugin");
        plugin.setVersion("1.0.0");
        plugin.addSystem(MySystem);
    }
};

const MySystem = struct {
    pub fn update(world: *artemis.World, dt: f32) !void {
        _ = world;
        _ = dt;
        std.debug.print("My plugin is running!\n", .{});
    }
};

pub fn main() !void {
    var gpa = std.heap.GeneralPurposeAllocator(.{}){};
    defer _ = gpa.deinit();

    var world = artemis.World.init(gpa.allocator());
    defer world.deinit();

    var plugin_manager = plugins.PluginManager.init(gpa.allocator());
    defer plugin_manager.deinit();

    try plugin_manager.addPlugin(MyPlugin);
    try plugin_manager.initializePlugins(&world);

    // Game loop
    for (0..3) |_| {
        try plugin_manager.updatePlugins(&world, 1.0/60.0);
    }
}
```

## 🗺️ Plugin Types

- **System Plugins** - Add new ECS systems
- **Component Plugins** - Define new component types  
- **Asset Plugins** - Custom asset loaders
- **Renderer Plugins** - Rendering extensions
- **Input Plugins** - Custom input handlers
- **Network Plugins** - Multiplayer functionality

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

**Artemis Plugins** - Extensible Plugin System for Zig  
*Part of the [Terminatable](https://github.com/terminatable) ecosystem*