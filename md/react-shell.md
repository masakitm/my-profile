# サクッとReact + typescript書きたいとき一発で環境構築するシェルスクリプトを作る

## 概要
Reactの環境構築はcreate-react-appで全然いい。  
Next.jsでもGatsbyでも全然いい。  
  
いいのだけど、サッと書きたいだけのときは重い。  
そう思って最低限必要だと思うものを1コマンド叩けばいいようにスクリプトを書いた。
  
## やること
最終結果： 現在のディレクトリにreact環境を作る

1. tsconfig.jsonを作る
2. webpack.config.jsを作る
3. index.htmlを作る
4. src/index.tsx, src/components/App.tsxを作る
5. npmパッケージ webpack webpack-cli webpack-dev-server html-webpack-plugin typescript ts-loader をインストールする
6. react関連のパッケージ react react-dom @types/react @types/react-dom をインストールする
  
eslintやprettierは必要なら別途入れる。  
個人でサクッと書く分にはvscode側のlint拡張とかで済ませていいと思うので含めていない。  
css in jsも必要なら後で入れればいい。プロダクト開発のテンプレートではないので。  
  
今回はwebpackにしたけど、バンドラーは好きなものを使っていい。

## 解説
  
まずはtsconfigファイルを生成する関数を作る。  
シェルはzshを使っている。  
  
```
function create_tsconfig() {
  touch tsconfig.json # 現在のディレクトリに tsconfig.json を作る
  
  echo '{
  "compilerOptions": {
    "target": "es2019",
    "module": "esnext",
    "moduleResolution": "node",
    "jsx": "react",
    "strict": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true
  }
}' >> tsconfig.json # echo '' の中に tsconfig.json に書き込みたい内容を書く
}
```
  
シェルは我流なので詳しくないのだけど、改行文字列をうまいことインデント整えつつ書く方法はないんだろうか。  
次に webpack.config.js を作る。  
  
```
function create_webpack() {
  touch webpack.config.js # 現在のディレクトリに webpack.config.js を作る

  echo 'const path = require("path")
const HTMLPlugin = require("html-webpack-plugin")

module.exports = {
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: {
          loader: "ts-loader",
          options: {
            transpileOnly: true,
          },
        },
      }
    ],
  },
  resolve: {
    extensions: [".js", ".ts", ".tsx", ".json"]
  },
  plugins: [
    new HTMLPlugin({
      template: path.join(__dirname, "src/index.html"),
    })
  ]
}' >> webpack.config.js # echo '' の中に webpack.config.js に書き込みたい内容を書く
}
```

世のシェル職人は1関数にまとめて処理を書いてるイメージがあるけど、  
個人的には関数をこまめに分けたほうが後からカスタムしやすくて好き。  
  
最後にreactを導入してセットアップする関数を作る。  
ここはまとめたけど、責務で分割してもいいと思う。  

```
function create_react_env() {
  # 現在のパスを変数に代入
  current_path=$PWD

  # エントリーポイントの index.html に書き込みたい html を定義
  html_template='<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><title>Document</title></head><body><div id="root"></div></body></html>'

  # index.tsx に書き込みたい ts を定義
  index_tsx_templlate="import React from 'react'\nimport ReactDOM from 'react-dom'\nimport { App } from './components/App'\n\nconst root = document.getElementById('root')\n\nReactDOM.render(\n  <App />,\n  root\n)"

  # App.tsx に書き込みたい ts を定義
  app_tsx_template="import React from 'react'\n\nexport function App() {\n  return (\n    <div>\n      <h1>Welcome to react app</h1>\n    </div>\n  )\n}"

  # files
  # ダミーのエントリーファイルを作成
  touch main.js

  # tsconfig.jsonとwebpack.config.jsを作成
  create_tsconfig
  create_webpack

  # setup
  # 必要なパッケージをインストール
  # webpackとローカルサーバー、react類、型ファイル
  npm init
  npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin typescript ts-loader
  npm i -S react react-dom @types/react @types/react-dom

  # package.json scripts
  # package.json の scripts に書き込みたい内容を追加
  sed -i -e '7i \ \ \ \ "build": "webpack --mode production",' package.json
  sed -i -e '7i \ \ \ \ "dev": "webpack-cli serve --mode development",' package.json

  # src
  # srcディレクトリを切って移動
  mkdir src
  cd src

  # 定義した内容を書き込む
  touch index.html
  echo -e ${html_template} >> index.html

  touch index.tsx
  echo -e ${index_tsx_template} >> index.tsx

  # hello world
  mkdir components
  cd components
  touch App.tsx
  echo -e ${app_tsx_template} >> App.tsx

  # end
  cd $current_path
  rm main.js

  echo 'A new react project has created'
  echo 'Run "$ npm run dev" to start a dev server'
}
```
  
自分はこの内容を.zshrcにつっこんであり  
適当なディレクトリで $ create_react_env を叩くだけでreact + tsが書けるようにしている。  
.shファイル化するもよし、好きにカスタムするもよし。  
  
めでたしめでたし。