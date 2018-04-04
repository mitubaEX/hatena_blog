# React + TypeScript + MobXで簡易Counterアプリを作成してみた

- 参考にしました

[sample-react-mobx](https://github.com/togana/sample-react-mobx)


## プロジェクト作成

今回はTypeScriptを利用するので，optionをつけてアプリを作成します

```
create-react-app --scripts-version=react-scripts-ts [app_name]
```

## いじいじ

まずApp.tsxを以下のように変更します．

```App.tsx
import React, { Component } from 'react';
import CountContainer from './containers/CountContainer';

class App extends Component {
  render() {
    return (
      <CountContainer />
    );
  }
}

export default App;
```

今回，stateをもらってくる役目をCountContainerにしてもらいました．
(reduxっぽい感じにしたかった)

### 必要なディレクトリを作成

```
mkdir containers
mkdir components
mkdir stores
```

storeはデータの置き場みたいなもので，このstoreのメソッドを呼び出すことによってデータを変更していきます．


### CountContainer

```./containers/CountContainer.tsx
import React, { Component } from 'react';
import { Provider } from 'mobx-react';
import CountStore from '../store/CountStore';
import Counter from '../components/Counter';

const stores = new CountStore();

export default class CountContainer extends Component {
    render() {
        return (
            <Provider count={stores}>
                <Counter />
            </Provider>
        );
    }
}
```

### Counter

```./components/Counter.tsx
import React, { Component } from 'react';
import { inject, observer } from 'mobx-react';
import { CountStoreType } from '../store/CountStore';

type Props = {
    count?: CountStoreType
};

@inject('count')
@observer
class Counter extends Component<Props> {
    render() {
        const { count } = this.props;

        return (
            <div>
                Counter : {count!.num} <br />
                < button onClick={count!.onIncrement} > + </button>
                < button onClick={count!.onDecrement} > - </button>
                < br /> GetDoubleCount: {count!.getDoubleCount}
            </div>
        );
    }
}

export default Counter;
```

### CountStore

```./store/CountStore.ts
import { observable, action, computed } from 'mobx';

export type CountStoreType = {
  num: number
  getDoubleCount: () => void
  onIncrement: () => void
  onDecrement: () => void
};

export default class CountStore {
  @observable num = 0;

  @computed get getDoubleCount() {
    return this.num * 2;
  }

  @action.bound onIncrement() {
    this.num = this.num + 1;
  }

  @action.bound onDecrement() {
    this.num = this.num - 1;
  }
}
```

## 詰まった点

### import fails with 'no default export'

- [import fails with 'no default export'](https://github.com/Microsoft/TypeScript-React-Starter/issues/8)

`tsconfig.json`の`"compilerOptions"`の中に以下を記述します．

```
"allowSyntheticDefaultImports": true
```

### Providerで値を渡す

- [React x MobXな趣味プロダクトをTypeScriptでリライトしようとしてたメモ](https://lealog.hateblo.jp/entry/2018/01/09/155747)

このブログに書かれているように`Provider`からもらってくる値には`?`を付ける必要があります．

### アノテーション周り

```
"experimentalDecorators": true,
```

これを`tsconfig.json`に書くとエラーが消えました．
