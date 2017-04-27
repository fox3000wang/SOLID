# 接口隔离原则

## 官方定义

* 接口隔离原则，英文缩写ISP，全称Interface Segregation Principle。
* 原始定义：Clients should not be forced to depend upon interfaces that they don't use，还有一种定义是The dependency of one class to another one should depend on the smallest possible interface。
* 官方翻译：其一是不应该强行要求客户端依赖于它们不用的接口；其二是类之间的依赖应该建立在最小的接口上面。简单点说，客户端需要什么功能，就提供什么接口，对于客户端不需要的接口不应该强行要求其依赖；类之间的依赖应该建立在最小的接口上面，这里最小的粒度取决于单一职责原则的划分。

---

## 简介

### 一、ISP简介（ISP--Interface Segregation Principle）：

* 使用多个专门的接口比使用单一的总接口要好。
* 一个类对另外一个类的依赖性应当是建立在最小的接口上的。
* 一个接口代表一个角色，不应当将不同的角色都交给一个接口。没有关系的接口合并在一起，形成一个臃肿的大接口，这是对角色和接口的污染。
* “不应该强迫客户依赖于它们不用的方法。接口属于客户，不属于它所在的类层次结构。”这个说得很明白了，再通俗点说，不要强迫客户使用它们不用的方法，如果强迫用户使用它们不使用的方法，那么这些客户就会面临由于这些不使用的方法的改变所带来的改变。

### 二、举例说明：

* 使用场合,提供调用者需要的方法,屏蔽不需要的方法.满足接口隔离原则.比如说电子商务的系统,有订单这个类,有三个地方会使用到,
* 一个是门户,只能有查询方法,
* 一个是外部系统,有添加订单的方法,
* 一个是管理后台,添加删除修改查询都要用到.
* 根据接口隔离原则\(ISP\),一个类对另外一个类的依赖性应当是建立在最小的接口上.
* 也就是说,对于门户,它只能依赖有一个查询方法的接口.

## 图解

