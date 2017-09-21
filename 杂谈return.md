先看个例子：
```
       function foo1() {
            return{
                bar:"hello"
            };
        }
        function foo2()
        {
            return
            {
                bar:"hello"
            };
        }
        console.log(foo1());   //{bar: "hello"}
        console.log(foo2());   //undefined
```
 看见上面的代码注释，想想为什么是这样的呢，先看他们的区别就是return后面的“{}”一个是在同一行一个是换了一行，那么我们看见这个就能想到return有个特点就是：其后面同一行内跟的值就返回，要是换了一行的话，return后面会自动补一个分号，那么下一行的内容就不会再执行
（1）因为return标志着函数的结束，其后面的内容不执行。
（2）并且return没有明确返回值的话，就会默认返回一个undefined。
（3）用函数加括号就等于函数内return的值。