
                 关于怎样判断NaN的一些方法

  看见了这样一个题目：找出下面当中的是NaN的位置
  ![一些类型](http://img.blog.csdn.net/20170805163943696?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  那么首先肯定是要找出当中哪些是NaN,判断NaN时最重要的是先要知道NaN是数字类型，其意思是 not a number,就是不是数字的数字类型。（数字和数字类型是两个概念）

  NaN与自己都不相等，这个时候就不能用‘===’来判断，那我们用全局函数isNaN()，但是这个函数存在一个问题，当函数里面是字符串的时候，就会返回true，因为isNaN()的实现原理是先用Number()把里面内容转换成数字类型，当里面的内容转出来是数字的，那么isNaN()的结果就是false，不是数字是其他的话，返回值就是true，但是Number已经把里面的东西转换成数字类型了，所以出来的只要不是数字那么都会是NaN。

为解决这个问题我们可以这样做：
1：在使用isNaN()之前先检查一下这个值是不是数字类型，这样就避免了隐式转换的问题：

```
 function f1(value) {

        if (typeof value==='number'&&isNaN(value)) {
           alert('这是NaN');
        }else{
            alert('这不是NaN');
        }
    }
    f1('abc'-1);
```
2:用自身特性，与自己不相等来判断

```
 function f1(value) {

        if (value!=value) {
           alert('这是NaN');
        }else{
            alert('这不是NaN');
        }
    }
    f1('abc');
```
由这两个方法就可以判断出上面问题中的哪些是NaN，有三个，他们分别是：'abc'-6 , '200px'-30 , Number('abc')
注意:'abc'-6 中的减号是隐式转换为数字类型。还有一些隐式转换：

![隐式转换](http://img.blog.csdn.net/20170805172634369?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)