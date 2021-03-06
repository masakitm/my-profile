
# 世界一伝わるバグ報告の書き方

## ！結論から！

1. 仕様や状況を知らない人にもわかるように書く
2. 5W1Hを明確にする

### 1. 仕様や状況を知らない人にもわかるように書く

読み手が発生箇所の仕様や現状に詳しい人、同じコンテキストを共有している人とは限りません。  
なるべく知識のない人にもわかるように書きましょう。  

1. 特殊な用語や略称を避け、なるべく正式名称に近い名前で書く
2. `先月に修正した場所` などのコンテキストを要求する書き方も避け、第三者視点でわかるように明示する  
  
以上2点を重視するとよいでしょう。

### 2. 5W1Hを明確にする

どこで何が起きたかわからないと現象再現ができないためです。  
担当エンジニアとやり取りが増える原因は主にここです。  

書いてあるとうれしいこと
* いつ
* どこで
* 何を
* どうしたら
* どうなったか
  
+ 発生環境も書いてあるとさらによいでしょう。

### 例

では、例をあげて見てみましょう。

#### 伝わらないバグ報告例

```
こないだ直した画面のリストが〇〇になってます！
```

→ なんもわからん

```
### バグ報告
こないだ直した画面(いつ？だれが？)(どこ？)
のリスト(なにそれ？)
が〇〇になってます！(何をしたとき？)(本来はどうなるの？)
```

第一報ならこれでもいいけれど、もう少し情報がほしいですね。

#### 伝わるバグ報告例

```
### 詳細
〜〜画面に遷移した時、
初期表示が終わったタイミングで、
▲▲ボタンを押した場合、
■■エリア下部の★★選択セレクトボックスの値が〇〇になっています

### 補足
本来はxxが表示されるのが正のため、不具合です
Win10 chrome最新版にて現象を確認しました
```


→　わかる  

```
### 詳細
[where] 〜〜画面に遷移した時、
[when] 初期表示が終わったタイミングで、
[how] ▲▲ボタンを押した場合、
[what] ■■エリア下部のリスト選択セレクトボックスの値が〇〇になっています

### 補足
[why] 本来はxxが表示されるのが正のため、不具合です
[発生環境] Win10 chrome最新版にて現象を確認しました
```

いつ、どこで、なにをしたら、どうなったか、なぜ不具合なのかが書いてありますね。  
担当エンジニアは泣いて喜ぶことでしょう。  

### まとめ

パッと見めんどうに見えるけど、メリットの多い書き方です。
  
1. フォーマットが決まっているので文を考えるコストが減る  
2. 担当エンジニアへの説明コストが減る  
3. 担当エンジニア自身もリーディングコストが減り、調査に集中できる  


ぜひバグ報告フォーマットを活用してみましょう！  

```
### 詳細
[where] どこで
[when] いつどのタイミングで
[how] どうしたら
[what] なにがおきるか

### 補足
[why] なぜ不具合なのか
```