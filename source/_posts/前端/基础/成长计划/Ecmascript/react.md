---
title: react
toc: true
date: 2018-03-28  08:35:28
tags: [前端, react, js 基础]
---

# React 组件的生命周期、通信方式

## 组件的生命周期

1.创建阶段

该阶段主要发生在创建组件类的时候，即调用 React.createClass 时触发。
这个阶段只会触发一个 getDefaultProps 方法，该方法返回一个对象并缓存起来。然后与父组件指定的 props 对象合并，最后赋值给 this.props 作为该组件的默认属性。
props属性介绍：
1，props 是一个对象，是组件用来接收外面传来的参数的。
2，组件内部是不允许修改自己的 props 属性，只能通过父组件来修改。上面的 getDefaultProps 方法便是处理 props 的默认值的。

2.实例化阶段

该阶段主要发生在实例化组件类的时候，也就是该组件类被调用的时候触发。这个阶段会触发一系列的流程，按执行顺序如下：
（1）getInitialState：初始化组件的 state 的值。其返回值会赋值给组件的 this.state 属性。
（2）componentWillMount：根据业务逻辑来对 state 进行相应的操作。
（3）render：根据 state 的值，生成页面需要的虚拟 DOM 结构，并返回该结构。
（4）componentDidMount：对根据虚拟 DOM 结构而生的真实 DOM 进行相应的处理。组件内部可以通过 ReactDOM.findDOMNode(this) 来获取当前组件的节点，然后就可以像 Web 开发中那样操作里面的 DOM 元素了。
state属性介绍：
它是用来存储组件自身需要的数据。它是可以改变的，它的每次改变都会引发组件的更新。这也是 ReactJS 中的关键点之一。
即每次数据的更新都是通过修改 state 属性的值，然后 ReactJS 内部会监听 state 属性的变化，一旦发生变化，就会触发组件的 render 方法来更新 DOM 结构。

3.更新阶段

这主要发生在用户操作之后或者父组件有更新的时候，此时会根据用户的操作行为进行相应得页面结构的调整。这个阶段也会触发一系列的流程，按执行顺序如下：
（1）componentWillReceiveProps：当组件接收到新的 props 时，会触发该函数。在改函数中，通常可以调用 this.setState 方法来完成对 state 的修改。
（2）shouldComponentUpdate：该方法用来拦截新的 props 或 state，然后根据事先设定好的判断逻辑，做出最后要不要更新组件的决定。
（3）componentWillUpdate：当上面的方法拦截返回 true 的时候，就可以在该方法中做一些更新之前的操作。
（4）render：根据一系列的 diff 算法，生成需要更新的虚拟 DOM 数据。（注意：在 render 中最好只做数据和模板的组合，不应进行 state 等逻辑的修改，这样组件结构更加清晰）
（5）componentDidUpdate：该方法在组件的更新已经同步到 DOM 中去后触发，我们常在该方法中做一 DOM 操作。

4.销毁阶段
这个阶段只会触发一个叫 componentWillUnmount 的方法。
当组件需要从 DOM 中移除的时候，我们通常会做一些取消事件绑定、移除虚拟 DOM 中对应的组件数据结构、销毁一些无效的定时器等工作。这些事情都可以在这个方法中处理。

## 父子组件间通信

这种情况下很简单，就是通过 props 属性传递

## 非父子组件间的通信

使用全局事件 Pub/Sub 模式，在 componentDidMount 里面订阅事件，在 componentWillUnmount 里面取消订阅，当收到事件触发的时候调用 setState 更新 UI。

直接怼redux 或 Mobx

# 手写实现 React 高阶组件

>高阶函数是至少满足下列一个条件的函数：

>接受一个或多个函数作为输入
输出一个函数

>高阶组件就是接受一个组件作为参数并返回一个新组件的函数。这里需要注意高阶组件是一个函数，并不是组件，这一点一定要注意。


## 基于属性代理的方式


属性代理是最常见的高阶组件的使用方式，上述描述的高阶组件就是这种方式。它通过做一些操作，将被包裹组件的props和新生成的props一起传递给此组件，这称之为属性代理。

```

export default function withHeader(WrappedComponent) {
  return class HOC extends Component {
    render() {
      const newProps = {
        test:'hoc'
      }
      // 透传props，并且传递新的newProps
      return <div>
        <WrappedComponent {...this.props} {...newProps}/>
      </div>
    }
  }
}

```

## 基于反向继承的方式

