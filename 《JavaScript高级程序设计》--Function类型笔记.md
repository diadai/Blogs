函数实际上是对象，每个函数都是function类型的实例，而且都与其他引用类型一样具有属性和方法。

**函数定义**

```
    function f1(a,b) {
        return a+b;
    }                   //函数声明
```

```
    var sum = function(a,b) {
        return a+b;
    };                  //函数表达式
```
在使用函数表达式定义函数时，没有必要使用函数名

```
    var f1 =new Function('a','b','return a+b'); //这种不推荐
```
因为这种语法会导致解析两次代码，，第一次是解析常规的ECMAScript代码，第二次是解析传入构造函数中的字符串。影响性能。

**函数是对象，函数名是指针**

```
    function f1(a,b) {
    return a+b;
    }
    alert(f1(2,3));   //5

    var f2=f1;
    alert(f2(2,3));   //5

    f1=null;
    alert(f2(2,3));   //5
```
由于函数名是指针，所以在`f2=f1` 时，f2和f1都指向同一个函数，因此f2();也能调用并返回结果，将`f1=null` 也只是减少了一个指向函数的名字而已，f2()仍然可以正常调用，（不带圆括号的函数名，如f2，是访问函数指针，而f2()才是调用）。一个函数有多个名字也没有什么错。

**函数重载**

ECMAScript中没有函数重载的概念

```
   function f3(num) {
      return num + 10;
   }
   function f3(num) {
      return num + 20;
   }
   var  result=f3(10);  // 30
```
虽然有两个同名的函数，实际上其实是第二个覆盖第一个函数，从函数的[预解析](http://blog.csdn.net/erdaidai/article/details/77460798") 也可以理解这个是覆盖，而不是重载。

**作为值的函数**

在ECMAScript中的函数名本身就是变量，所以函数也能作为值来使用，不仅可以像传递参数一样把一个函数传递给另外一个函数，而且可以将一个函数作为另外一个函数的结果返回。

```
function createComparisonFunction(propertyName) {
    return function (object1,object2) {
        var value1 = object1[propertyName];    //用方括号表示法来取得给定属性的值
        var value2 = object2[propertyName];

        if (value1 < value2){
            return -1;
        }else if (value1 > value2){
            return 1;
        }else {return 0;}
    };
}
var data = [{name:'zhangsan',age:28},{name:'lisi',age:29}];

data.sort(createComparisonFunction('name'));
alert(data[0].name);    // lisi

data.sort(createComparisonFunction('age'));
alert(data[0].name);   // zhangsan
```
这是书上的原例，向`createComparisonFunction(propertyName)` 传递一个属性变量，在返回比较后的数值，由sort()方法根据数值比较大小，返回对应的属性值。

**函数内部属性**

函数内部有两个特殊的对象：arguments和this。其中arguments有个属性叫callee，该属性是一个指针，指向拥有这个arguments对象的函数。
下面是金典的阶乘函数：

```
       function factorial(num) {
            if (num <=1){
                return 1;
            }else {
                return num*arguments.callee(num-1); //消除下面紧密耦合现象
             // return num*factorial(num-1); 耦合紧密
            }
        }
        alert(factorial(3)); // 6
```
在编写程序中，尽量消除函数耦合现象，这有利于查看和维护代码。

还有一个函数对象属性：caller，这个属性中保存着调用当前函数的引用，如果是在全局作用域中调用当前函数，它的值为null。

```
        function outer() {
            inner();
        }
        function inner() {
            alert(arguments.callee.caller);  //更松散的耦合
         // alert(inner.caller);  和上行代码是等价的
        }
        outer();
```
这个结果会返回outer()函数的源代码（完整的代码）
