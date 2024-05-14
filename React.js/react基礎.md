## RECAT

### kiso

JSX：只能在render内的return内使用

JavaScript：可在render内的return外使用

JSXを**{/\* \*/}**で囲むと、その部分はコメントになります。

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

当JS要在return内使用，需用中括弧包裹，比如如下例子：

```java
class App extends React.Component{
    render(){
        const text = '123';
        return(
            <p>{text}</p>a
        )
    }
}
```
