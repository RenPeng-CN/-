### HQ Chart 股票行情表
### 1. [K线界面操作 k线的各种参数解释](https://blog.csdn.net/jones2000/article/details/90272733)
1. Type： 图形类型， 这里填历史K线图代表创建一个K线图
2. Symbol： 显示的股票代码 股票代码需要带后缀. 上海.sh 深证.sz
3. IsAutoUpdate: 是否自动更新最新行情，如果是true, 会定时(30s/次)获取行情数据,更新K线图及指标
4. IsShowRightMenu: 在K线图上，是否显示右键菜单， 如果是手机页面可以设置成false
5. IsShowCorssCursorInfo: 在K线图上 鼠标移动或手势的时候是否显示十字光标
6. KLine.DragMode: 拖拽模式 0=禁止拖拽(禁止鼠标或手势左右拖动数据) 1=数据拖拽(可以用鼠标或手势左右拖动数据) 2=区间选择(拖动可以选择一段时间数据做区间统计，和形态匹配
7. KLine.Right: 复权 0=不复权 1=前复权 2=后复权
8. KLine.Period: 周期 0=日线  1=周线  2=月线  3=年线  4=1分钟  5=5分钟  6=15分钟  7=30分钟  8=60分钟  9=季线  10=分笔线 （小程序不支持) 20001-29999 自定义分钟  30001-32000 自定义秒  40001-49999 自定义天数
9. IsApiPeriod 周期和复权数据全部使用api获取， 默认周期和复权是本地计算的
10. KLine.MaxReqeustDataCount: 请求K线数据的最大个数
11. KLine.PageSize: 初始一屏显示几根K线，通过鼠标滚轴上下，键盘上下，手势两个手指上下可以调整一屏显示K线的个数
12. KLine.Info: 信息地雷 目前支持“互动易",“大宗交易”,‘龙虎榜’,“调研”,“业绩预告”,“公告”， 可以选择其中的几个填上
13. KLine.IsShowTooltip: 是否显示K线的tooltip信息, 鼠标移动到K线上 或 键盘左右移动十字光标都会出现tooltip.
14. KLine.DrawType K线类型 0=实心K线柱子 1=收盘价线 2=美国线 3=空心K线柱子 4= 收盘价面积图
15. KLine.IsShowMaxMinPrice K线上是否显示最大最小值 （版本>=7717)
16. KLineTitle.IsShowName: K线标题是否显示股票名称
17. KLineTitle.IsShowSettingInfo: K线标题是否周期/复权
18. KLine.RightSpaceCount 最后数据和右边框空白间距,空白的宽度=RightSpaceCount*k线宽度
19. Border.Left, Border.Right,Border.Bottom,Border.Top 坐标边框到画布边框的间距
20. Windows 指标窗口，数组可以配置多个指标窗口， 每个窗口单独设置
1. Index 系统指标名字  2. Modify 是否显示修改指标参数按钮  3. Change 是否可以切换股票  4. Close 是否显示关闭指标窗口按钮

21. Frame 指标窗口坐标设置
1. SplitCount y轴刻度个数 
2. IsShowLeftText 是否显示左侧Y轴刻度 
3. 左侧刻度如果间距不够会显示在框架内部， 必须设置这个值才能去掉左侧Y轴刻度显示
4. IsShowRightText 是否显示右侧Y轴刻度
5. Height 窗口高度比值
6. Custom自定义刻度 详见HQChart使用教程50-Y轴自定义刻度设置说明

22. StepPixel 移动一个K线需要的手势移动的像素(默认4)
23. ZoomStepPixel 缩放一次,2个手指需要移动的间距像素(默认5)


### 2.[HQChart使用教程2- 如何把自定义指标显示在K线图页面](https://blog.csdn.net/jones2000/article/details/90273684)
### 3. [HQChart使用教程3- 如何把指标上锁显示在K线图页面](https://blog.csdn.net/jones2000/article/details/90285723)
### 4. [HQChart使用教程4- 如何自定义K线图颜色风格](https://blog.csdn.net/jones2000/article/details/90286933)
### 5. [HQChart使用教程5- K线图控件操作函数说明](https://blog.csdn.net/jones2000/article/details/90301000)
注：所有这些操作(除了全局函数)必须在图形创建完成并且调用SetOption()以后.
周期切换 Chart.ChangePeriod(周期值整型)  //周期值 0=日线 1=周线 2=月线 3=年线 4=1分钟 5=5分钟 6=15分钟 7=30分钟 8=60分钟
切换股票 Chart.ChangeSymbol(symbol) // symbol 字符串股票代码 ， 代码.市场后缀 上海=sh 深证=sz
切换指标 Chart.ChangeIndex(windowIndex,indexName,option) 
  1. windowIndex 窗口索引 从0开始
  2. indexName 指标唯一的ID
  3. option 可选设置 {StringFormat: 标题数据格式, FloatPrecision: 小数位数}
  
