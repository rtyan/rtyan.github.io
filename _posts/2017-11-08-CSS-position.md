### CSS中的定位

#### 引言
CSS定位，一个看似简单却又很难得问题，说他简单是因为大部分前端相关的、甚至后端人员都会接触到他，
说他难是因为似乎只有小部分人能真正彻底的搞懂他们，这里整理下相关的知识，一方面自己记录下，另一
方面也分享给大家，写的不好的地方，请斧正。

#### static定位
`position:static`是元素在文档流中的默认定位，一般不会显示写出来，他会按照先后顺序排列元素。

#### fixed 定位
`position: fixed`，跟他的名字一样，使用这个定位，可以让元素*钉*在屏幕可视区域中，定位也是
相对于可视区域定位的。

#### relative相对定位
`position:relative`，文档脱离文档流，但文档原本的位置还会占用，此时可以使用`left: 20px; top: 100px`
对元素定位，这个偏移是以元素原本的位置为参考的。

[点击查看](http://en.jsrun.net/fPiKp/edit)relative定位的例子

#### absolute 绝对定位
`position: absolute`，文档脱离文档流，并删除元素占用的文档流，此时可以使用`left: 20px; top:100px`
对元素进行定位，这个偏移是以离元素最近的一个*有定位(relative,absolute,fixed)的父元素*为参考的。

[点击查看](http://en.jsrun.net/2PiKp/edit)absolute定位的例子
