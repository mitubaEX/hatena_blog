### No.1
メモリインタリーブは，主記憶を複数の独立したグループに分けて各グループに交互にアクセスすることによって，主記憶へのアクセスの高速化を測る

### No.2
時間なのでバイトはいらない
平均アクセス時間 -> キャッシュメモリのアクセス時間 x ヒット率 + 主記憶のアクセス時間 x (1 - ヒット率)

### No.3
DRAMは，SRAMよりbit当たりの単価が安い
DRAMは主記憶，メモリはSRAM

SRAMはフリップフロップ

### No.4
ライトバック -> キャッシュメモリにのみ書き込み
ライトスルー -> キャッシュメモリ，主記憶，両方に書き込む

### No.5
1.5Mバイトのデータ圧縮率が40%なので，1.5*0.4 = 6
ここから20M/sなので 6/ 20 = 0.3秒
あとは6* 0.03


### No.7
フラッシュメモリ -> ブロック単位で電気的に内容の消去ができる．
フラッシュメモリに使用されるのはSRAM

### No.9
MMU -> CPUが指定した仮想アドレスと物理アドレスの対応関係を持っている

### No.16
ダイレクトマップ方式 -> 一つのメモリブロックをキャッシュ内の単一のロケーションに割り当てる

### No.17
ライトバックは，コヒーレンシ（一貫性）を保つ処理を実装しないといけない


### No.20
ライトバックでは，キャッシュメモリからデータが追い出された時に主記憶にデータを書き込む


### No.22
ハミング符号は 2bit誤り検出可能

### No.28
セットアソシアティブ方式 -> キャッシュを連続したブロックごとにまとめそのブロック内ならどこでも格納ができる方式
複数のブロックに対応付けることが可能．

ダイレクトマッピングは1ブロックのみ対応付
フルアソシアティブ方式は，全てに対応

### No.38
ランダムにアクセスされまくると，キャッシュはあまり効果がない

### No.44
CD-R -> レーザ光によってピットと呼ばれる焦げ跡を作ってデータを記録する光ディスク

### No.48
ハミング符号 -> メモリの誤り制御に用いられ，自動訂正機能を持つ

