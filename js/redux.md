# reduxについての自分的なメモ

先日reduxについていろいろ学んだので、自分なりにまとめてみました．

reduxは以下の記事が参考にしつつ頑張りました．

- [React+Redux入門](https://qiita.com/erukiti/items/e16aa13ad81d5938374e)
- [ReactとReduxを結ぶパッケージ「react-redux」についてconnectの実装パターンを試す](https://qiita.com/MegaBlackLabel/items/df868e734d199071b883)


## hello world

今回はcreate-react-appを利用して書いていきました．

```
npm install -g create-react-app
create-react-app my-app
```
以上のコマンドで`my-app`というディレクトリができると思います．
cdでそのディレクトリに移動して中身を確認します．

```
~/my-app >>  ls
README.md         node_modules      package-lock.json package.json      public            src
~/my-app >>  tree src/
src/
├── App.css
├── App.js
├── App.test.js
├── index.css
├── index.js
├── logo.svg
└── registerServiceWorker.js
```

以下のコマンドを実行することで[localhost:3000](http://localhost:3000/)にアクセスするとWelcome to Reactと表示されると思います．

```
npm install
npm start
```

## reduxアーキテクチャの導入

今のままだとただのReactなので，少しずつReact + Reduxの形にしていきます．
今回は以下のサンプルを参考にコードを書いていきます．

- [todoアプリ](https://github.com/reactjs/redux/tree/master/examples/todos)

とりあえずいらないファイルを削除します．

```
rm src/App.js
rm src/App.css
rm src/App.test.js
rm src/index.css
rm src/logo.svg
rm src/registerServiceWorker.js
```
reduxをinstallします．

```
npm install redux
npm install react-redux  
```

src/index.jsの内容を変更します．


```diff:src/index.js
import React from 'react';
- import ReactDOM from 'react-dom';
- import './index.css';
- import App from './App';
- import registerServiceWorker from './registerServiceWorker';
+ import { createStore } from 'redux';
+ import { Provider } from 'react-redux';
+ import App from './components/App';
+ import reducer from './reducers';
+ import { render } from 'react-dom';

- ReactDOM.render(<App />, document.getElementById('root'));
- registerServiceWorker();

+ const store = createStore(reducer)
+ render(
+   <Provider store={store}>
+     <App />
+   </Provider>,
+   document.getElementById('root')
+ )
```

必要なディレクトリを作成していきます．

```
mkdir reducers
mkdir containers
mkdir components
mkdir actions
```


とりあえずcomponentにApp.jsを作成します．
このApp.jsがindex.jsに呼ばれて，こいつがいろいろ呼んでいきます．

```JavaScript:components/App.js
import React, { Component } from 'react';
import AddUser from '../containers/AddUser'

class App extends Component {
    render() {
        return (
            <div className="App">
                <AddUser />
            </div>
        );
    }
}

export default App;
```

## container

とりあえずAddUserというcontainerを呼びます．
このcontainerとはcomponentを呼んだりactionを呼んだりする発火点的な役割の場所です．

```JavaScript:containers/AddUser.js
import React from 'react'
import { connect } from 'react-redux'
import { addUser } from '../actions'

let AddTodo = ({ dispatch }) => {
  let input

  return (
    <div>
      <form onSubmit={e => {
        e.preventDefault()
        if (!input.value.trim()) {
          return
        }
        dispatch(addUser(input.value))
        input.value = ''
      }}>
        <input ref={node => {
          input = node
        }} />
        <button type="submit">
          Add User
        </button>
      </form>
    </div>
  )
}
AddTodo = connect()(AddTodo)

export default AddTodo
```

これは`preventDefault()`を呼ぶことによって本来のsubmitを止めて，`dispatch`を呼んだ後inputを空にする処理を記述しています．
試しに`e.preventDefault()`を消すとページが更新されてしまいます．ページが更新されてしまうとフロント側で保持していた状態が消えてしまうのでとりあえずページ更新を行わないようにする感じです．

ここで`AddUser.js`では，`dispatch`が呼ばれています．この関数は`action`の関数を引数として呼び出します．そして`action`が`reducer`を呼んで，その後`reducer`が状態を更新するという処理が実行されます．詳しくは後述します．

## action

actionを作成します．この`action`は，`dispatch`関数で呼ばれる関数を記述する場所です．

```JavaScript:actions/index.js
let nextTodoId = 0
export const addUser = (text) => ({
    type: 'ADD_USER',
    id: nextTodoId++,
    text
})
```

ここでは`addUser`という関数を定義しており，これは`text`をもらってきて，`type, id, status, text`を入れたオブジェクトを返します．

## reducer

reducerはactionの返したオブジェクトを受け取り，action.typeによって処理をいろいろと変えられる場所です．

```JavaScript:reducers/index.js
import { combineReducers } from 'redux'
import users from './users'

const App = combineReducers({
  users
})

export default App
```

```JavaScript:reducers/users.js
const users = (state = [], action) => {
  switch (action.type) {
    case 'ADD_USER':
      return [
        ...state,
          {
              id: action.id,
              name: action.text
          }
      ]
    default:
      return state
  }
}

export default users
```

`index.js`で`combineReducers`を呼んでいることがわかると思います．これは複数の`reducer`をまとめることができる凄いやつです．
肝心のreducerは別ファイルのusers.jsに書いておき，index.jsで呼び出します．
これで複数ファイルにreducerを分けられます．(便利)

reducerでやっていることは，action.typeでswitch caseを書いて，変更した状態をreturnする感じです．今回のstateはArrayですが，Objectなども返せます．


これで一通りの登録処理が終わったので，ここから追加したuserを表示するcomponentを作成していきます．

## UserListの作成

追加されたUserを表示するためのcontainerとcomponentを作成していきます．
まずcontainerから．

```JavaScript:UserList.js
import { connect } from 'react-redux'
import UserTable from '../components/UserTable'

const mapStateToProps = (state) => ({
  users: state.users
})

const UserList = connect(
  mapStateToProps
)(UserTable)

export default UserList
```

connectというものが呼ばれているのがわかります．これはstateやらactionやらを渡してcomponentを作成できる便利なやつです．
渡されたstateやcomponentはcomponentがそのまま使用できます．

次にcomponentです．

```JavaScript:UserTables.js
import React from 'react';

const UserTables = (users) => (
    <table>
        {users.users.map(user =>
            <tr><td>{user.id}</td><td>{user.name}</td></tr>
        )}
    </table>
);

export default UserTables;
```

