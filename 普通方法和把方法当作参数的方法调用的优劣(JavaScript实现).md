# 普通方法和把方法当作参数的方法调用的优劣

> 当我们编写代码时，方法调用（函数调用）是我们经常使用的一种基本技术。它允许我们将一段逻辑封装为可重用的代码块，并在需要的时候进行调用。然而，就像任何编程技术一样，方法调用也有一些缺点和考虑事项。
>
> 在本文中，我们将探讨一般方法调用的一些缺点，这些缺点可能会对代码的性能、可读性和维护性产生影响。了解这些缺点将帮助我们更好地理解何时使用方法调用，并在适当的时候考虑使用其他技术。

* 额外的开销：方法调用涉及函数栈的创建和销毁，以及参数的传递和返回值的处理。这些操作会导致额外的开销，尤其是在频繁调用的情况下，可能会对性能产生一定的影响。

* 上下文切换：方法调用需要在调用者和被调用者之间进行上下文切换。这涉及到保存和恢复现场，包括寄存器、栈指针等，这些操作会增加执行时间。

* 栈溢出：方法调用使用函数栈来存储局部变量和返回地址等信息。如果递归调用层级过深或者方法内部使用了大量的局部变量，可能会导致栈溢出的问题。

* 可读性下降：过多的方法调用可能会导致代码的嵌套层级过深，降低代码的可读性和可维护性。特别是在长链式的方法调用中，代码的含义可能不够清晰，增加了理解和调试的难度。

* 依赖性和耦合性：方法调用在一定程度上引入了依赖性和耦合性。调用者需要知道被调用方法的名称和参数，这使得调用者与被调用者之间存在一定的依赖关系。如果被调用方法发生变化，调用者的代码可能需要相应调整。

> 需要注意的是，这些缺点并不是绝对的，它们的影响取决于具体的应用和场景。在大多数情况下，方法调用仍然是一种重要的编程机制，具有诸多优势和灵活性。开发人员需要在设计和使用方法调用时权衡这些缺点，并结合具体情况进行合理的选择和优化。

## JavaScript中用lambda表达式

> 下面介绍三种在JavaScript中用lambda表达式来把函数作为参数进行传递的方法

### 1.求自然数n的和

```js
function sum(n) {
    let sum = 0;
    for (let i = 1; i <= n; i++) {
        sum += i;
    }
    return sum;
}
```

### 2.求n的阶层

```js
function sum(n) {
    let sum = 1;
    for (let i = 1; i <= n; i++) {
        sum = sum * i;
    }
    return sum;
}
```

### 3.求自然数的平方和

```js
function sum(n) {
    let sum = 0;
    for (let i = 1; i <= n; i++) {
        sum += i*i;
    }
    return sum;
}
```

## 使用lambda表达式来实现

### 1.

```js
function sum(n, f) {
    let sum = 0;
    for (let i = 1; i <= n; i++) {
        sum += f(i);
    }
    return sum;
}
```



![251913092-64c1798c-d9f6-4cc0-baf1-225f4bd25d91](https://raw.githubusercontent.com/DecZeroTwo/blogimage/main/images/202310091710120.png)



### 2.

```js
function sum(n, f) {
    let sum = 1;
    for (let i = 1; i <= n; i++) {
      sum = f(sum, i);
    }
    return sum;
}
```



![251913156-6658687c-699d-404b-9e05-8390a94f30ff](https://raw.githubusercontent.com/DecZeroTwo/blogimage/main/images/202310091711627.png)



### 3.

```js
function sum(n, f) {
    let sum = 0;
    for (let i = 1; i <= n; i++) {
        sum += f(i);
    }
    return sum;
}
```



![251913197-30d9c7dd-c45b-4e9e-bac0-7c1f726e155c](https://raw.githubusercontent.com/DecZeroTwo/blogimage/main/images/202310091711934.png)



### 还可以在函数中套函数，更灵活的实现多种运算

```js
function sum(n,f,comb){
    let sum = 0;
    for (let i = 1; i <= n ; i++) {
      sum = comb(sum,f(i));
    }
    return sum;
  }
```


![251913223-e4cffa73-9f8e-4a74-bf2d-7f466a4211b1](https://raw.githubusercontent.com/DecZeroTwo/blogimage/main/images/202310091712761.png)



### 在Java中我们也可以通过接口来实现

```java
public class Lambda {
    public Integer sumOfAnything(int n, Op1 op1, Op2 op2) {
        Integer sum = 0;
        for (int i = 1; i <= n; i++) {
            sum = op2.operator(sum,op1.operator(i));
        }
        return sum;
    }

    public Integer anyOfAnything(int n, Op1 op1, Op2 op2,int identity) {
        Integer sum = identity;
        for (int i = 1; i <= n; i++) {
            sum = op2.operator(sum,op1.operator(i));
        }
        return sum;
    }

    public static void main(String[] args) {
        Lambda lambda = new Lambda();
        System.out.println("结果1为" + lambda.sumOfAnything(5,(x)->{
            return x*x*x;
            },(x,y)->{
            return x+y;
        }));
        System.out.println("结果2为" + lambda.anyOfAnything(3,(x -> {
            return x*x*x;
            }),(x,y)->{
            return x*y;
        },1));
    }

}

interface Op1 {
    public Integer operator(int x);
}

interface Op2{
    public Integer operator(int x,int y);
}
```



![251209719-4a93c41b-459f-4eb1-94eb-543d137d51ad](https://raw.githubusercontent.com/DecZeroTwo/blogimage/main/images/202310091712292.png)



相当于重写了接口的方法，比如传进去一个x返回的是x的立方

## 总结

> 把函数当作参数具有以下优点。
>
> 1、灵活性：将方法作为参数传递允许在运行时动态决定要执行的具体代码。这使得程序具有更大的灵活性，可以根据不同的需求和条件来选择不同的行为。
>
> 2、可重用性：通过将方法作为参数传递，可以实现代码的重用性。可以编写通用的方法，接受特定的方法作为参数，并在多个地方使用它们，而不必重复编写相似的代码。
>
> 3、代码简洁性：通过传递方法作为参数，可以避免编写大量的条件语句或重复的代码块。这使得代码更加简洁和易读。
>
> 4、扩展性：将方法作为参数传递，使得程序更容易扩展和修改。如果需要改变功能或添加新的操作，只需要提供新的方法并将其传递给调用者即可，而不需要修改原有的代码。
