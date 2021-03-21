
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

At the root of your project directory you can have a `content/` directory, this is where all the Json/Hjson data goes. Inside of `content/` you have subdirectories for the various kinds of content, these are the current common ones:

-   `content/items/` for [items](#item), like `copper` and `surge-alloy`;
-   `content/blocks/` for [blocks](#block), like turrets and floors;
-   `content/liquids/` for [liquids](#liquid), like `water` and `slag`;
-   `content/units/` for flying or ground [units](#unittype), like `eclipse` and `dagger`;

Note that each one of these subdirectories needs a specific content type. The filenames of these files is important, because the stem name of your path *(filename without the extension)* is used to reference it.

Furthermore the files within these `content/<content-type>/*` directories may be arbitrarly nested into other sub-directories of any name, to help you organize them further, for example:

-   `content/items/metals/iron.hjson`, which would respectively create an item named `iron`.

The content of these files will tend to look something like this:

    type: TypeOfThing
    name: Name Of Thing
    description: Description of thing.
    # ... more fields here ...

|field|type|notes|
|---|---|---|
|type|String|Content type of this object.|
|name|String|Displayed name of content.|
|description|String|Displayed description of content.|

Other fields included will be the fields of the type itself.

A side note, `name` and `description` are not required to be in the json structure. You can define them for any language with (Bundles)[#bundles]. However, if they are not present in either then the name will be <type>.<modname>-<stemname>.name and an empty description respectively.



## Types

Types have numerous fields, but the important one is `type`; this is a special field used by the content parser, that changes which type your object is. *A `Router` type can't be a `Turret` type*, as they're just completely different.

Types *extend* each other, so if `MissileBulletType` extends `BasicBulletType`, you'll have access to all the fields of `BasicBulletType` inside of `MissileBulletType` like `damage`, `lifetime` and `speed`. Fields are case sensitive: `hitSize =/= hitsize`.

What you can expect a field to do is up to the specific type, some types do absolutely nothing with their fields, and work mostly as a base types will extend from. One such type is `Block`.

In this unit example, the type of the unit is `flying`. The type of the `bullet` is `BulletType`, so you can use `MissileBulletType`, because `MissileBulletType` extends `BulletType`.

One could also use `mech`, `legs`, `naval` or `payload` as the unit type here.

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

As of build `125.1`, types can also be the *fully-qualified class name* of a Java class. 

For example, to specify a block as a `MendProjector`, you may write
`type: mindustry.world.blocks.defense.MendProjector` instead of `type: MendProjector`.

While not particularly useful for vanilla types, this can be used to load block types *from other Java mods* as dependencies.

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


