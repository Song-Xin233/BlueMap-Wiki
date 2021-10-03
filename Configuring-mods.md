## 配置模组

如果您正在渲染一个存在有模组自定义方块的世界，BlueMap 将不知道如何处理渲染这些方块，因此它们将看起来像这样：

如果您不希望出现这种情况，您可以尝试自行添加和配置这些模组的模型资源，告诉 BlueMap 如何渲染这些东西

**不幸的是，对于 1.12.x 来说，这并不容易，甚至非常枯燥**

以下是具体的步骤

## First Step - 常规

    Tips:
    在阅读本章之前，请务必阅读“安装资源包”章节

首先你要做的也是最重要的一件事，就是把符合版本的**客户端模组**上传到相关文件夹

比如：如果您想添加对丰富的生物群落“biomes-o-plenty”模组的支持，您需要从客户端获得相应版本的模组文件并将其上载到服务器上的 BlueMaps resourcepack 文件夹（位于 BlueMaps 配置文件旁边）！

然后，BlueMap 将会像加载资源包一样加载该模组：尝试解析方块的模型并加载它的纹理。

    Tips：
    某些模组可能会使用不支持的资源包/纹理格式（Forge's blockstate.json的支持是非常有限），所以您必须使用正确的格式来替换覆盖纹理资源包文件

如果一切正常，它就已经正常的显示了！但是，（尤其是在 1.12.x 上）您可能需要做一些更多的配置文件来更好的适配，浏览下面的章节！

## Second Step - 配置

### 1. 方块 ID 配置（仅限 1.12）

**文件（示例：blockIds.json）**

```json
{
  "extragrassmod:grass:0": "extragrassmod:grass[variant=mossy,snowy=false]",
  "extragrassmod:grass:1": "extragrassmod:grass[variant=dry,snowy=false]",
  "extragrassmod:grass:2": "extragrassmod:grass[variant=sandy,snowy=false]",
  "extragrassmod:grass:3": "extragrassmod:grass[variant=flowered,snowy=true]"
}
```

这将是最烦人的部分！但它只是将方块存储为 1.12.x 的格式！  
所以，如果您有一个只存在于 1.13 及以上的服务器，您并不需要这样做

在 1.13 版本之前，方块是通过其数字 ID 和元值存储。因此，某些方块的存储类型类似于这样`1:5`的键值对形式。

如果模组想要添加新的方块，这个方块就会被赋予新的物品 ID。然后，Forge 将该方块的数字 ID 和值 ID 存储在世界文件的相应表中。现在，BlueMap 需要知道每个 ID:值组合呈现的是什么类型的方块

因此，我们有三种 ID 类型：

- 一些特定的整数代表的方块，但是对于不同世界来说可能会有所不同：`numeric-id 169`
- 一些特定的名称（唯一）代表的方块：`literal-id minecraft:sea_lantern`
- 在资源或资源包中声明的方块（通常含有状态位置和名称），因此，BlueMap 将会在加载的资源文件中收缩，这通常是相同的，但也有一些罕见的情况下，他们是不同的`resource-id minecraft:sea_lantern assets/<namespace>/blockstates/<id>.json assets/minecraft/blockstates/sea_lantern.json literal-id`

所以要配置一个方块，您需要找到所有可能的键值对及方块名称，然后使用键值对的形式格式化它们，并像开头示例一样添加到配置文件中，您也可以使用数字 ID 而不是字母 ID，但是由于数字 ID 可能随着世界的改变而变化，因此不建议这样做。`"namespace:literal-id:meta": "namespace:resource-id[property1=value1,property2=value2]"`

如果您不能通过常规的方式寻找到某些模组方块的键值对或方块名称，可以查看下面这些资料获取帮助

- 模组的 Wiki 也许会有
- 按住 F3 查看相应方块会显示 ID 和值
- BlueMap 的`/BlueMap debug`指令会向您显示关于您脚下方块的所有信息
- WorldEdit 可以根据键值对创建方块

### 2.方块属性配置

    Tips：
    如果没有明确定义方块的属性，BlueMap将会查看方块模型并试图猜测其属性，当然这通常来说都是准确的，您可以忽略此配置。

文件（示例：`blockProperties.json`）：

```json
{
  "morenaturemod:special_log": {
    "culling": true,
    "occluding": true,
    "flammable": true
  },
  "morenaturemod:special_leaves": {
    "culling": false,
    "occluding": true,
    "flammable": true
  },
  "morenaturemod:cool_flower": {
    "culling": false,
    "occluding": false,
    "flammable": true
  }
}
```

要正确渲染方块，BlueMap 需要知道方块的以下属性：

- `culling`它的相邻方块，如果为是则这个块含有相邻方块，它自身的某些面是隐藏不见的
- `occluding`方块距离，这一项用于计算相邻方块遮挡时，该方块是否有“遮挡环境光”（译者注：计算是否为透明方块？）
- `flammable`这目前仅在 1.12.x 中用于计算火的外观。

### 3. 方块颜色配置

文件（示例：`blockColors.json`）

```json
{
  "minecraft:water": "@water",
  "minecraft:grass": "@grass",
  "minecraft:redstone_wire": "#ff0000",
  "minecraft:birch_leaves": "#86a863"
}
```

某些方块（如草、叶子、水或者红石）是动态的颜色变化，这些颜色可能会因为生物群系、相关特性处于静态或者变化的。

您可以使用生物群系的树叶（@foliage）、草（@grass）或者水（@water）的颜色给方块着色，或者您也可以使用 CSS 格式的十六位颜色代码给方块设置静态颜色（如#86a863）

### 4. 生物群系配置

现在仍然处于 TODO 状态，目前未知的生物群落被视为“海洋”生物群落。

## Third Step - 安装

好了！经过了上面的配置您获得了一些配置文件，但是它们应该怎么使用？

有两个地方可以激活它们

- 在 core.conf 和 render.conf 等配置文件旁边的 config 文件夹中
- 在资源包中的此文件夹中：assets/<mod-id>/BlueMap

如果您想要创建易于与他人共享的模组配置文件，资源包法也许非常有用。:)

## Fourth Step - 开发者

如果你想让你的模组与 BlueMap 兼容，你可以简单地将所有需要的资源和配置文件添加到你的 jar 文件中。

所有的配置都可以放在`assets/<namespace>/BlueMap`  
如果您想使用 BlueMap 的资源文件覆盖您自己的资源文件，您可以通过在文件夹中添加它们以实现（比如 `assets/yourmod/BlueMap/blockstates/someblock.json` 将会覆盖掉 `assets/yourmod/blockstates/someblock.json`）
