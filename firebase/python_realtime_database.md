python3からfirebaseのRealtime Databaseにアクセスする方法なんかをまとめました．

## install

pythonでfirebaseを利用するには，`firebase-admin`というものがいるのでそれを入れます．
installは以下のコマンドで

```
sudo pip3 install firebase-admin
```

## 下準備
まずfirebaseで新規プロジェクトを作成してください．
作成したプロジェクトに入ると，こんな画面が出てくると思います．

まずはProject Overviewと書かれた文字の右側の歯車をクリックして設定画面へ行ってください．
こんな画面になると思います．

そしてサービスアカウントに行って，一番下の新しい秘密鍵の生成で秘密鍵のjsonを入手してください．
秘密鍵は***pythonコードと同じディレクトリ***に配置してください．


次に左のメニューから`Database`を選択します．

そうすると以下のような画面が出てくると思います．

ルールを開いて，ルールを以下のように変更します．

```
{
  "rules": {
    "public_resource": {
      ".read": true,
      ".write": true
    },
    "some_resource": {
      ".read": "auth.uid === 'my-service-worker'",
      ".write": false
    },
    "another_resource": {
      ".read": "auth.uid === 'my-service-worker'",
      ".write": "auth.uid === 'my-service-worker'"
    }
  }
}
```

これで下準備は完了です．

## アクセスする

pythonコードは以下のような感じです．

```
import firebase_admin
from firebase_admin import credentials
from firebase_admin import db

cred = credentials.Certificate('./<your service account json>')

firebase_admin.initialize_app(cred, {
    'databaseURL': 'https://<your database url>',
    'databaseAuthVariableOverride': {
        'uid': 'my-service-worker'
    }
})

ref = db.reference('/another_resource')

## add data to database
users_ref = ref.child('users')

users_ref.set({
    'alanisawesome': {
        'date_of_birth': 'June 23, 1912',
        'full_name': 'Alan Turing'
        },
    'gracehop': {
        'date_of_birth': 'December 9, 1906',
        'full_name': 'Grace Hopper'
        }
    })

# insert
users_ref.child('mituba').set({
    'date_of_birth': 'Aug 23, 1994',
    'full_name': 'Mituba Mituba'
    })

## get data
print(ref.get())
```

以上でアクセスできたと思います．

## 感想

楽
