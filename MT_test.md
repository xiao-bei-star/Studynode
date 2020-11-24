### 美团面试题分析

#### 简答题

##### 1. 问:以下变量i,s,a在堆上还是栈中?最后执行a.i的值是多少?
```     
class A {
    String i = "op";
    void func(String s) {
        s = ""+9;
    }
    static void test() {
        A a = new A();
       a.func(a.i);
    }
} 
```
+ **考察点**
  栈内存
       栈内存主要用于存储各种基本类型的变量，包括Boolean、Number、String、Undefined、Null以及对象变量的指针

  堆内存
  
  堆内存主要存储object对象
  
  **注意：A是new出来的对象，存储在堆内存中，对应地址是指针a存的内容，所以说a还是在栈内存中**
  
  参数传递（考察按值传递）

  ECMAScript中所有函数的参数都是按值传递的

+  **题目分析**
        i,s,a表示的依旧是基本类型的变量，所以还是存储栈内存中
    
       a是A的一个实例，所以a.i='op',执行a.func(a.i)函数，相当于传入'op'参数，该函数会赋值一个变量，两个变量是完全独立的，s=''+9只是在复制的变量上执行的，函数体外的a.i并没有改变
    
----
##### 2. 请按顺序写出打印结果，并说明原因。
```
var name = 'global';
var obj = {
    name: 'local',
    foo: function(){
        this.name = 'foo';
    }.bind(window)
};
var bar = new obj.foo();
setTimeout(function() {
    console.log(window.name);
}, 0);
console.log(bar.name);
 
var bar3 = bar2 = bar;
bar2.name = 'foo2';
console.log(bar3.name);
```
+ **考察点**
Event Loop

     同步异步不同情况，以及定时器的使用
  
  绑定
  
     不同绑定优先级的问题
  
  引用类型赋值
  
     复杂类型值的赋值是引用赋值，均指向同一个对象
  
+  **题目分析**
        'foo'    //bind返回的是一个函数，这个函数是绑定在window上的，通过new对该函数进行了构造，返回了一个新对象，函数体中的this指向该对象，bind为硬绑定，new绑定的优先级高于硬绑定，所以this还是绑定在ban上，bar.name=this.name='foo'
        'foo2'   //var bar3=bar2=bar;复杂类型值赋值时引用赋值，bar3,bar2,bar指向的都是同一个对象，所以对bar2进行修改的时候，bar3和bar的内容也会被修改掉，所以bar3.name=bar2.name='foo2'
        'global' //setTimeout设置了一个定时器，会将回调的执行放在所有事件的末尾执行，不可以插队，所以console.log(window.name)输出的是全局下的name='global'

-----
##### 3. 问:请写出如下代码运行后产生的结果，并给出解释，说明结果是如何得出的。

```
setTimeout(() => console.log('a'));
Promise.resolve().then(
   () => console.log('b’);
 ).then(
   () => Promise.resolve('c').then(
     (data) => {
       setTimeout(() => console.log('d'));
       console.log('f');
       return data;
     }
   )
 ).then(data => console.log(data));
```

+ **考察点**

  promise（宏任务）和setTimeout（微任务）的执行先后顺序

+ **题目分析**

  执行顺序：Promise——>其后的（b）——>同步的（f）

                   .resolve异步的（c）
        
                   .JS执行原理从上到下的（a）
        
                   .then()——>setTimeout异步的（d）

---
##### 4.请写出下面ES6代码编译后所生成的ES5代码
``` 
class Person {
     constructor (name) {
          this.name = name;
     }
     greet () {
          console.log(`Hi, my name is ${this.name}`);
     }
     greetDelay (time) {
          setTimeout(() => {
               console.log(`Hi, my name is ${this.name}`);
          }, time);
     }
}
```

+ **考察点**

  ES6语法理解，JS原型的理解，this指向的理解

+ **题目分析**

  ES5使用JS原型来进行输出，以及this在window下的定义，需要中间变量的传递
  
  greet（）和greetDelay（）两个函数其实就是Person原型链下的两个实例函数对象
  
  ---
  
##### 5.形如1, 1, 2, 3, 5, 8, 13, 21, 34, 55的数列，后一位是前面两位相加（斐波那契数列），写出函数要求找到第 N 位是多少，如：fib(3) => 3 ， fib(5) => 8, 要求时间复杂度为O(n)

```
 if(n == 0 || n == 1) cout << 1 << endl;
    else{
        long long a = 1, b = 1;
        for(int i=2; i<=n; i++){
            b = a + b;
            a = b - a;
        }
        cout << b << endl;
```

```
fib(0)=fib(1)=1;后面的数就是前两个数相加之和，相当于b是输入值的输出，而通过交换可以让a的值变化为第n-1给结果来进行输出；还有一个关键点就是要给这两个变量什么类型，会不会存在放不下溢出问题。
```

---

##### 6.代码实现金钱转换，假设现有100元的商品，而代金券有50元、30元、20元、5元四种，则最佳优惠是两张50元面额的代金券；而如果现有65元的商品，则最佳优惠是两张30元代金券以及一张5元代金券。

```
 for(int p=0;p<size;++p){
            for(int q=0;q<size-p-1;++q){
                if(coins[q]>coins[q+1]){
                    swap(coins[q],coins[q+1]);
                }
            }
        }
        int *F=new int[cash+1];
        for(int i=0;i<=cash;++i){
            F[i]=cash+1;
        }
        F[0]=0;
        for(int j=0;j<=cash;++j){
            for(int k=0;k<size;++k){
                if(j<coins[k]){
                    break;
                }
                else{
                    F[j]=min(F[j-coins[k]]+1,F[j]);
                }
            }
        }
```

```
这个题想法是，从面值最大的开始设置变量计算，一点一点再从面值小的里面选择
```

---

###### 7.给定一个包含非负整数的 M x N 迷宫，请找出一条从左上角到右下角的路径，使得路径上的数字总和最小。每次只能向下或者向右移动一步。

```
for(int i=0;i<M;i++){
        for(int j=0;j<N;j++){
            cin>>a[i][j];
        }
    }
    for(int i=M-1;i>=0;i--){
        for(int j=N-1;j>=0;j--){
            if(i+1<M && j+1<N)
            {
                a[i][j] += min(a[i+1][j],a[i][j+1]);
            }
            else if(i + 1< M)
            {
                a[i][j] += a[i+1][j];
             
            }else if(j + 1 < N)
            {
                a[i][j] += a[i][j+1];
            }
        }
    }
```

```
这个题考察的是二维数组，首先是要先把给的数组存到二维数组中，然后没走一步都要进行判断，向下向右那个值小，走值小的那个部分（但还是要进行预先判断），每走一步就把所走的路程存到数组中，然后通过对数组的内容相加来显示输出最短距离。
```

---

