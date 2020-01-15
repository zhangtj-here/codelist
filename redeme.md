这篇文章只是用来进行总结思路的一篇文章，用来锻炼表达的，非展示性的文章
## 需求描述 

* 初始值为1
* 运算的另一个值为2
* 然后根据获取到的运算字符串进行累计运算
* 运算字符串只有加号和减号组成
* 输出运算结果 
## 实现思路

* 用一个输入框来获取运算字符串，输入框进行控制，只能输入加减运算符
* 根据你输入的运算符字符串来识别各个运算符，然后进行累计运算
* 当删除之前输入运算符时，进行相应的结果展示 
* 用一个h1标题来展示运算结果 
## 根据思路展示代码

### html部分
```
<input id="message" type="text" placeholder="请输入算法法则" oninput="demo.input(this)" >
<h1 id="result">1</h1>
```

* 这是html代码，需要一个获取运算符的input标签和展示结果h1标签
* input需要绑定input事件，及时调用函数，展示运算结果

### js部分
#### 总体 
```
class Demo {
  constructor () {
    this.state = true;
    this.a1 = 1;
    this.a2 = 2;
    this.result = this.a1;
    this.len = 1;
    this.stack = [1];
    this.fuhao = ["+", "-"];
  }
  input (e) {
    let string = document.getElementById('message').value;
    let str = string.split('');
    let fuhao = str[str.length - 1];
    if (this.len > str.length + 1) {
      this.tanzhan();
      return;
    }

    if (!this.fuhao.includes(fuhao)){
      e.value = e.value.replace(/[^(\+\-)]/g,'');
      return;
    } 

    this.yunsuan(fuhao);
  }

  tanzhan () {
    this.stack.pop();
    this.result = this.stack[this.stack.length -1];
    this.len = this.stack.length;
    document.getElementById('result').innerHTML = this.result;
  }

  yunsuan (fuhao) {
   /*  if(fuhao == '+') {
      this.add();
      this.stack.push(this.result);
    }
    if(fuhao == '-') {
      this.substraction();
      this.stack.push(this.result);
    } */
    fuhao == "+" ? this.add() : this.substraction();
    this.stack.push(this.result);
    this.len = this.stack.length;
    document.getElementById('result').innerHTML = this.result;
  }

  add (a1, a2) {
    // return a1 + a2;
    this.result += this.a2; 
  }

  substraction (a1, a2) {
    // return a1 - a2;
    this.result -= this.a2; 
  }

}
    const demo = new Demo();
```

* 这是总的代码，可以进行总体的概括
* 下面进行拆解

#### 变量部分
```
constructor () {
    this.state = true;
    this.a1 = 1;
    this.a2 = 2;
    this.result = this.a1;
    this.len = 1;
    this.stack = [1];
    this.fuhao = ["+", "-"];
}
```

* a1是初始值
* a2是之后进行累累计运算的值
* result是运算的结果
* len是存储当前累计运算结果的次数
* stack是存储运算结果的栈
* fuhao是只能运算的运算符集合
#### 基本运算部分 
```

  add (a1, a2) {
    // return a1 + a2;
    this.result += this.a2; 
  }

  substraction (a1, a2) {
    // return a1 - a2;
    this.result -= this.a2; 
  }

```

* 这是加法和减法的基础函数
* result 会进行累计运算
#### 输入框调用函数 
```
  input (e) {
    let string = document.getElementById('message').value;
    let str = string.split('');
    let fuhao = str[str.length - 1];
    if (this.len > str.length + 1) {
      this.tanzhan();
      return;
    }

    if (!this.fuhao.includes(fuhao)){
      e.value = e.value.replace(/[^(\+\-)]/g,'');
      return;
    } 

    this.yunsuan(fuhao);
  }
```

* 这是input框的input方法，这个方法会获取输入内容
* 将输入的字符串拆分为一个个的运算字符，组成数组
* 判断是输入字符还是删除字符，如果是删除字符就掉用“tanzhan”函数，结束该函数
* 如果是输入，判断输入的符号是不是加减符号，如果不是那么忽略该符号，结束该函数
* 如果是调用运算函数进行运算
#### 运算处理部分 

