# 1.关于变量赋值

对于传统C，变量赋值直接=一下就得，允许重赋值和初始化赋值，此时等于用的是`==`：

```c
int a = 3;
a=5;
```

在有的语言当中，为了将等于和赋值区分开来，赋值采用了`:=`，而等于可以用`=`。

在Haskell中，由于要严格确保副作用，所以不允许变量后期赋值，只允许初始化赋值，也就是let bindings:

```haskell
let a = 3 in print a
```
此时`a`被赋值之后不允许再修改。

而我个人的看法是，保留Haskell中的let bindings用于处理无副作用变量（类似于constexpr），而有副作用变量，应该人为声明它是`mutable`的，然后对其进行修改可以用C方式，可以使用`:=`表达式。（同时，AST应该支持标记出所有`mutable`的变量被修改的情况，从而可以通过特殊的编译指令，将`mutable`的变量被修改时出现标记）

# 2.关于多态

多态的话我个人觉得Haskell中的typeclass是比较优美的。从实践上看，typeclass的写法比较类似于concept。

## I.类型

```haskell
data MyType = Type11 Type12 ... | Type21 Typ22 ... | ...
```

这是Haskell中的类型定义方式，用`|`分割可能取值情况。

## II.类型类与实例类型

```haskell
class BasicEq a where
    isEqual :: a -> a -> Bool
```

上面这个例子，我们定义了一个类型类，在OOP当中可以看成是一个父类，但是有区别的是，这个父类是不能使用的（其实更严谨的说这里是类型的类型）。

当我们要使用类型的时候，可以使用实例类型`instance`

```haskell
instance BasicEq Bool where
    isEqual True  True  = True
    isEqual False False = True
    isEqual _     _     = False
```

上面这个例子里面就定义了一个实例类型，我们将Bool作为一个实例类型，给出isEqual这个函数的实现，此时Bool就成为了一个`BasicEq`类型，之后如这个样子的类型限定就包含Bool了：`BasicEq a => a -> a -> ...`（此时所有的a都是BasicEq类型类的实例类型）


## III.自动派生

```haskell
data Color = Red | Green | Blue
    deriving (Read, Show, Eq, Ord)
```

上面这个实例中，ghc会自动把`Color`类型派生成`Read`等类型，此时所有这些类型可以允许的操作都可以在`Color`上使用（当然，要类型比较简单的时候，如果复杂了还是应该用前述的实例类型）。

# 3.关于指针操作

类似C++泛型的方式？传统C方式？Monad？

# 4.关于泛型与自动类型推断

自动类型推断应该达到一个怎么样的精准度？

# 5.lambda演算

# 6.尾递归优化

# 7.条件控制（case expressions和guards）

# 8.反射

# 9.严格求值与惰性求值

# 10.Functor&Monad
