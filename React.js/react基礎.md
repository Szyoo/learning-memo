## RECAT

### 基礎

```js
// Reactをインポートしてください
import React from 'react':

// Appクラスを定義してください
class App extends React.Component{
    render(){
        return(
            // jsx部分
        )
    }
}

// Appクラスをエクスポートしてください
export default App;
```


| 区別     | JSX                          | JavaScript                 |
| -------- | ---------------------------- | -------------------------- |
| 範囲     | 只能在render内的return内使用 | 可在render内的return外使用 |
| コメント | {/\* \*/}                    | //                         |

当JS要在return内使用，需用中括弧`{}`包裹，比如如下例子：

```java
render(){
    const text = '123';
    return(
        <p>{text}</p>a
    )
}
```

イベント、state

イベント名={() => { 処理 }}

ユーザーの動きに合わせて変わる値のことを、Reactでは**state**と呼びます。

stateは、**constructorの中**で、**オブジェクト**として定義します。
ここで定義したオブジェクトがstateの初期値となります。

```js
class App extends React.Component{

  constructor(props){
    super(props);
    this.state = {name: 'A'};
  }

  render(){
    return(
      {this.state.name}
    )
  }
}
```
