# [超入門] React Hooks? 何それ？ おいしいの？
## - とりまコレだけは覚えて -

### はじめに
React HooksとはReact16.8から追加された新機能です。
2019.2.4にリリースされた比較的、新しい技術となります。

ここでは最低限覚えておけば使えるレベルに公式ドキュメントを要約して記事を書きました。
少しでもとっかかりになってくれればという想いで書いています。
もっと詳しく知りたい人は公式ドキュメントを参照ください。

cf. [フックの導入](https://ja.reactjs.org/docs/hooks-intro.html)

### React Hooks導入以前
- ステートやライフサイクルはクラスコンポーネントでしか書くことができなかった。

- 最初はシンプルだったコンポーネントがstateやロジックを使うことで複雑になりがちになっていた。

- 一緒に更新されなければならないコードがバラバラにされたり、逆に無関係コードが1つのメソッド内に書かれることが多く簡単にバグが起こるようになっていた。

### React Hooksを使うことで
- フックを使えば、ステートを持ったロジックを、コンポーネントの階層構造を変えることなしに再利用できるようになった。

### ではまず全文を書いて両者を比較してます
- クラスコンポーネントで書くと...

```jsx
// App.js
import React, { Component } from 'react';

class App extends Component {
    // state
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        }
    }

    // ライフサイクルメソッド
    componentDidMount() {
        console.log(this.state.count);
    }
    componentDidUpdate() {
        console.log(this.state.count);
    }

    render() {
        return (
            <div className="App">
            { this.state.count }
            <button onClick={() => this.setState({ count: this.state.count + 1 })}>
                Click
            </button>
            </div>
        );
    }
}

export default App;
```

- ファンクションコンポーネントで書くと...

```jsx
// App.js
// useState, useEffectを追加
import React, { useState, useEffect } from 'react';

const App = () => {
    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log(`${count}回カウントされています`);
    });

    return (
        <div className="App">
            { count }回カウントされています
            <button onClick={ () => setCount(count + 1) }>
            Click
            </button>
        </div>
    )
}

export default App;
```
もともとReactをやっていた人ならお分かりいただけたではないでしょうか。
このようにHooksを利用するとかなりコードを圧縮してスッキリ書くことが可能。

Happy!Happy!


### フックの種類

- そもそもフック(Hook)って何なの？

フックとはReactの機能に接続（hook into）するための特別な関数のこと。
ステートフックと副作用フックがあります。

- ステートフック
useStateによってstateの機能を関数コンポーネントに追加できます。

```jsx
// useStateを追加
import React, { useState　} from 'react';

const App = () => {
    // 初期値(count)に引数の0が代入される
    const [count, setCount] = useState(0);

    // 1つのコンポーネント内で2個以上使用可能 かつ 配列を代入することも可能
    const [todos, setTodos] = useState([
        { text: 'Learn Hooks' }
    ]);
    
    // setCountでstateが更新されていく
    <button onClick={ () => setCount(count + 1) }>
}
```

cf.[配列の分割代入構文](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)を利用
(注)配列の分割代入構文はReact Hooks APIの機能ではないので注意してください

```jsx
const [todo, setTodo] = useState('');
// ↓
// こちらと等価
const todoStateVariable = useState('');
const todo = todoStateVariable[0]; // itemの1番目格納
const setTodo = todoStateVariable[1]; // itemの2番目格納
```

- 副作用フック

副作用フックとは外部データの取得や購読、手動でのDOM更新などの操作のこと。
クラスコンポーネントだとcomponentDidMountやcomponentDidUpdateに相当します。

```jsx
    // クラスコンポーネントにおけるライフサイクル
    componentDidMount() {
        console.log(this.state.count);
    }
    componentDidUpdate() {
        console.log(this.state.count);
    }
```

```jsx
    // useEffectを追加
    import React, { useState, useEffect } from 'react';

    // ファンクションコンンポーネントにおけるuseEffect
    // こちらもuseState同様1つのコンポーネント内で2個以上使用可能
    useEffect(() => {
        console.log(`${count}回カウントされています`);
    });
```

以上を見てわかるようにクラスコンポーネントではレンダー前と後で同じコードを2回書かなければならないのに
対して、useEffectを使うと1度で完結しています。

このようにuseEffectはcomponentDidMountやcomponentDidUpdate
およびcomponentWillUnmountなどのメソッドがひとまりになったものだと
考えておくと分かりやすいかと思います。
厳密には違いがありますがここでは触れません。


### 結論 React Hooksでできること
- ファンクションコンポーネントでもstate、ライフサイクル、状態管理を書くことができる。

- それによりコードをスッキリさせ再利用可能なコンポーネントにすることができる。

てな訳で新しく書く場合はファンクションコンポーネントでReact Hooksで書こう！！

# 脱 クラスコンポーネント！！

### 他にも...
useContextやReact ReduxのフックであるuseDispatchなど便利な機能もあるので
ぜひ、ググって使って見てください。
　
Good React Life for you!