[![](https://github.com/fox3000wang/fcc_study/raw/beta/DesignPattern/SOLID-ISP-00.jpg "ISP")](https://github.com/fox3000wang/fcc_study/blob/beta/DesignPattern/SOLID-ISP-00.jpg)

---

## 代码

```javascript
# 反例：图形接口
# 缺点：有一些平面的图形，没有体积，但是又要实现volume这个接口
interface ShapeInterface {
    public function area();
    public function volume();
}   

```

```javascript
# 修改
interface ShapeInterface {
    public function area();
}

interface SolidShapeInterface {
    public function volume();
}

class Cuboid implements ShapeInterface, SolidShapeInterface {
    public function area() {
        // calculate the surface area of the cuboid
    }

    public function volume() {
        // calculate the volume of the cuboid
    }
}

```

```javascript
# 优化
interface ManageShapeInterface {
    public function calculate();
}

class Square implements ShapeInterface, ManageShapeInterface {
    public function area() { /*Do stuff here*/ }

    public function calculate() {
        return $this-
>
area();
    }
}

class Cuboid implements ShapeInterface, SolidShapeInterface, ManageShapeInterface {
    public function area() { /*Do stuff here*/ }
    public function volume() { /*Do stuff here*/ }

    public function calculate() {
        return $this-
>
area() + $this-
>
volume();
    }
}   

```

---

## javaScript代码

* JavaScript下我们改如何遵守这个原则呢？毕竟JavaScript没有接口的特性，如果接口就是我们所想的通过某种语言提供的抽象类型来建立contract和解耦的话，那可以说还行，不过JavaScript有另外一种形式的接口。在Design Patterns – Elements of Reusable Object-Oriented Software一书中我们找到了接口的定义：

> 一个对象声明的任意一个操作都包含一个操作名称，参数对象和操作的返回值。我们称之为操作符的签名（signature）。

> 一个对象里声明的所有的操作被称为这个对象的接口（interface）。一个对象的接口描绘了所有发生在这个对象上的请求信息。

* 不管一种语言是否提供一个单独的构造来表示接口，所有的对象都有一个由该对象所有属性和方法组成的隐式接口。参考如下代码：

``````javascript

var exampleBinder = {};
exampleBinder.modelObserver = (function() {
    /* 私有变量 */
    return {
        observe: function(model) {
            /* 代码 */
            return newModel;
        },
        onChange: function(callback) {
            /* 代码 */
        }
    }
})();

exampleBinder.viewAdaptor = (function() {
    /* 私有变量 */
    return {
        bind: function(model) {
            /* 代码 */
        }
    }
})();

exampleBinder.bind = function(model) {
    /* 私有变量 */
    exampleBinder.modelObserver.onChange(/* 回调callback */);
    var om = exampleBinder.modelObserver.observe(model);
    exampleBinder.viewAdaptor.bind(om);
    return om;
};

```

* 上面的exampleBinder类库实现的功能是双向绑定。该类库暴露的公共接口是bind方法，其中bind里用到的关于change通知和view交互的功能分别是由单独的对象modelObserver和viewAdaptor来实现的，这些对象从某种意义上来说就是公共接口bind方法的具体实现。

* 尽管JavaScript没有提供接口类型来支持对象的contract，但该对象的隐式接口依然能当做一个contract提供给程序用户。

---

## ISP与JavaScript

* 我们下面讨论的一些小节是JavaScript里关于违反接口隔离原则的影响。正如上面看到的，JavaScript程序里实现接口隔离原则虽然可惜，但是不像静态类型语言那样强大，JavaScript的语言特性有时候会使得所谓的接口搞得有点不粘性。

### 堕落的实现

* 在静态类型语言语言里，导致违反ISP原则的一个原因是堕落的实现。在Java和C\#里所有的接口里定义的方法都必须实现，如果你只需要其中几个方法，那其他的方法也必须实现（可以通过空实现或者抛异常的方式）。在JavaScript里，如果只需要一个对象里的某一些接口的话，他也解决不了堕落实现这个问题，虽然不用强制实现上面的接口。但是这种实现依然违反了里氏替换原则。

```
var rectangle = {
    area: function() {
        /* 代码 */
    },
    draw: function() {
        /* 代码 */
    }
};

var geometryApplication = {
    getLargestRectangle: function(rectangles) {
        /* 代码 */
    }
};

var drawingApplication = {
    drawRectangles: function(rectangles) {
       /* 代码 */
    }
};

```

* 当一个rectangle替代品为了满足新对象geometryApplication的getLargestRectangle 的时候，它仅仅需要rectangle的area\(\)方法，但它却违反了LSP（因为他根本用不到其中drawRectangles方法才能用到的draw方法）。

---

## 静态耦合

* 静态类型语言里的另外一个导致违反ISP的原因是静态耦合，在静态类型语言里，接口在一个松耦合设计程序里扮演了重大角色。不管是在动态语言还是在静态语言，有时候一个对象都可能需要在多个客户端用户进行通信（比如共享状态），对静态类型语言，最好的解决方案是使用Role Interfaces，它允许用户和该对象进行交互（而该对象可能需要在多个角色）作为它的实现来对用户和无关的行为进行解耦。在JavaScript里就没有这种问题了，因为对象都被动态语言所特有的优点进行解耦了。

## 语义耦合

* 导致违反ISP的一个通用原因，动态语言和静态类型语言都有，那就是语义耦合，所谓语义耦合就是互相依赖，也就是一个对象的行为依赖于另外一个对象，那就意味着，如果一个用户改变了其中一个行为，很有可能会影响另外一个使用用户。这也违反单一职责原则了。可以通过继承和对象替代来解决这个问题。

## 可扩展性

* 另外一个导致问题的原因是关于可扩展性，很多人在举例的时候都会举关于callback的例子用来展示可扩展性（比如ajax里成功以后的回调设置）。如果想这样的接口需要一个实现并且这个实现的对象里有很多熟悉或方法的话，ISP就会变得很重要了，也就是说当一个接口interface变成了一个需求实现很多方法的时候，他的实现将会变得异常复杂，而且有可能导致这些接口承担一个没有粘性的职责，这就是我们经常提到的胖接口。

---

## 参考资料

[S.O.L.I.D五大原则之接口隔离原则ISP](https://www.kancloud.cn/kancloud/deep-understand-javascript/43699)

[S.O.L.I.D: The First 5 Principles of Object Oriented Design](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)

