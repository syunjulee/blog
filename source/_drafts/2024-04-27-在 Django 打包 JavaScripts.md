---
title: 在 Django 打包 JavaScripts
date: 2024-04-27 09:18:24
tags:
---
但若是想要使用 CSS 套件，以 Tailwind 為例，就要從安裝前端環境開始建立：
### 在專案資料夾中安裝前端環境
首先要在這個虛擬環境中建立一個 `package.json` 檔案
```md
$ node install 
$ npm install 
```
就可以安裝 CSS 套件
```md
$ npx tailwindcss init
```
init 之後會生出一個 `Tailwind.config.js` 檔案，它想知道電腦裡面有哪些檔案要用 Tailwind，沒有用到的就不會裝，可以節省容量，專有名詞叫做 Tree Shaking。
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  /* 我只有放在 templates 裡的 html 有用到 css */
  content: ["templates/**/*.html"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
### 建立 src/tailwind 資料夾，放 input.css 檔案
src/tailwind 裡的檔案是用來編譯語法的，編譯完會放在 static/styles/style.css 裡，在 src/tailwind 寫的語法，則會蓋過 static/styles/style.css 裡的語法，以達效果。
先到 `package.json` 裡的 `scripts` 欄做設定：
```js
"scripts": {
  "dev": "tailwindcss -i ./src/tailwind/style.css -o ./static/styles/style.css --watch"
  "build": "tailwindcss -i ./src/tailwind/style.css -o ./static/styles/style.css"
}
// -i：輸入地 -o：輸出地
// --watch 是即時預覽，開發時可啟動 npm run dev 即可開啟
```
若是開了即時預覽，建議開三個終端機作業，一是開 Django 的本地端伺服器，二是開 node 預覽 tailwindcss 效果，三則是負責輸入一般指令。
設定完之後就可以啟動 tailwind 預覽：
```md
$ npm run dev
```
接下來若要撰寫 CSS，除了在 html 檔上加上 class，也可以到 `src/tailwind/style.css` 去寫。
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-red-400;
}
``` 