这种方式返回的React组件继承了被传入的组件，所以它能够访问到的区域、权限更多，相比属性代理方式，它更像打入组织内部，对其进行修改。

```

export default function (WrappedComponent) {
  return class Inheritance extends WrappedComponent {
    componentDidMount() {
      // 可以方便地得到state，做一些更深入的修改。
      console.log(this.state);
    }
    render() {
      return super.render();
    }
  }
}

```

## 组合 Compose

compose可以帮助我们组合任意个（包括0个）高阶函数，例如compose(a,b,c)返回一个新的函数d，函数d依然接受一个函数作为入参，只不过在内部会依次调用c,b,a，从表现层对使用者保持透明。
基于这个特性，我们便可以非常便捷地为某个组件增强或减弱其特征，只需要去变更compose函数里的参数个数便可。
compose函数实现方式有很多种，这里推荐其中一个recompact.compose，详情见下方参考类库。

```
const enhance = compose(withHeader,withLoading);
@enhance
class Demo extends Component{

}

```



# 手写实现一个 Redux 中的 reducer (state, action) => newState

Reducer：Store收到Action以后，必须给出一个新的State，这样View才会发生变化。这种State的计算过程就叫做Reducer。Reducer是一个函数，它接受Action和当前State作为参数，返回一个新的State。

```

let reducer = function(state, action) {
  if(!state) {
    return {counter: 0};
  }
  switch(action.type) {
    case 'ADD':
      return {counter: state.counter+1};
    case 'DEL':
      return {counter: state.counter-1};
    default:
      return state;
  }
};

```


# React 受控组件和非受控组件的区别

受控组件与其它React组件行为一样，其所有状态属性的更改都由React 来控制，也就是说它根据组件的props和state来改变组件的UI表现形式.

非受控组件即组件的状态改变不受控制.
非受控组件一般没什么用途，其值并非受父组件控制，它的值受其自身控制。但是，我们可以对其添加一个ref属性，这样可以获得对非受控组件渲染后底层DOM元素的访问。

```

一个受控组件有如下过程：

通过getInitialState设置defaultValue
<input>渲染时设置默认值
onChange事件触发后，调用相关处理器
change事件处理更新state
重新渲染<input>

```

## 假设有一个列表的数据，React 如何更快的加载数据？（优化方法）

在 React.js 处理列表就是用 map 来处理、渲染的。

React.js 的是非常高效的，它高效依赖于所谓的 Virtual-DOM 策略。简单来说，能复用的话 React.js 就会尽量复用，没有必要的话绝对不碰 DOM。对于列表元素来说也是这样，但是处理列表元素的复用性会有一个问题：元素可能会在一个列表中改变位置。

React.js 就简单的通过 key 来判断出来，这两个列表元素只是交换了位置，可以尽量复用元素内部的结构。

对于用表达式套数组罗列到页面上的元素，都要为每个元素加上 key 属性，这个 key 必须是每个元素唯一的标识。一般来说，key 的值可以直接后台数据返回的 id，因为后台的 id 都是唯一的。

# React的this.setState设计的原因，为什么不让用户自己来


setState不会立刻改变React组件中state的值；
setState通过引发一次组件的更新过程来引发重新绘制；
多次setState函数调用产生的效果会合并。

当setState被调用时，能驱动组件的更新过程，引发componentDidUpdate、render等一系列函数的调用。

当shouldComponentUpdate函数被调用的时候，this.state没有得到更新。
当componentWillUpdate函数被调用的时候，this.state依然没有得到更新。

直到render函数被调用的时候，this.state才得到更新。

(或者，当shouldComponentUpdate函数返回false，这时候更新过程就被中断了，render函数也不会被调用了，这时候React不会放弃掉对this.state的更新的，所以虽然不调用render，依然会更新this.state。）

如果你没兴趣去记住React的生命周期（虽然你应该记住），那就可以简单认为，直到下一次render函数调用时（或者下一次shouldComponentUpdate返回false时）才得到更新的this.state。

如果传递给this.setState的参数不是一个对象而是一个函数，那游戏规则就变了。


这个函数会接收到两个参数，第一个是当前的state值，第二个是当前的props，这个函数应该返回一个对象，这个对象代表想要对this.state的更改，换句话说，之前你想给this.setState传递什么对象参数，在这种函数里就返回什么对象，不过，计算这个对象的方法有些改变，不再依赖于this.state，而是依赖于输入参数state。