切换自定义指标 Chart.ChangeScriptIndex=(windowIndex,indexData) 
   1. windowIndex 窗口索引 从0开始
   2. indexData 自定义指标 {Name：指标名字, Script：指标脚本, Args：指标参数（数组) }

切换复权 Chart.ChangeRight(right)  right 复权 0=不复权 1=前复权 2=后复权
叠加股票/取消叠加股票  Chart.OverlaySymbol(symbol)  // symbol 需要叠加的股票代码字符串 只支持叠加一个股票
K线切换类型 Chart.ChangeKLineDrawType(drawType)  // drawType K线切换类型 0=实心K线 1=收盘价线 2=美国线 3=空心K线
设置指标窗口个数  Chart.ChangeIndexWindowCount (count) // count 窗口个数， 最多8个
五彩K线|交易指示 指标  Chart.ChangeInstructionIndex(indexName) indexName 五彩K线或交易指示 ID
取消五彩K线|交易指示 指标 Chart.CancelInstructionIndex() 
自定义五彩K线|交易指示 指标  Chart.ChangeInstructionScriptIndex=function(indexData)
   1. indexData 自定义指标 {Name：指标名字, Script：指标脚本, Args：指标参数（数组)， InstructionType： 指标类型 2=五彩K线 1=交易指示 }

信息地雷 目前支持 “互动易”,“大宗交易”,‘龙虎榜’,“调研”,“业绩预告”,“公告”
   1. 增加一个信息地雷 Chart.AddKLineInfo(infoName, bUpdate) // infoName 信息地雷名字 ( “互动易”,“大宗交易”,‘龙虎榜’,“调研”,“业绩预告”,“公告”) bUpdate 是否立即更新
   2. 删除一个信息地雷 Chart.DeleteKLineInfo(infoName) infoName 删除信息地雷的名字
   3. 删除所有信息地雷 Chart.ClearKLineInfo()

移动当前屏K线数据到具体的某一天 Chart.SetFirstShowDate(obj) // obj { Date:起始日期(必填), PageSize:一屏显示的数据个数(可选) }
注册K线图事件监听事件  Chart.AddEventCallback(obj)  // obj { event:事件ID, callback:回调函数 } 
设置颜色风格（全局） JSChart.SetStyle=function(option)  // 具体设置可以看 HQChart使用教程4- 如何自定义K线图颜色风格
K线缩放比例配置  JSChart.GetKLineZoom=function() //K线缩放配置  返回柱子宽度和间距的值 
数据API域名修改（全局）JSChart.SetDomain(domain,cacheDomain) //  全局设置 成员静态函数  数据我们分API数据 和 json缓存文件数据（不通过api获取， 我们直接把数据切片以Json文件格式分发到CDN或OSS） 
   1. domain api数据域名
   2. cacheDomain 缓存文件下载域名

### 6. [HQChart使用教程6- 如何获取K线图上的指标数据进行回测](https://blog.csdn.net/jones2000/article/details/90314625)
 对指标的数据进行统计或回测是验证指标是否有效的一个重要功能，一般这些功能都会由服务器进行计算，返回给前端。
 对于一些简单指标统计计算是可以由客户端计算的，毕竟现在的手机性能已经和强大了。这章我们介绍如果从K线图中获取指标数据并进行统计
 注册指标计算完成监听事件 Chart.AddEventCallback(obj)  // obj { event:事件ID, callback:回调函数 } 目前支持一下事件监听
 指标结果数据结构说明 //指标注册回调函数function(e, data, obj) 一共3个参数， 最主要的就是第2个 data它是存储的是指标结果数据, 结果是基于指标的脚本返回的数据。
 简单的前端回测计算类(RegressionTest) 我们这里提供一个简单的单股票单指标的BS点回测类(RegressionTest)， 文件在（jscommon\umychart.regressiontest.js)
     /*
         指标回测
         计算: Trade: {Count 交易次数  Days:交易天数 Success:成功交易次数 Fail:失败交易次数}
               Day: {Count:总运行  Max:最长运行 Min:最短运行 Average:平均运行}
               Profit: 总收益 StockProfit:个股收益  Excess:超额收益 MaxDropdown:最大回撤 Beta:β(Beta)系数(指标里面需要又大盘数据)
               NetValue: [ {Date:日期, Net:净值, Close:股票收盘价, IndexClose:大盘的收盘价}, ]
     */
     function RegressionTest()
     {
     	//只读数据不能修改
         this.HistoryData;   //K线数据
         this.BuyData;       //策略买数据
         this.SellData;      //策略卖数据
         this.IndexClose;    //大盘收盘价
         this.NetCalculateModel=0;   //净值及收益计算模型 0=使用B点开盘价计算 1=使用B点下一天的开盘价计算
     
         this.InitialCapital=10000;  //初始资金1W
     .....
     }
     