```
 yunsuan (fuhao) {
   /*  if(fuhao == '+') {
      this.add();
      this.stack.push(this.result);
    }
    if(fuhao == '-') {
      this.substraction();
      this.stack.push(this.result);
    } */
    fuhao == "+" ? this.add() : this.substraction();
    this.stack.push(this.result);
    this.len = this.stack.length;
    document.getElementById('result').innerHTML = this.result;
  }
```

* 运算函数根据对应的运算符号，调取对应的方法运算
* 将该次运算结果存储到运算的结果栈中
* 根据当前的运算结果栈，更新当前累计运算结果的次数
* 更新展示结果
#### 回退处理部分 
```
  tanzhan () {
    this.stack.pop();
    this.result = this.stack[this.stack.length -1];
    this.len = this.stack.length;
    document.getElementById('result').innerHTML = this.result;
  }
```

* 当用户删除输入框中的运算符时，调用改函数.
* 该函数会将最近一次运算结果弹出栈中
* 将前一次运算结果赋值给运算结果
* 更新当前累计运算结果的次数
* 更新展示结果
## 改进方案
### 弊端 

* 上面的方法只能计算单次的运算
* 结果是保存在结果栈中的
* 删减运算符的结果是通过栈来实现的，会增加内存的使用
* 这种方式思路比较绕
### 改进思路

* 思路转化，就是根据运算符字符串进行累计运算，每次都进行一次运算
* 这种方式虽然增加了计算量，但是节约了内存的使用，并且以当前的计算机硬件水平，用户也不会有太大的感觉
## 改进代码
### 改进前
```
  constructor () {
    this.state = true;
    this.a1 = 1;
    this.a2 = 2;
    this.result = this.a1;
    this.len = 1;
    this.stack = [1];
    this.fuhao = ["+", "-"];
  }
  input (e) {
    let string = document.getElementById('message').value;
    let str = string.split('');
    let fuhao = str[str.length - 1];
    if (this.len > str.length + 1) {
      this.tanzhan();
      return;
    }

    if (!this.fuhao.includes(fuhao)){
      e.value = e.value.replace(/[^(\+\-)]/g,'');
      return;
    } 

    this.yunsuan(fuhao);
}

  tanzhan () {
    this.stack.pop();
    this.result = this.stack[this.stack.length -1];
    this.len = this.stack.length;
    document.getElementById('result').innerHTML = this.result;
  }

  yunsuan (fuhao) {
    fuhao == "+" ? this.add() : this.substraction();
    this.stack.push(this.result);
    this.len = this.stack.length;
    document.getElementById('result').innerHTML = this.result;
  }
```
### 改进后

```
   constructor () {
    this.a1 = 1;
    this.a2 = 2;
    this.result = this.a1;
  }
  input (e) {
    e.value = e.value.replace(/[^(\+\-)]/g,'');
    let string = document.getElementById('message').value;
    let str = string.split('');
    this.jisuan(str);
    document.getElementById('result').innerHTML = this.result;
  }

  jisuan(fuhaoArr) {
    this.result = this.a1;
    for (let i of fuhaoArr) {
      i == "+" ? this.add() : this.substraction();
    }
  }
```

* 可以看出首先代码简洁了不少
* “input”函数只需要进行输入检测规范化，然后进行累计运算，更新展示结果即可
* “jisuan”函数将result初始化，然后根据运算符字符串转化的数组进行累计运算
## 页面展示
效果如下所示


![](https://user-gold-cdn.xitu.io/2020/1/15/16fa87a5c7942617?w=489&h=831&f=png&s=15221)
## 总结


* 在之前用栈的方式的时候，比较绕，出了不少bug,都解决了，但是粘贴多个运算符时，就没办法运算了，这种模式下功能不完善了
* 基于上面的问题，研究出了后来的改进方案
* 两种方式其实各有利弊，第一种方案，执行效率高，因为运算量小，算是拿空间换时间吧，但是不支持一次粘贴多个运算符的方式
* 第二种方案每次更改都会从头开始计算，增加了运算量，但是节约了空间，算是拿时间换空间吧，同时也简化了思维，而且支持粘贴多个运算符的方式
* 如果你有更好的方式，欢迎一起来讨论

