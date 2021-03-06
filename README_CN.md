# flutter_pulltorefresh

## 介绍
一个提供上拉加载和下拉刷新的组件,同时支持Android和Ios
(仍然处于开发阶段中,功能善未完全)

## 特性
* 同时支持Android,IOS
* 提供上拉加载和下拉刷新
* 几乎适合所有的部件,例如GridView,ListView,Container,Text...
* 高度扩展性和很低的限制性
* 灵活的回弹能力


## 截图
![](arts/screen1.gif)
![](arts/screen2.gif)<br>
1.1.0(开始支持翻转下的ScrollView)
![](arts/screen3.gif)<br>

## 我该怎么用?
1.第一步,在你的pubspec.yml声明

```

   dependencies:
     pull_to_refresh: ^1.1.2
     
```

2.然后,导入,SmartRefresher是一个组件包装在你的外部,child就是你的内容控件

```


   import "package:pull_to_refresh/pull_to_refresh.dart";
     ....

     build() =>

      new SmartRefresher(
          enablePullDown: true,
          enablePullUp: true,
          onRefresh: _onRefresh,
          onOffsetChange: _onOffsetCallback,
          child: new Container(
            color: Colors.white,
            child: new ListView.builder(
              physics: const NeverScrollableScrollPhysics(),
              shrinkWrap: true,
              itemExtent: 40.0,
              itemCount: data.length,
              itemBuilder: (context,index){
                return data[index];
              },

            ),
          )
      )

```

3.你应该要根据不同的刷新模式状态下,显示不同的布局.当然,
 我这里已经构造了一个指示器方便使用,叫做ClassicIndicator,
 如果不符合要求,也可以选择自己定义一个指示器

```


    Widget _buildHeader(context,mode){
     return new ClassicIndicator(mode: mode);
    }


    Widget _buildFooter(context,mode){
      // the same with header
      ....
    }

    new SmartRefresher(
       ....
       footerBuilder: _buildFooter,
       headerBuilder: _buildHeader
    )



```

4.
无论是顶部还是底部指示器,onRefresh都会被回调当这个指示器状态进入刷新状态。
但我要怎么把结果告诉SmartRefresher,这不难。内部提供一个Controller,通过contrleer.
sendBack(int status)就可以告诉它返回什么状态。

```

  void _onRefresh(bool up){
  		if(up){
  		   //headerIndicator callback
  		   new Future.delayed(const Duration(milliseconds: 2009))
                                 .then((val) {
                                   _refreshController.sendBack(true, RefreshStatus.failed);
                             });

  		}
  		else{
  			//footerIndicator Callback
  		}
      }
  
```

5.如果你的内容控件是一个滚动的部件, 例如 ScrollView, ListView, GridView 和 so on, 你应该给这类控件分配两个属性.因为我内部封装是通过ListView来封装的,
这非常重要,如果你不注意这个,你会遇到某些问题.

```

new ListView(){
    physics: const NeverScrollableScrollPhysics(),
    shrinkWrap: true,
    child:...
}

```

## 属性表

| Attribute Name     |     Attribute Explain     | Parameter Type | Default Value  | requirement |
|---------|--------------------------|:-----:|:-----:|:-----:|
| child      | 你的内容部件   | Widget   |   null |  必要
| headerBuilder | 头部指示器构造  | (BuildContext,RefreshMode) => Widget  | null | 如果你打开了下拉是必要,否则可选 |
| footerBuilder | 尾部指示器构造     | (BuildContext,RefreshMode) => Widget  | null | 如果你打开了上拉是必要,否则可选 |
| enablePullDown | 是否允许下拉     | boolean | true | 可选 |
| enablePullUp |   是否允许上拉 | boolean | false | 可选 |
| onRefresh | 进入刷新时的回调   | (bool) => Void | null | 可选 |
| onOffsetChange | 它将在拖动时回调（除了刷新状态和完成状态,范围在:0~实际距离/triggerDistance   | (double) => Void | null | 可选 |
| controller | 控制内部状态  | RefreshController | null | optional |
| headerConfig |  这个设置会影响你使用哪种指示器,config还有几个属性可以设置   | Config | RefreshConfig | optional |
| footerConfig |  这个设置会影响你使用哪种指示器,config还有几个属性可以设置     | Config | LoadConfig | optional |
| enableOverScroll |  越界回弹的开关,如果你要配合RefreshIndicator(material包)使用,有可能要关闭    | bool | true | optional |



## 注意点
1.组件是无界的，所以当你使用它时，要小心由高度引起的问题，特别是Column、Stack，这也是对无限制高度的控制，要格外小心。
 
## 开源协议
 
 ```
 
MIT License

Copyright (c) 2018 Jpeng

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

 
 ```