### 7. [HQChart使用教程7- 如何快速创建一个分时图页面](https://blog.csdn.net/jones2000/article/details/90319619)
MinuteLine.IsShowAveragePrice 是否显示均线 
MinuteTitle.IsShowName 是否显示品种名称 默认是显示
Windows 指标窗口，数组可以配置多个指标窗口， 每个窗口单独设置， 指标窗口是从第3个窗口开始。 前面2个窗口是固定的一个是第1个价格图，第2个成交量图 1. Index 系统指标名字

### 8.[HQChart使用教程8- 如何快速创建一个横屏分时图页面 ](https://blog.csdn.net/jones2000/article/details/90453776) 
### 9. [HQChart使用教程9- 如何快速创建K线训练页面](https://blog.csdn.net/jones2000/article/details/90478687)
### 10. [HQChart使用教程10- 手机端页面设置的几个特殊属性](https://blog.csdn.net/jones2000/article/details/90727468)
### 11. [HQChart使用教程11-如何把K线数据API替换成自己的API数据](https://blog.csdn.net/jones2000/article/details/90747715)
 HQChart只提供前端行情展示， 不提供行情API数据，Demo中用到的数据都是测试数据，哪如何把这些测试数据替换成你自己的正式的行情数据呢。首先你要自己有一个Web API的服务器， 把我们前端调用数据的API地址域名换成你自己的WebAPI域名，（注意:必须和我们的请求接口和返回json格式一致才可以）
我们为每一个组件类都提供了一个SetDomain(domain, cacheDomain)的方法用来提供这个组件使用的数据接口域名。
domain 数据API的地址 (如果只是要换K线数据就替换这个API,）
cacheDomain 数据缓存的API地址（涉及到个股的财务，全市场的数据， 只要不是动态实时更新的， 我们都是使用缓存API来读取数据， 减少数据库压力，提高访问速度),如果你的指标里面没有用到这些数据， 就填第1个domain域名就可以了

### 12. [HQChart使用教程12-如何在K线图上添加弹幕](https://blog.csdn.net/jones2000/article/details/91125408)
### 13. [HQChart使用教程13-5分钟完成一个小程序K线图](https://blog.csdn.net/jones2000/article/details/91471252)
下载HQChart组件
https://github.com/jones2000/HQChart
把 \trunk\wechathqchart 目录拷贝到小程序项目的目录里面。

配置 WXML
新建一个小程序页面，
在WXML里面增加一个’canvas’元素， 并设置canvas-id, 定义好手势事件，bindtouchstart，bindtouchmove，bindtouchend
<view class="container">
  <canvas class="historychart"  canvas-id="historychart" 
    style="height:{{Height}}px; width:{{Width}}px;" 
    bindtouchstart='historytouchstart' bindtouchmove='historytouchmove' bindtouchend='historytouchend'/>

</view>
配置页面js文件
在对应的页面js文件里面 import HQChart组件 （wechathqchart/umychart.wechat.3.0.js）

### 14. [HQChart使用教程14-分析家语法执行器](https://blog.csdn.net/jones2000/article/details/93731637)
HQChart 内置一个分析家语法的执行器， 把分析家脚本通过词法，语法分析构建一个抽象语法树(AST)， 执行器通过AST执行， 最后获得数据。 
（具体细节我就不多讲了，很枯燥，有兴趣的可以看下“编译原理”， 都是一样的标准化流程）

