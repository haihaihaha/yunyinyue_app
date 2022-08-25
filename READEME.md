# 关于
作者：Yang
时间：2020-8-15
项目说明：实现了仿网易云音乐播放器微信小程序
## 优化
1.目前接口都是借助尚硅谷中间服务器向网易发请求并整理后返回的数据，在运行时，还要开启服务器。但该接口并不稳定，还是要熟悉网易的接口，将接口改成网易的正式接口
2.获取视频列表数据要先登录
3.video 视频下的模块未完成

## 其他
1.输入框一般用防抖，下拉上拉用节流
2.小程序 常规分包
  --分包
  --修改配置
  --注意路由要修改

## 笔记
### .wxml
swiper  微信轮播图组件
      里面只能用swiper-item
      属性：
        indicator-dots：是否显示面板指示点 不写默认flase ；声明后为true，可以不赋值为tre 
        indicator-color：设置点的颜色
        indicator-active-color：设置选中点的颜色
        circular--是否采取衔接滑动
        next-margin--露出后面的
        previous-margin--露出前面的

scroll-view 可滚动的视图区域
        需要设置固定高度
        属性：
          enable-flex：启用flexbox布局
          scroll-x：允许横向滑动

wx:for 
        默认数组当前项下标变量名为index，变量名为item
        属性：         
          wx:key--指定当前元素下标变量名
          wx:for-item--指定当前元素变量名

自定义组件 NavHeader 
      要使用先注册--在使用的文件夹的json的usingComponents里注册组件

<block>...</block>
    将里面的内容变为块
    做好事不留名

### wxss==css
  /* 设置文本位于图片居中位置 */
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -150rpx;
  margin-top: -50rpx;

  /* 使子元素相对于父元素水平垂直居中  父元素要设置position: relative;*/
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;

设置超出文本隐藏 
    1.设置最大宽度
    2.设置不允许换行
    3.超出文本隐藏
    4.用省略号带代替

  /* 单行文本溢出隐藏 省略号代替 */
  max-width: 400rpx;
  display: block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

  /* 多行文本溢出隐藏 省略号代替*/
  overflow: hidden;/*超出部分隐藏*/
  text-overflow: ellipsis;/*用省略号代替*/
  display: -webkit-box;
  -webkit-box-orient: vertical; /*设置对其模式*/
  -webkit-line-clamp: 2; /*设置多行的行数*/

  /* 当一个元素设置为flex，其子元素会自动成为block（块）元素*/

  flex-grow: 可拉伸 
  flex-shrink： 可压缩 
  flex-basis: 当前元素的宽度
  flex默认值: flex-grow: 0, flex-shrink: 1, flex-basis: auto
  flex:1: flex-grow: 1, flex-shrink: 1, flex-basis: 0%
  flex:auto :flex-grow: 1, flex-shrink: 1, flex-basis: auto
  flex: 1会导致父元素宽度自动为100%

  calc: 可以动态计算css的宽高， 运算符左右两侧必须加空格，否则计算会失效 */
  /* 视口单位： vh vw  1vh = 1%的视口高度  1vw = 1%的视口宽度

  ？？？
  filter
  z-index
  动画：translate  animation
  @keyframes: 设置动画帧
    1) from to
      - 使用于简单的动画，只有起始帧和结束帧
      - 北京 - 上海  直达
    2) 百分比
      - 多用于复杂的动画，动画不止两帧
      - 北京 - 上海 ---> 北京 -- 天津 --- 深圳 --- 上海
      - 0% - 100%, 可以任意拆分


### .js
生命周期函数
    onLoad --监听页面加载
    onShow --监听页面显示
    onReady --监听页面初次渲染完成

导航跳转API
    wx.navigateTo

函数节流|防抖
封装请求
/*
* 1. 封装功能函数
*   1. 功能点明确
*   2. 函数内部应该保留固定代码(静态的)
*   3. 将动态的数据抽取成形参，由使用者根据自身的情况动态的传入实参
*   4. 一个良好的功能函数应该设置形参的默认值(ES6的形参默认值)
* 2. 封装功能组件
*   1. 功能点明确
*   2. 组件内部保留静态的代码
*   3. 将动态的数据抽取成props参数，由使用者根据自身的情况以标签属性的形式动态传入props数据
*   4. 一个良好的组件应该设置组件的必要性及数据类型
*     props: {
*       msg: {
*         required: true,
*         default: 默认值，
*         type: String
*       }
*     }
*
* */

### .json
配置顶部标题
"navigationBarTitleText": "登录"

## 其他
  说明: 登录流程
  1. 收集表单项数据
  2. 前端验证
    1) 验证用户信息(账号，密码)是否合法
    2) 前端验证不通过就提示用户，不需要发请求给后端
    3) 前端验证通过了，发请求(携带账号, 密码)给服务器端
  3. 后端验证
    1) 验证用户是否存在
    2) 用户不存在直接返回，告诉前端用户不存在
    3) 用户存在需要验证密码是否正确
    4) 密码不正确返回给前端提示密码不正确
    5) 密码正确返回给前端数据，提示用户登录成功(会携带用户的相关信息)