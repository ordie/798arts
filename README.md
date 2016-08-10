# 798arts
798景区导览APP

# 简介
之前做的一个景区导览APP，核心功能是地图展示、路径搜索和导航。由于有些特殊需求使得不能用第三方地图SDK，所以自己基于苹果的Large Image Downsizing Demo写了一个简易的地图引擎，实现了这几个核心功能。

# 功能
1 地图分级缩放，共分3级，支持多级。
2 在地图上标识当前位置
3 地标点分层显示
4 道路支持后台编辑（添加删除道路）
5 中英文地图即使切换（地图和地标点同时切换）

# 实现原理
1 地图分级缩放功能基于苹果提供的例子程序
 （参见：https://developer.apple.com/library/ios/samplecode/LargeImageDownsizing/Introduction/Intro.html）
  基本思想是将无法一次性加载到内存的大图分块处理，再加入块的复用机制（类似UITableView），减小内存占用。

2 地图文件分成小块以文件存储，文件名类似这样：map_grid_cn_1_00_00，map_grid是前缀，cn代表中文版地图，1代表分层级别，后面的两个00代表行和列的序号。每层对于上层的缩放比例是2倍关系。

3 当前位置定位
  取地图上的3个特定地标点（需要实地测量经纬度），计算出当前位置经纬度与这3点的距离，根据3点定位法，即可算出当前经纬度在地图上的位置。具体计算细节见LocationCalculator类。

4 道路后台编辑
  道路信息存储在798street.json文件里。其中，points数组存放所有的地标点信息（id和x、y坐标，坐标是像素坐标），paths数组存储道路信息（id和起点终点）。服务端在Web页面上可以编辑对道路进行增删改操作，然后用json同步到客户端。客户端根据paths数组points数组建立道路节点路径。这部分代码见StreetInfo文件。

5 路径搜索
  使用Dijkstra算法寻找最短路径，这部分代码也可见StreetInfo文件。
