1. `output { outlibraryTarget: 'umd' }`
    ```
    Bug: Failed to mount component: template or render function not defined
    ```

2. still need install `gesang-dropdown ` after `npm install`
    ```json
    <!-- import 需要从package.json的main中找到组件的入口 -->
    <!-- 否则 终端仍然会提醒: gesang-dropdown没有安装, 需要安装 -->
    <!-- 解决: 需要在package.json 中指名组件被调用文件地址。即: -->
    "main": "dist/gesang-dropdown.js"
    ```

4. add  `src/index.js` to entry in `webpack.production.js` and export component in `src/index.js` to build dist folder