### 15. [HQChart使用教程15-分析家语法执行器python版本](https://blog.csdn.net/jones2000/article/details/94738592)
### 16. [HQChart使用教程16-py中使用麦语言指标可视化](https://blog.csdn.net/jones2000/article/details/94920596)
### 17. [HQChart使用教程17- 多技术指标独立坐标叠加](https://blog.csdn.net/jones2000/article/details/95618901)
### 18. [HQChart使用教程18- K线截图](https://blog.csdn.net/jones2000/article/details/95738306)
### 19. [HQChart使用教程19-基于HQChart的后台单股票指标计算服务](https://blog.csdn.net/jones2000/article/details/96479448)
### 20. [HQChart使用教程20-单股票截面数据(财务数据)计算器](https://blog.csdn.net/jones2000/article/details/97135592)
### 21. [HQChart使用教程21-十字光标设置说明](https://blog.csdn.net/jones2000/article/details/97682466)
### 22. [HQChart使用教程22-如何创建移动筹码图](https://blog.csdn.net/jones2000/article/details/97928892)
### 23. [HQChart使用教程23-Y轴刻度显示设置](https://blog.csdn.net/jones2000/article/details/98320020)
### 24. [HQChart使用教程24-多语言设置](https://blog.csdn.net/jones2000/article/details/98734091)
### 25. [HQChart使用教程25- 叠加多个品种设置](https://blog.csdn.net/jones2000/article/details/98878463)
### 26. [HQChart使用教程26- K线图及走势图数据自动更新设置](https://blog.csdn.net/jones2000/article/details/99483328)
### 27. [HQChart使用教程27- 动态设置K线图指标模板](https://blog.csdn.net/jones2000/article/details/100079989)
### 28. [HQChart使用教程28-如何创建系统指标](https://blog.csdn.net/jones2000/article/details/100103486)
### 29. [HQChart使用教程29-走势图如何对接第3方数据1](https://blog.csdn.net/jones2000/article/details/100132357)
### 29. [HQChart使用教程29-走势图如何对接第3方数据2-最新分时数据](https://blog.csdn.net/jones2000/article/details/100149703)
### 29. [HQChart使用教程29-走势图如何对接第3方数据3-多日分时数据](https://blog.csdn.net/jones2000/article/details/100150842)
### 29. [HQChart使用教程29-走势图如何对接第3方数据4-叠加股票分时数据](https://blog.csdn.net/jones2000/article/details/100167703)
### 29. [HQChart使用教程29-走势图如何对接第3方数据4-异动提示信息](https://blog.csdn.net/jones2000/article/details/100516071)
### 29. [HQChart使用教程29-走势图如何对接第3方数据5-指标数据](https://blog.csdn.net/jones2000/article/details/102426337)
### 29. [HQChart使用教程29-走势图如何对接第3方数据6-websocket分钟数据](https://blog.csdn.net/jones2000/article/details/102568258)
### 30. [HQChart使用教程30-K线图如何对接第3方数据1](https://blog.csdn.net/jones2000/article/details/100181279)
### 30. [HQChart使用教程30-K线图如何对接第3方数据2-日K数据](https://blog.csdn.net/jones2000/article/details/100552022)
### 30. [HQChart使用教程30-K线图如何对接第3方数据3-1分钟K数据](https://blog.csdn.net/jones2000/article/details/100557649)
### 30. [HQChart使用教程30-K线图如何对接第3方数据4-流通股本数据](https://blog.csdn.net/jones2000/article/details/100574186)
### 30. [HQChart使用教程30-K线图如何对接第3方数据5-指标数据](https://blog.csdn.net/jones2000/article/details/100579223)
### 30. [HQChart使用教程30-K线图如何对接第3方数据6-分笔K线数据](https://blog.csdn.net/jones2000/article/details/100671849)
### 30. [HQChart使用教程30-K线图如何对接第3方数据7-日K数据分页下载](https://blog.csdn.net/jones2000/article/details/101275824)
### 30. [HQChart使用教程30-K线图如何对接第3方数据8-1分钟K线数据分页下载](https://blog.csdn.net/jones2000/article/details/101277092)
### 30. [HQChart使用教程30-K线图如何对接第3方数据9-BS(买卖)指标数据](https://blog.csdn.net/jones2000/article/details/101350429)
### 30. [HQChart使用教程30-K线图如何对接第3方数据10-如何绘制自定义线段或多边行指标数据](https://blog.csdn.net/jones2000/article/details/101694618)
### 30. [HQChart使用教程30-K线图如何对接第3方数据11-如何绘制多组自定义图标](https://blog.csdn.net/jones2000/article/details/101757384)
### 30. [HQChart使用教程30-K线图如何对接第3方数据12-如何在指标中绘制文字](https://blog.csdn.net/jones2000/article/details/101864046)
### 30. [HQChart使用教程30-K线图如何对接第3方数据13-使用websocket更新最新K线数据](https://blog.csdn.net/jones2000/article/details/102138784)
### 30. [HQChart使用教程30-K线图如何对接第3方数据14-轮询增量更新日K数据](https://blog.csdn.net/jones2000/article/details/102518334)
### 30. [HQChart使用教程30-K线图如何对接第3方数据15-轮询增量更新1分钟K线数据](https://blog.csdn.net/jones2000/article/details/102518422)
### 30. [HQChart使用教程30-K线图如何对接第3方数据16-日K叠加股票](https://blog.csdn.net/jones2000/article/details/102661873)
### 30. [HQChart使用教程30-K线图如何对接第3方数据17- 分钟K叠加股票](https://blog.csdn.net/jones2000/article/details/102887690)
### 30. [HQChart使用教程30-K线图如何对接第3方数据18-如何绘制自定义柱子](https://blog.csdn.net/jones2000/article/details/104417736)
### 30. [HQChart使用教程30-K线图如何对接第3方数据19-如何绘制彩色K线柱](https://blog.csdn.net/jones2000/article/details/104859784)
### 31. [HQChart使用教程31- 走势图异动数据设置](https://blog.csdn.net/jones2000/article/details/100191957)
### 32. [HQChart使用教程32-如何K线图显示自定义SVG矢量图标](https://blog.csdn.net/jones2000/article/details/100613634)
### 33. [HQChart使用教程33-如何在麦语法中自定义变量](https://blog.csdn.net/jones2000/article/details/100710615)
### 34. [HQChart使用教程34-如何在麦语法中自定义函数](https://blog.csdn.net/jones2000/article/details/100734839)
### 35. [HQChart使用教程35 - 如何在uni-app创建K线图(h5)](https://blog.csdn.net/jones2000/article/details/101039026)
### 36. [HQChart使用教程36 - 如何在uni-app创建走势图(h5)](https://blog.csdn.net/jones2000/article/details/101039673)
### 37. [HQChart使用教程37 - 如何在uni-app创建k线图(app)](https://blog.csdn.net/jones2000/article/details/101075683)
### 38. [HQChart使用教程38 - 如何在uni-app创建走势图(app)](https://blog.csdn.net/jones2000/article/details/101481960)
### 39. [HQChart使用教程39-指标中如何绘制文本分割线](https://blog.csdn.net/jones2000/article/details/101487482)
### 40. [HQChart使用教程40-如何自定义分钟周期或日线周期K线](https://blog.csdn.net/jones2000/article/details/101722958)
### 41. [HQChart使用教程41-分钟K线设置拖拽自动下载历史数据](https://blog.csdn.net/jones2000/article/details/102471720)
### 42. [HQChart使用教程42-K线图如何对接数字货币](https://blog.csdn.net/jones2000/article/details/102493905)
### 43. [HQChart使用教程43-日K线设置拖拽自动下载历史数据](https://blog.csdn.net/jones2000/article/details/102511317)
### 44. [HQChart使用教程44-uniapp使用条件编译同时支持h5,app,小程序](https://blog.csdn.net/jones2000/article/details/102529190)
### 45. [HQChart使用教程45-如何动态修改指标参数](https://blog.csdn.net/jones2000/article/details/102594672)
### 46. [HQChart使用教程46-分钟周期数据计算外部接口](https://blog.csdn.net/jones2000/article/details/102628045)
### 47. [HQChart使用教程47-如何自定义右键菜单](https://blog.csdn.net/jones2000/article/details/102720671)
### 48. [HQChart使用教程48-如何自定义X轴刻度](https://blog.csdn.net/jones2000/article/details/102741428)
### 49. [HQChart使用教程49-指标配置项说明](https://blog.csdn.net/jones2000/article/details/102928907)
### 50. [HQChart使用教程50-Y轴自定义刻度设置说明](https://blog.csdn.net/jones2000/article/details/103174483)
### 51. [HQChart使用教程51-指标切换按钮事件说明-h5版本](https://blog.csdn.net/jones2000/article/details/103187576)
### 52. [HQChart使用教程52- 自定义手机端K线图Tooltip](https://blog.csdn.net/jones2000/article/details/103820718)
### 53. [HQChart使用教程53- log日志输出控制](https://blog.csdn.net/jones2000/article/details/104122774)
### 54. [HQChart使用教程54- K线缩放控制按钮接口说明](https://blog.csdn.net/jones2000/article/details/104346016)
### 55. [HQChart使用教程55- 自定义PC端K线图Tooltip](https://blog.csdn.net/jones2000/article/details/104443471)
### 56. [HQChart使用教程56-内置品种对应后缀列表说明](https://blog.csdn.net/jones2000/article/details/104457569)
### 57. [HQChart使用教程57-如何调整K线的柱子缩放大小](https://blog.csdn.net/jones2000/article/details/104817724)
