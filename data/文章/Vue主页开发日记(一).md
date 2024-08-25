---
title: Vue主页开发日记(一)
date: 2024-03-11 04:00:00
categories: default
tags: []
---
webpages虽然开源了，但是那是之前的html模板，现在已经逐渐适配Vue。在编写Vue时，为了记录开发进程，以日记的方法来记录
> 我的主页: https://www.nuoyis.net  
> 我的开源库: https://github.com/nuoyis/webpages

Vue的编写前肯定得安装nodejs和其框架,这个需要官网上去了解  
网站地址: https://nodejs.org  
Vue官网: https://cn.vuejs.org/

建议安装框架时，使用pnpm或cnpm,会减少报错和耐心
```shell
npm install -g pnpm
pnpm set registry https://registry.npmmirror.com/
pnpm create vue@latest
```
项目名一输，建议保留router

✔ Project name: … <your-project-name>  
✔ Add TypeScript? … **No** / Yes  
✔ Add JSX Support? … **No** / Yes  
✔ Add Vue Router for Single Page Application development? … No / **Yes**  
✔ Add Pinia for state management? … **No** / Yes  
✔ Add Vitest for Unit testing? … **No** / Yes  
✔ Add an End-to-End Testing Solution? … **No** / Cypress / Playwright  
✔ Add ESLint for code quality? … **No** / Yes  
✔ Add Prettier for code formatting? … **No** / Yes  

Scaffolding project in ./<your-project-name>...  
Done.

然后创建好后就是三件套
```shell
cd <your-project-name>
pnpm install
pnpm run dev
```

`pnpm run dev`即是项目启动预览，如果你要项目静态化,直接`pnpm run build`,生成的文件夹为./dist  
如果你想直接流水线部署(github actions),请复制以下yml并在`.github/workflows`内创建(名称随意)
```shell
name: Build webpages for vue
on:
  workflow_dispatch:
  push:
    branches: [main, master] # 当监测 main,master 的 push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2 # If you're using actions/checkout@v2 you must set persist-credentials to false in most cases for the deployment to work correctly.
      with:
        persist-credentials: false
    - name: Install and Build
      run: |
        npm install
        npm run build
    - name: Deploy
      uses: JamesIves/github-pages-deploy-action@releases/v3
      with:
        ACCESS_TOKEN: ${{ secrets.ACTION_TOKEN }}
        BRANCH: gh-pages
        FOLDER: dist
```
这个ACTION_TOKEN你需要在github内获取token,并添加变量，如果你不知道位置，请在你的库名后加入`/settings/secrets/actions`,例如:https://github.com/nuoyis/webpages/settings/secrets/actions  
然后Vue项目启动预览后，基本的框架已经建设好了，你要修改的全在src内。建议是除了main.js和app.vue，其他的移动出项目外,以便在没有思绪的情况下看看官方的写法。  
项目根目录下的`index.html`可以修改head区域，body就不建议修改。
接下来的内容在(二)中编写，感谢你的观看。