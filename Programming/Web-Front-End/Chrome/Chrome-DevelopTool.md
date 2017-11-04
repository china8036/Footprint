- [Chrome 开发者工具使用](#chrome-%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8)
  - [DOM Breakpoints](#dom-breakpoints)
    - [Subtree Modifications](#subtree-modifications)
    - [Attributes Modifications](#attributes-modifications)
    - [Node Removal](#node-removal)
  - [XHR Breakpoints](#xhr-breakpoints)
  - [Console](#console)
    - [$(selector)](#selector)
  - [Timeline Tool](#timeline-tool)
  - [Chrome Profiles](#chrome-profiles)
  - [Refer Links](#refer-links)

# Chrome 开发者工具使用

## DOM Breakpoints

原文：[你可能不知道的 chrome 控制台：DOM 断点（属性、节点、内容变化监听）](http://frontenddev.org/article/you-may-not-know-the-chrome-console-dom-breakpoint-properties-node-content-changes-to-monitor.html)

### Subtree Modifications


子节点（内容、属性）修改通知.

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/8ae11932d7a84d3068816bff395b82c6.jpg)

```javascript
// 不会触发 Subtree Modifications
document.body.innerHTML = 'test';
document.body.innerHTML = 'test2';

// 会触发 Subtree Modifications
document.body.innerHTML = '<b></b>';
```

使用场景：常用在子节点内容发生变化后，来定位源码。


### Attributes Modifications

（当前节点）属性修改通知.

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/e9f878cf9fdf5b476a6e0d41346e253b.jpg)

```javascript
// 会触发 Attributes Modifications
document.body.id = 123;
document.body.setAttribute('id', 123);
document.body.setAttribute('id2', 456);

// 不会触发 Attributes Modifications
document.body.id2 = 456;
```

使用场景：节点的 className 等属性被修改后，来定位源码。

### Node Removal

（当前）节点移动（通知）.

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/c1c79ce9c6129017ce30f699a48e8262.jpg)

```javascript
// 第1次：不会触发
// 监听的是“子节点1”，并且它的位置也发生了变化，但这个变化是由“子节点2”导致的，所以不会触发通知回调
document.body.insertBefore(document.body.children[1], document.body.children[0]);

// 第2次：会触发
此时，会触发节点移动通知回调，实际操作的就是“子节点1”
document.body.insertBefore(document.body.children[1], document.body.children[0]);
```

使用场景：节点被移除了，定位源码。


## XHR Breakpoints

原文：[ 你可能不知道的 chrome 控制台：ajax xhr 断点](http://frontenddev.org/article/you-may-not-know-the-chrome-console-ajax-xhr-breakpoints.html)

通过监听 xhr 的断点，可以轻而易举的找到事件的触发点和调用堆栈，这一点和上一节说到的 DOM 断点几乎是一样的，只不过此处换成了 xhr 了而已。

打开 chrome 浏览器控制台：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/6330c035f1ee5499fae9b843b7def07f.jpg)


例：

添加“baidu.com”:

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/569dcd67847268695c1f7e4bdccbdbd2.jpg)

在输入框输入搜索词之后，触发了 XHR 断点，美化代码之后，可以很明显的看到，xhr 断点直接暂停在xhr.send这一步，通过上下文分析，可以看到当前请求的一些参数，如a.uri。


## Console

### $(selector)

即使当前页面没有加载jQuery，你也依然可以使用$和$$函数来选取元素，实际上，这两个函数只是对document.querySelector()和document.querySelectorAll()的简单封装，$用于选取单个元素，$$则用于选取多个。

除了$_，你还可以使用$0,$1,$2,$3,$4这5个变量来引用最近选取过的5个DOM元素。 $0 为Elements HTML 面板中选中的元素。 $1 为上一次在 HTML 面板中选中的元素。 $2、$3、$4 同样的。不过只能到$4。


## Timeline Tool

https://developers.google.com/chrome-developer-tools/docs/timeline

http://frontenddev.org/link/the-timeline-panel-of-the-chrome-developer-tools.html


## Chrome Profiles

原文：[JS内存泄漏排查方法——Chrome Profiles](http://frontenddev.org/link/js-memory-leak-screening-method-chrome-profiles.html)




## Refer Links

https://developers.google.com/web/tools/chrome-devtools/

http://frontenddev.org/column/chrome-development-tools-using-guide/

http://frontenddev.org/link/chrome-debugging-tools-commonly-used-functions.html



