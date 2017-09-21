我们知道sort()方法传参数的话，只能是函数参数，但是我们会困惑，

```
       function compare(value1,value2) {
            console.log(value1,value2,arr);
            if(value1<value2){return -1;}
            else if(value1>value2){return 1;}
            else {return 0}
        }
```
 像上面的compare函数，被调用的话，也是返回的值，-1，1，0，那我们直接像sort(-1)写，会发生什么呢？下面我测试了一下

```
        var arr=[1,3,10,4,2,5];
        arr.sort(-1);
        alert(arr);
```
我用了三个浏览器测试，其中分别是谷歌、火狐、IE，三者结果是有不一样的

（1）谷歌:
![这里写图片描述](http://img.blog.csdn.net/20170921214550134?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
谷歌这个没有报错，结果不是原来的数组，是有变动的，如图结果是arr=[1,10,2,3,4,5],其实这个结果和`arr.sort();`是一样的，但是和传入函数参数返回值为-1的结果不一样。

（2）IE：
![这里写图片描述](http://img.blog.csdn.net/20170921215045164?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
IE直接报了一个错误，说参数不是JavaScript对象

（3）火狐：
![这里写图片描述](http://img.blog.csdn.net/20170921215152492?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
火狐也是报了错误

再来是测试一下1。

```
        var arr=[1,3,10,4,2,5];
        arr.sort(1);
        alert(arr);
```
三个浏览器的反应和上面的一样，谷歌不报错，但是结果不对，火狐和IE报错。

测试0，结果还是和上面一样。

根据火狐MDN文档错误分析得出错误原因是没有给出正确的参数，**参数只能是undefined和操作比较的函数参数。**

其实对于**数值类型或者valueOf()方法会返回数值类型的对象类型**，可以使用一个更简单的比较函数，这个函数只要用两个值相减即可。

```
        var arr=[1,3,10,4,2,5];
        function compare(value1,value2) {
            return value1-value2;
        }
        arr.sort(compare);
        alert(arr);
```
要升序就第一个减第二个，要降序就第二个减第一个。

其实我现在只是知道sort()原理和用法，但是不知底层实现机制，我查过很多资料，js当中的方法都是被封装在浏览器的引擎当中，有兴趣的可以查看V8引擎的源码，也有sort()这一块，（我是没有看懂）这是链接。

https://github.com/v8/v8/blob/master/src/js/array.js#L726