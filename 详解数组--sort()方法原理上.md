sort()方法在适当的位置对数组进行排序，并且返回数组。

对于sort()方法，W3school给的定义是
```
arrayObject.sort(sortby)
```
![W3school的定义](http://img.blog.csdn.net/20170921084800521?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可知参数是可选的，也就是有两种情况，一是不加参数，另外一种是加**函数**参数，参数必须是函数。函数就是比较函数。

先看第一种不加参数的情况：

```
        var arr=[1,3,10,4,2];
        arr.sort();
        alert(arr);   //1,10,2,3,4
```
**默认排序顺序是升序**，在上面的例子当中，我们感觉排序的结果是：1，2，3，4，10。怎么出来10反而在前面呢？**那是因为sort()排序是按照字符串的Unicode码**，10的比后面的小，则他在前面，但是这个结果不是我们想要的，那怎么办？此时我们就要用到第二种情况：加函数参数。

第二种加函数参数的情况：

```
        var arr=[1,3,10,4,2];
        function compare(value1,value2) {
            if(value1<value2){return -1;}
            else if(value1>value2){return 1;}
            else {return 0}
        }
        arr.sort(compare);
        alert(arr)   //1,2,3,4,10
```
按照我的理解是，在对于一个数组使用sort()方法时，会逐项的将数组中的元素传入到compare函数中作为参数，也就是说在上面 var arr=[1,3,10,4,2]使用sort()方法时，按照顺序先把1，3分别传给value1和value2，再比较1和3的大小，显然1<3，那么根据判断函数返回-1；sort()方法在接受到函数返回的-1时，就做出按照升序的原理不换位置，现在虽然数组位置没有变，但是数组是刷新过一边的，sort()返回了一个新的数组，所以在第二次传入参数的时候，是传入的第二项3和第三项10（第一次换位置的话，那么第二项就是换过后的第二项）按着同样的原理进行比较返回-1；后面的步骤就一样了，在第三次比较的时候要注意，如果前面的返回值为-1，那么新数组的第一项和第二项不用再比较（升序，-1就不比较，因为-1表示大数本来就在后面 ），如上面所示：1<3,3<10,那么1<10;如果为1，比如，2<5,5>1第二个返回值是1，则排完的新数是：2，1，5；所以接下来就是2和1比较，最终的1，2，5......所以上面的结果是1,2,3,4,10

接下来我用一个例子来说明这个过程：

```
      var arr=[1,3,10,4,2,5];
      function compare(value1,value2) {
            console.log(value1,value2,arr);
            if(value1<value2){return -1;}
            else if(value1>value2){return 1;}
            else {return 0}
        }
        arr.sort(compare);
        alert(arr);   //1,2,3,4,5,10
```
这段代码在第三行加了`console.log(value1,value2,arr);`为了更好理解参数传入和比较的过程，下面图是输出结果，可能上面讲的那段话，有的人觉得繁琐，那么接下来我用段代码逐行分析。
![这里写图片描述](http://img.blog.csdn.net/20170921174820700?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
(*我本来是想在控制台输出数组，结果出现这种重复以及丢失的排序，目前还没有解决，提出来让大家知道，以下排序过程是对的，只是控制台输出有误*。)

我们定义的数组是这样的 arr=[1,3,10,4,2,5]
第一行：传入的是value1=1，value2=3，比较大小返回-1，数组的位置不变。
第二行：传入的是value1=3，value2=10，比较大小返回-1，数组的位置不变。
第三行：传入的是value1=10，value2=4，比较大小返回1，两项交换位置，现在的数组是arr=[1,3,4,10,2,5]
第四行：现在不是继续往后面比较，由于返回值为1（这是升序，1的时候往前面比较），则在第三行操作交换位置后，这传入的参数是value1=3，value2=4，比较大小，返回-1，位置不变，数组还是arr=[1,3,4,10,2,5]
第五行：这才继续向后比较，传入value1=10，value2=2，比较大小，返回1，交换位置，数组为arr=[1,3,4,2,10,5]
第六行：前面的返回值又是1，则又比较前面的，value1=4，value2=2，比较大小，返回1，数组变为arr=[1,3,2,4,10,5]
第七行：第六行结果返回值是1，所以这个地方也不能往后面比较，value1=3，value2=2；比较大小，返回1，数组为arr=[1,2,3,4,10,5]
第八行：同理，value1=1，value2=2，比较大小返回-1，数组不变。
第九行：继续往后面比较，value1=10，value2=5，比较大小，返回1，改变位置，数组是arr=[1,2,3,4,5,10]
最后一行：前面返回值是1，则向前比较，value1=4，value2=5，比较大小，返回-1，数组不变，（因为前面4前面的数是排好序的，只要5>4，就会比前面都大，则前面不用比较。）

传参的详细过程就是这样，所以最后得到的数组是：arr=[1,2,3,4,5,10]，上面是谷歌测试。

这是火狐的，结果是一样的，传参是一样的，但是控制台输出的数组没有变过
![火狐测试](http://img.blog.csdn.net/20170921162954481?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这是IE的，IE给value1，value2传值的顺序和谷歌，火狐是相反的，并且它的控制台不输出arr数组，但是它的结果还是一样的。
![这里写图片描述](http://img.blog.csdn.net/20170921163146538?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我又把上述代码里的数组改了：`   var arr=['a','C','b','D','d'];`
![测试2](http://img.blog.csdn.net/20170921160941206?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
再改成：` var arr = ['réservé', 'premier', 'cliché', 'communiqué', 'café', 'adieu'];`

谷歌这个结果是对的，但是控制台在输出步骤当中出了一点错误（我圈出来的重复出现，还有一个不在了，但是结果是对的，目前还没有解决）
![这里写图片描述](http://img.blog.csdn.net/20170921172914933?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

火狐的控制台输出数组没变，但是结果是一样的
![这里写图片描述](http://img.blog.csdn.net/20170921173327441?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170921173600722?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZXJkYWlkYWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

IE的结果也是一样的。
这个例子说明**字符串也是可以用这个方法来排序的**，
**《JavaScript高级程序设计》上面说，compare函数适合大多数数据类型。**

