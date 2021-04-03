
# mod制作介绍  

Mindustry的mod只是像文件目录。有很多方法可以使用moddingapi，具体取决于您想要做什么，以及您愿意做多少。  

您可以重新编写现有的游戏内容，您可以使用更简单的json的api创建新的游戏内容（这是本文档的重点），您可以添加自定义声音（或重用现有声音）。它可以添加地图更改模式，并添加脚本程序特殊行为到您的模式，如自定义效果。

开始mod的制作只需要一份文件目录; mod也可以跨平台连接到支持它们的任何平台。您需要使用[GitHub]（#GitHub）（*或类似的服务*）来托管源代码。  

制作mod只需要电脑上的文本编辑器就行。


## 目录结构  

你的文件目录应该像这个：


    project
    ├── mod.hjson
    ├── content
    │   ├── items
    │   ├── blocks
    │   ├── liquids
    │   └── units
    ├── maps
    ├── bundles
    ├── sounds
    ├── schematics
    ├── scripts
    ├── sprites-override
    └── sprites

-   `mod.hjson` （必需）您的mod的数据文件，
-   `content/*` 游戏内容目录 [Content](#content),
-   `maps/` 目录中的地图
-   `bundles/` 关于 [Bundles](#bundles)的目录,
-   `sounds/` 关于 [Sound](#sound) 的目录,
-   `schematics/` 关于 [Schematic](#schematic) 的目录,
-   `scripts/` 关于 [Scripts](#scripts) 的目录,
-   `sprites-override/` [Sprites](#sprites) 覆盖游戏原版贴图中的目录,
-   `sprites/` [Sprites](#sprites) mod内容贴图目录,

每个平台都有不同的用户应用程序数据目录，这是您的mod应该放置的位置：  

-   Linux: `~/.local/share/Mindustry/mods/`
-   Steam: `steam/steamapps/common/Mindustry/mods/`
-   Windows: `%appdata%/Mindustry/mods/`
-   MacOS: `~/Library/Application Support/Mindustry/mods/`

*请注意，您的文件名应使用小写和连字符分隔：*

-   正确： `xem8k5的_方_块_.json`
-   错误： `XEM8K5的 方 块 .json`


## Hjson

Mindustry用 [Hjson](https://hjson.org/), 对于任何了解Json的人来说，它是非常流行的序列化语言 [Json](https://en.wikipedia.org/wiki/JSON). &#x2013; 这意味着任何有效的Json都可以工作，但您会得到额外的有用信息：

    # single line comment
    
    // single line comment
    
    /* multiline
    comment */
    
    key1: single line string
    
    key2:
    '''
    multiline
    string
    '''
    
    key3: [ value 1
            value 2
            value 3 ]
    
    key4: { key1: string
            key2: 0 }

如果你不知道这些语言。 &#x2013; 这种语言只是一种为程序编码信息的语言， *encode* 表示将信息从一种形式转换为另一种形式，在这种情况下，还表示将文本转换为Java数据结构。



## `mod.hjson`

在项目目录的根目录下，必须有 `mod.json` 它定义了项目的基本数据。 此文件也可以（可选）命名为 `mod.hjson` 以帮助您的文本编辑器选择更好的语法高亮显示。

    name: "模组内置名字"
    displayName: "模组外置名字"
    author: 你自己
    description: "描述"
    version: "1.0"
    minGameVersion: "最低兼容版本"
    dependencies: [ ]
    hidden: false

-   `name` 会被用来引用你的mod，所以要小心命名；
-   `displayName` 这将被用作UI的显示名，您可以使用它来为所述名称添加格式；
-   `description` 在游戏中mod管理器中会呈现mod的一部分，所以要简洁；
-   `dependencies` 是可选的，如果您想了解更多信息，请转到 [dependencies](#dependencies) 栏目;
-   `minGameVersion` 是游戏的最低支持版本。**要求** 比105大（105以下是老版本，很可能出现bug）.
-   `hidden` 是这个mod对多人游戏是否重要，默认为false。材质包、JS插件等应该启用它，以免分别与服务器和客户机发生冲突。作为一个经验法则，如果你的mod加了实质性内容，它不应该被隐藏。



## Content

在项目目录下可以有一个 `content/` 的目录, 这是所有Json/Hjson的储存位置。 在 `content/` 里面， 你可以有各种内容的子目录， 这些是比较常见的：  

-   `content/items/` 是负责 [物品](#item), 像 `铜copper` 和 `巨浪合金surge-alloy`（原版物品必须要英文）;
-   `content/blocks/` 是负责 [方块](#block), 像加工厂、或者地板;
-   `content/liquids/` 是负责 [液体](#liquid), 像 `水water` 和 `矿渣slag`;
-   `content/units/` 是负责天上飞的或者地上跑的 [单位](#unittype), 像 `日蚀eclipse` 和 `尖刀dagger`;

请注意，这些子目录中的每一个子目录都需要特定的内容类型，因为路径的根名称 *(不带拓展名的文件名)* 来引用它。

此外，这些的 `content/<content-type>/*` 的目录都可以套在任何其它的子目录, 来让你更方便查看, 比如这样:

-   `content/items/metals/iron.hjson`, 创建一个名字为 `iron` 的文件。

content中的文件形式大多像这个：  

    type: 东西的类型
    name: 东西的名字
    description: 东西的描述
    # ... 更多标签 ...

|标签|类型|说明|
|---|---|---|
|类型|字符串|此对象的内容类型。|
|名字|字符串|显示桌面的物品名字。|
|描述|字符串|显示桌面的物品描述。|

包含的其他字符将是类型本身的字符。

注： `name` 和 `description` 都没必要放在json结构。 你可以使用 (Bundles)[#bundles]来定义。 但是, 如果它们都不在其中，则名称将分别为 <type>.<modname>-<stemname>.name 和一个空描述



## Types

Types有许多字段类型, 但是最重要的那个就是 `type`; 这是content里的一个特殊字段， 来更改项目的类型。 *一个 `Router` 的类型不能改成 `Turret` 类型*, 因为他们八竿子都打不到一块，完全不同。
Types彼此 *延展* , 所以如果 `MissileBulletType` 延展了 `BasicBulletType`, 你可以调用 `BasicBulletType` 中的 `MissileBulletType` 像 `damage`, `lifetime` 和 `speed`. 要区分大小写： `hitSize =/= hitsize`.

你可以延展一个你想要的类型, 有些字段什么都不会做出任何操作，主要就是负责基本拓展，就像`Block`.  

在单位的例子中 单位的类型是 `flying`. `bullet` 的类型是 `BulletType`, 所以你可以使用 `MissileBulletType`, 因为 `MissileBulletType` 拓展了 `BulletType`.

还可以使用 `mech`, `legs`, `naval` 或者 `payload` 来作为单位类型

```hjson
type: flying
weapons: [
  {
    bullet: {
      type: MissileBulletType
      damage: 9000
    }
  }
]
```

在build `125.1`时, types也可以是Java类别的 *完全限定名* 

例如，要将块指定为 `MendProjector`, 可以写
`type: mindustry.world.blocks.defense.MendProjector` 而不是 `type: MendProjector`.

虽然对于普通类型不是特别有用，也可以加载其它方块 *来自别的 Java mods* 作为依赖项

## Tech Tree

Much like `type` there exist another magical field known as `research` which can go at the root of any block object to put it in the techtree.

    research: duo

This would put your block after `duo` in the techtree, and to put it after your own mods block you would write your `<block-name>`, a mod name prefix is only required if you're using the content from another mod.

Research costs:

|type|cost|notes|
|---|---|---|
|blocks|`requirements ^ 1.1 * 20 * researchCostMultiplier`|`researchCostMultiplier` is a stat that can be set on blocks|
|units|`requirements ^ 1.1 * 50`|---|

The cost is then rounded down to the nearest 10, 100, 1k, 10k, or 100k depending on how expensive the cost is.

`requirements` is the cost of the block or unit. Units use their build cost/upgrade cost for the calculations.

If you want to set custom research requirements use this object in place of just a name:

    research: {
      parent: duo
      requirements: [
        copper/100
      ]
    }

This can be used to override block or unit costs, or make resources need to be researched instead of just having to produce it.

## Sprites

All you need to make sprites, is an image editor that supports transparency *(aka: not paint).* Block sprites should be `32 * size`, so a `2x2` block would require a `64x64` image. Images must be PNG files with a 32-bit RGBA pixel format.
**Any other pixel format, such as 16-bit RGBA, may cause Mindustry to crash with a "Pixmap decode error".** You can use the command-line tool `file` to print information about your sprites:

    file sprites/**.png

If any of them are not 32-bit RGBA formatted, fix them.

Sprites can simply be dropped in the `sprites/` subdirectory. The content parser will look through it recursively.
Images are packed into an "atlas" for efficient for rendering. The first directory in `sprites/`, e.g. `sprites/blocks`, determines the page in this atlas that sprites are put in. Putting a block's sprite in the `units` page is likely to cause lots of lag; thus, you should try to organize things similarly to how the [vanilla game does](https://github.com/Anuken/Mindustry/tree/master/core/assets-raw/sprites).

Content is going to look for sprites relative to it's own name. `content/blocks/my-hail.json` has the name `my-hail` and similarly `sprites/my-hail.png` has the name `my-hail`, so it'll be used by this content.

Content may look for multiple sprites. `my-hail` could be a turret, and it could look for the suffix `<name>-heat` and what this means is it'll look for `my-hail-heat`.

You can find all the vanilla sprites here:

-   <https://github.com/Anuken/Mindustry/tree/master/core/assets-raw/sprites>

Another thing to know about sprites is that some of them are modified by the game. Turrets specifically have a black border added to them, so you must account for that while making your sprites, leaving transparent space around turrets for example: [Ripple](https://raw.githubusercontent.com/Anuken/Mindustry/master/core/assets-raw/sprites/blocks/turrets/ripple.png)

To override ingame content sprites, you can simply put them in `sprites-override/`.
This removes the `<modname>-` prefix to their id, which allows them to override sprites from vanilla and even other mods.
You can also use this to create sprites with nice short names like `cat` for easy use with scripts, just beware of name collisions with other mods.



## Sound

Custom sounds can be added through the modding system by dropping them in the `sounds/` subdirectory. It doesn't matter where you put them after that. Two formats are supported: `ogg` and `mp3`. Note that `mp3` files cannot loop seamlessly, so try to use `ogg` whenever possible.

Just like any other assets, you reference them by the stem of your filenames, so `pewpew.ogg` and `pewpew.mp3` can be referenced with `pewpew` from a field of type `Sound`.

Here's a list of built-in sounds:

$sounds

## Dependencies

You can add dependencies to your mod by simple adding other mods name in your `mod.json`:

    dependencies: [
      other-mod-name
      not-a-mod
    ]

The name of dependencies are lower-cased and spaces are replaced with `-` hyphens, for example `Other MOD NamE` becomes `other-mod-name`.

To reference the other mods assets, you must prefix the asset with the other mods name:

-   `other-mod-name-not-copper` would reference `not-copper` in `other-mod-name`
-   `other-mod-name-angry-dagger` would reference `angry-dagger` in `other-mod-name`
-   `not-a-mod-angry-dagger` would reference `angry-dagger` in `not-a-mod`



## Bundles

An optional addition to your mod is called bundles. The main use of bundles are give translations of your content, but there's no reason you couldn't use them in English. These are plaintext files which go in the `bundles/` subdirectory, and they should be named something like `bundle_ru.properties` (for Russian).

The contents of this file is very simple:

    block.example-mod-silver-wall.name = Серебряная Стена
    block.example-mod-silver-wall.description = Стена из серебра.

If you've read the first few sections of this guide, you'll spot it right away:

-   `<content type>.<mod name>-<content name>.name`
-   `<content type>.<mod name>-<content name>.description`

With your own custom bundle lines for use in scripts you can use whatever key you like:

-    `message.egg = Eat your eggs`
-    `randomline = Random Line`

Notes:

-   mod/content names are lowercased and hyphen separated.

List of content types:

$contentTypes

List of bundle suffixes relative to languages:

$bundles


## GitHub

Once you have a mod of some kind, you'll want to actually share it, and you may even want to work with other people on it, and to do that you can use [GitHub](https://github.com/). If you don't know what Git (or GitHub) is at all, then you should look into [GitHub Desktop](https://desktop.github.com/), otherwise simply use your favorite command line tool or text editor plugin. 

All you need understand is how to open repositories on GitHub, stage and commit changes in your local repository, and push changes to the GitHub repository. Once your project is on GitHub, there are three ways to share it:

-   with the endpoint, for example `Anuken/ExampleMod`, which could then be typed in the ingame GitHub interface, and that would download it;
-   with the zip file, for example `https://github.com/Anuken/ExampleMod/archive/master.zip`, which would download the repository as a zip file, and put in mod directory (unzipping is not required);
-   add the topic/tag `mindustry-mod` on your repository, which should add it to the topic search and the [Mod scraper](https://github.com/Anuken/MindustryMods).



## FAQ

-   `time` in game is calculated through `ticks`;
-   `ticks` *sometimes called `frames`,* are assumed to be 1/60th of a second;
-   `tilesize` is 8 units internally;
-   to calculate range out of `lifetime` and `speed` you can do `lifetime * speed = range`;
-   *Abstract* what is `abstract`? All you need to know about abstract types is that they cannot be instantiated/initialized by themselves. If you do so, you'll  get an *"initialization exception"* of some kind;
-   what is a `NullPointerException`? This is an error message that indicates a field is null and shouldn't be null, meaning one of the required fields may be missing;
-   *bleeding-edge* what is `bleeding-edge`? This is the developer version of Mindustry, specifically it's refering to the Github master branch. Changes on bleeding-edge usually make it into Mindustry in the next release.


