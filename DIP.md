# 依赖倒置原则

## 官方定义

* 依赖倒置原则，英文缩写DIP，全称Dependence Inversion Principle。

* 原始定义：High level modules should not depend upon low level modules. Both should depend upon abstractions. Abstractions should not depend upon details. Details should depend upon abstractions。

* 官方翻译：高层模块不应该依赖低层模块，两者都应该依赖其抽象；抽象不应该依赖细节，细节应该依赖抽象。

---
## 简介

* 依赖倒置原则的最重要问题就是确保应用程序或框架的主要组件从非重要的底层组件实现细节解耦出来，这将确保程序的最重要的部分不会因为低层次组件的变化修改而受影响。

* 该原则的第一部分是关于高层模块和低层模块之间的耦合方式，在传统的分成架构中，高层模块（封装了程序的核心业务逻辑）总依赖于低层的一些模块（一些基础点）。当应用依赖倒置原则的时候，关系就反过来了。和高层模块依赖于低层模块不同，依赖倒置是让低层模块依赖于高层模块里定义的接口。举例来说，如果要给程序进行数据持久化，传统的设计是核心模块依赖于一个持久化模块的API，而根据依赖倒置原则重构以后，则是核心模块需要定义持久化的API接口，然后持久化的实现实例需要实现核心模块定义的这个API接口。

* 该原则的第二部分描述的是抽象和细节之间的正确关系。理解这一部分，通过了解C++语言比较有帮助，因为他的适用性比较明显。

* 不像一些静态类型的语言，C++没有提供一个语言级别的概念来定义接口，那类定义和类实现之间到底是怎么样的呢，在C++里，类通过头文件的形式来定义，其中定义了源文件需要实现的类成员方法和变量。因为所有的变量和私有方法都定义在头文件里，所以可以用来抽象以便和实现细节之前解耦出来。通过定只定义抽象方法来实现（C++里是抽象基类）接口这个概念用于实现类来实现。


---
## 图解
![](/assets/DIP.png)

---
## 代码

```javascript
# 反例
class PasswordReminder {
    private $dbConnection;

    public function __construct(MySQLConnection $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}
```


```javascript

interface DBConnectionInterface {
    public function connect();
}  
  
class MySQLConnection implements DBConnectionInterface {
    public function connect() {
        return "Database connection";
    }
}

class PasswordReminder {
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}  
```



---
## 参考资料

[S.O.L.I.D: The First 5 Principles of Object Oriented Design](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)

[小话设计模式原则之：依赖倒置原则DIP](https://zhuanlan.zhihu.com/p/24175489)

[S.O.L.I.D五大原则之依赖倒置原则DIP](https://www.kancloud.cn/kancloud/deep-understand-javascript/43700)

