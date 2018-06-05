> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 28 篇  
> https://ithelp.ithome.com.tw/articles/10195369
  
Angular應用程序以及Angular本身都依賴於很多第三方包(包括Angular自己)提供的特性和功能。這些包由Node包管理器( npm )負責安裝和維護。
因此Node.js和npm是做Angular開發的基礎。
這邊是Node.js下載連結：<a href="https://nodejs.org/en/download/">Downloads NPM</a>
如果在電腦中需要同時運行多個不同版本的npm，可以使用nvm來管理他們：<a href="https://github.com/creationix/nvm">nvm</a>
  
### package.json設定
這邊有詳細的說明文檔：<a href="https://docs.npmjs.com/files/package.json">Specifics of npm's package.json handling</a>
  
下面是一個預設CLI設定檔的範例
```javascript
{
  "name": "專案名稱應為惟一",
  "version": "0.0.0",
  "license": "MIT",
  "description": "對於這個專案的說名",
  "keywords": "可幫助在npm網站上能被搜尋到",
  "homepage": "http://claire-chang.com",
  "bugs": { 
    "url" : "https://github.com/owner/project/issues(ISSUE要被回報的網址和MAIL)",
    "email" : "project@hostname.com"
  },
  "author": "Barney Rubble (http://barnyrubble.tumblr.com/)",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "^5.0.0",
    "@angular/common": "^5.0.0",
    "@angular/compiler": "^5.0.0",
    "@angular/core": "^5.0.0",
    "@angular/forms": "^5.0.0",
    "@angular/http": "^5.0.0",
    "@angular/platform-browser": "^5.0.0",
    "@angular/platform-browser-dynamic": "^5.0.0",
    "@angular/router": "^5.0.0",
    "@angular/upgrade": "^5.0.0",
    "core-js": "^2.4.1",
    "mobx": "^3.3.1",
    "mobx-angular": "^1.9.0",
    "rxjs": "^5.5.2",
    "zone.js": "^0.8.14"
  },
  "devDependencies": {
    "@angular/cli": "1.5.0",
    "@angular/compiler-cli": "^5.0.0",
    "@angular/language-service": "^5.0.0",
    "@types/jasmine": "~2.5.53",
    "@types/jasminewd2": "~2.0.2",
    "@types/node": "~6.0.60",
    "codelyzer": "~3.2.0",
    "jasmine-core": "~2.6.2",
    "jasmine-spec-reporter": "~4.1.0",
    "karma": "~1.7.0",
    "karma-chrome-launcher": "~2.1.1",
    "karma-cli": "~1.0.1",
    "karma-coverage-istanbul-reporter": "^1.2.1",
    "karma-jasmine": "~1.1.0",
    "karma-jasmine-html-reporter": "^0.2.2",
    "protractor": "~5.1.2",
    "ts-node": "~3.2.0",
    "tslint": "~5.7.0",
    "typescript": "~2.4.2"
  }
} 
```
  
這邊說明幾個比較重要的設定值：
  
`script` : 是一個script commands的列表，這些script commands會在生命週期中的不同時間運行。其內容的key是生命週期事件，value是要執行的命令。可獲得的生命周期列表：<a href="https://docs.npmjs.com/misc/scripts">請見此</a>
  
`dependencies` : 要運行這個應用程式必要一定要安裝的package
  
`devDependencies` : 只有在開發時需要用到的功能則寫在這邊，當別人要導入我們所開發的專案時，不用一定要下載完整的devDependencies內容。
  
### 在dependencies需載入的Angular Packages
**angular/animations** : 網頁切換動畫效果  
**@angular/common**: services, pipes, and directives等都在這邊  
**@angular/compiler**: Angular的模板編譯器，在JIT  
**@angular/core**: 基本angular框架所需的功能如metadata decorators, Component, Directive, dependency injection,以及元件的lifecycle hooks都在裡面  
**@angular/forms**: template-driven和reactive forms的表單驗證功能都在這  
**@angular/http**: Angular的舊版即將被棄用的HTTP客戶端  
**@angular/platform-browser**: 一切DOM和瀏覽器相關的在這，AOT編譯方法也在這  
**@angular/platform-browser-dynamic**: JIT編譯器在這  
**@angular/router**: 路由器功能  
**@angular/upgrade**: 從angularJS升級所需使用的  
**core-js**: Polyfill packages  
**zone.js**: 一個為 Zone規範提供的填充庫  
**rxjs**: 新版的Angular主要使用的HTTP方法模組  
**bootstrap**: bootstrap是一個可以簡單做出UI的視覺框架  