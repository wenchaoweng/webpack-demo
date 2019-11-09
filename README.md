# webpack-demo
为什么有了sass还要有scss？
Ruby社区发明了SASS语法，
body
p
color:red
前端人员不懂，
于是，Ruby社区又推出SCSS语法，
body{
p{
color:red;
}
}
前端懂了。

前言：
sass语法完全兼容css语法，只是在css基础上增加了新语法。

安装：
1. https://github.com/sass/node-sass
2. npm install -g mirror-config-china --registry=http://registry.npm.taobao.org
npm install node-sass

使用：
node-sass src/style.scss dest/style.css

注意事项：
1. Html里面link只能是css文件，不能用scss文件，浏览器不认。
2. 如果要实时监控scss文件的更改，自动翻译为css，可以加-w参数。
如，node-sass src/style.scss dest/style.css -w

如何将ES6转为ES5语法，兼容IE
使用beble，https://www.babeljs.cn/docs/usage

安装：
npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save @babel/polyfill

配置：
在项目的根目录下创建一个命名为 babel.config.js 的配置文件
const presets = [
  [
    "@babel/env",
    {
      targets: {
        edge: "17",
        firefox: "60",
        chrome: "67",
        safari: "11.1",
      },
      useBuiltIns: "usage",
    },
  ],
];

module.exports = { presets };

使用：
./node_modules/.bin/babel src --out-dir lib

如何使用Webpack
安装：
npm install --save-dev webpack

配置：
在项目的根目录下创建一个命名为webpack.config.js的配置文件
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};

Webpack添加babel loader
安装：
npm install -D babel-loader @babel/core @babel/preset-env webpack

在webpack.config.js里面增加
module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
}

执行 npx webpack
bash.exe"-3.1$ npx webpack
Hash: 95a491b6b3a19052deba
Version: webpack 4.41.2
Time: 1125ms
Built at: 2019-11-08 23:50:50
  Asset     Size  Chunks             Chunk Names
main.js  3.8 KiB    main  [emitted]  main
Entrypoint main = main.js
[./src/index.js] 32 bytes {main} [built]

Webpack添加sass loader
安装：
npm install sass-loader node-sass webpack --save-dev
npm install style-loader css-loader

配置：
在webpack.config.js里面增加
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          'style-loader',
          // Translates CSS into CommonJS
          'css-loader',
          // Compiles Sass to CSS
          'sass-loader',
        ],
      },
    ],
  },
};

Webpack添加postcss loader
安装：
npm i -D postcss-loader
npm i postcss-import
npm i postcss-cssnext
npm i cssnano

配置：
在项目的根目录下创建一个命名为postcss.config.js的配置文件
module.exports = {
  //parser: 'sugarss',
  plugins: {
    'postcss-import': {},
    'postcss-cssnext': {},
    'cssnano': {}
  }
}

在webpack.config.js里面增加
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          { loader: 'css-loader', options: { importLoaders: 1 } },
          'postcss-loader'
        ]
      }
    ]
  }
}

使用：
npx webpack --watch
