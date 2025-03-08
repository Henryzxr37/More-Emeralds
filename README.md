# More-Emeralds
This is a data packet generated entirely by DeepSeek, with a very simple function of modifying the refresh probability of Emerald.
You can also modify the refresh range in the configuration file, but it is not modified by default.
In theory, it can also be used in other versions of Minecraft by modifying the "pack_fata" in pack.mcmeta

这是一个代码完全由DeepSeek生成的数据包，功能非常简单，就是修改绿宝石刷新概率。
也可以在配置文件修改刷新范围，但默认不修改。
按理来说也可在其他Minecraft版本使用，只需修改pack.mcmeta中的"pack_format"

Here is DeepSeek's original words / DeepSeek原话：

以下是为 **Minecraft 1.21.1** 编写的自定义绿宝石生成概率的数据包完整框架和代码，无需安装模组即可生效：
---


### 数据包结构

```bash

emerald-ores-datapack/
├── data/
│   └── minecraft/                       # 覆盖原版配置
│       └── worldgen/
│           ├── configured_feature/
│           │   └── ore_emerald.json    # 绿宝石矿石生成特征
│           └── placed_feature/
│               └── ore_emerald.json    # 绿宝石矿石分布规则
└── pack.mcmeta                         # 数据包元信息
```



---



### 文件内容



#### 1. `pack.mcmeta` (数据包描述)

```json

{
  "pack": {
    "pack_format": 42,    // 适用于1.21.X版本
    "description": "Increase emerald ore generation rate by 5x"
  }
}
```



---



#### 2. `data/minecraft/worldgen/configured_feature/ore_emerald.json`  

**覆盖原版绿宝石矿石特征配置**  

```json

{
  "type": "minecraft:ore",
  "config": {
    "discard_chance_on_air_exposure": 0,
    "size": 6,           // 矿脉最大生成数量（原版为3）
    "targets": [
      {
        "target": {
          "predicate_type": "minecraft:tag_match",
          "tag": "minecraft:stone_ore_replaceables"
        },
        "state": {
          "Name": "minecraft:emerald_ore"
        }
      },
      {
        "target": {
          "predicate_type": "minecraft:tag_match",
          "tag": "minecraft:deepslate_ore_replaceables"
        },
        "state": {
          "Name": "minecraft:deepslate_emerald_ore"
        }
      }
    ]
  }
}
```



---



#### 3. `data/minecraft/worldgen/placed_feature/ore_emerald.json`  

**覆盖原版绿宝石矿石分布规则**  

```json

{
  "feature": "minecraft:ore_emerald",
  "placement": [
    {
      "type": "minecraft:count",         // 每区块生成次数
      "count": 20                        // 原版为4，此处提升5倍
    },
    {
      "type": "minecraft:height_range",  // 生成高度范围
      "height": {
        "type": "minecraft:uniform",
        "min_inclusive": {
          "absolute": -16
        },
        "max_inclusive": {
          "absolute": 320
        }
      }
    },
    {
      "type": "minecraft:biome"
    }
  ]
}
```



---



### 功能说明

1. **生成频率调整**  

   - 原版每个区块生成 **4** 次绿宝石矿脉，现修改为 **20** 次（提升5倍）。

   - 矿脉最大单次生成数量（`size`）从 **3** 提升至 **6**。



2. **生成高度范围**  

   - 支持从 **Y=-16** 到 **Y=320** 生成（覆盖深层和常规矿石）。



---



### 使用方法

1. 将 `emerald-ores-datapack` 文件夹压缩为 `.zip` 格式。

2. 将压缩包放入游戏存档的 `datapacks` 文件夹（路径示例）:

   ```

   .minecraft/saves/你的存档名称/datapacks/

   ```

3. 进入游戏后输入命令 `/reload` 加载数据包。

4. 验证是否生效：在新生成的区块中挖掘测试。



---



### 自定义参数（可选）

- **调整生成频率**：修改 `placed_feature/ore_emerald.json` 中的 `count` 值。

- **限制生成高度**：修改 `height_range` 的 `min_inclusive` 和 `max_inclusive`。

- **修改矿脉大小**：调整 `configured_feature/ore_emerald.json` 中的 `size`。



---



### 注意事项

- **版本兼容性**：数据包基于 **1.21.1** 版本设计，其他版本可能需要调整 `pack_format` 和字段名称。

- **与原版特性冲突**：若其他数据包也修改了 `ore_emerald` 配置，需合并逻辑。
