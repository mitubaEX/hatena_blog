# なんちゃってreduxについての自分的なメモ

先日reduxを用いた開発を行ってみたので、自分なりにまとめてみました。

reduxは以下の記事が参考にしつつ頑張りました。

- [React+Redux入門](https://qiita.com/erukiti/items/e16aa13ad81d5938374e)
- [ReactとReduxを結ぶパッケージ「react-redux」についてconnectの実装パターンを試す](https://qiita.com/MegaBlackLabel/items/df868e734d199071b883)


## hello world

今回はcreate-react-appを利用して書いていきました

npm install -g create-react-app
create-react-app my-app

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
npm install
npm start

## reduxアーキテクチャの導入

今回は以下のサンプルを参考にコードを書いていきます

- [todoアプリ](https://github.com/reactjs/redux/tree/master/examples/todos)


