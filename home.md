![head](https://bluecolored.de/paste/BluemapBanner.png)
### 欢迎来到BlueMap中文Wiki  
BlueMap是一个可以将您的服务器地图以一个3D的样式展现在您的Web浏览器中，它现在支持几乎所有的Java-Edition我的世界服务端！  
在这个Wiki里面，我将试图为您解释这个插件您所需要的一切东西，如果您需要更多的帮助，可以随时加入discord或者reddit，然后在里面提问！
此wiki的原文地址在[GitHub](https://github.com/BlueMap-Minecraft/BlueMap/wiki)，译者为上杉夏相
### 常见问题  
1.  我可以在哪里查看我的地图？
    默认情况下，它是你的服务器的公网IP地址（如123.45.67.8）加上端口（8100），就像这样：`http://123.45.67.8:8100/`，不过您也可以更改端口选项
2. 我无法访问我的地图！
    下面是一个原因清单，请仔细排查：
> 1. 端口（在默认情况下是8100）是否正确的打开并且转发？您的防火墙是否组织了外部连接的进入？如果您不确定此选项，可以尝试在配置文件中更换端口！
> 2. 您是否在配置文件中设置了正确的服务器IP？尝试更换为0.0.0.0（注释掉默认）以允许所有的连接都允许访问！
> 3. 您的服务器控制台，log文件中是否含有`Webserver started...`的消息吗？ 如果没有，请仔细检查是否含有错误或者警告！
3. 我的浏览器显示404 - Not Found！
    检查您的`core.conf`配置文件，您是否为`accept-download`选项设置为`true`？如果是，请检查您的`render.conf`将`webserver.conf`设置为相同的/正确的文件夹！  
4. 我的地图没有更新！
    BlueMap需要等待服务器将世界数据保存到硬盘上，因此地图的更改更改可能需要一些时间延迟才能显示在网页上。另外，也可以单击左侧菜单`clear tile cache`选项，以使浏览器获取新副本。
5. 我的地图上有黑色/粉红色格子的方块！
    阅读此内容：https : //github.com/BlueMap-Minecraft/BlueMap/wiki/Configuring-mods
6. 当我放大地图时，我的方块纹理被弄乱了！
    首先，在浏览器菜单中使用`clear tile cache`选项刷新本地缓存资源确保浏览器没有缓存什么奇怪的内容，当然你也可以完全清除浏览器缓存（Ctrl+F5可能并不是很起作用）；如果这样还是不能解决问题，则您可能更改了一些 完全需要重新渲染地图的BlueMap设置：请删除包含渲染地图数据的整个文件夹，默认为`/bluemap/wen/data`，并且使用`/bluemap render <world>`重新渲染整个地图，然后在此清除浏览器缓存，应该可以修复。
7. 我只有全黑的界面/很多地图都不见了？
    - 是否使用了`bluemap render <world>`，如果没有，请立刻使用它！
    - 在浏览器菜单中使用`clear tile cache`选项刷新本地缓存资源确保浏览器没有缓存什么奇怪的内容，当然你也可以完全清除浏览器缓存（Ctrl+F5可能并不是很起作用）
    - 你是否创造过这个世界？玩家尚未访问过的世界不会被渲染！因此，他将今渲染玩家至少加载一次的区块。
    - 你在使用一些模组吗？一些模组可能会阻止Minecraft生存和存储光照数据。BlueMap需要光照的数据并且忽略没有此数据的区块；您可以使用在`render.conf`中更改`ignoreMissingLightData: true`修复此问题，但是他将始终启用洞穴渲染且地图上的所有内容都会被照亮，展现类似于夜视的效果。
8. 网页真的加载的很缓慢！
    请确保本地浏览器启用了硬件加速选项，使用您最喜欢的的搜索引擎来学习如何启用:）
9. BlueMap中找不到我的地图！
    您必须将您需要展示的世界添加到`render.conf`中！默认只加载world的三个维度。
10. 如何使用SSL(HTTPS)访问网页？
    BlueMap集成的Web服务器无法应用此选项，请使用NGINX或Apache等进行反代理添加SSL！
### 已知的不兼容模组
- JustEnoughIDs (jeid)
- NotEnoughIDs
- OpenCubicChunks
- SlimeWorldManager
