> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 18 篇  
> https://ithelp.ithome.com.tw/articles/10195338
  
# Angular CLI
### 使用CLI加速開發速度
這邊是Angular CLI的官網：https://cli.angular.io/
這邊有很詳細的Angular CLI的教學：
https://dotblogs.com.tw/wellwind/2016/09/30/angular2-angular-cli


> 直接使用

<pre>
ng --help
</pre>

> 如果我們使用CLI來創建這個component, directive或pipe，CLI會自動將這個directive宣告在app.module.ts的metadata裡

<pre>
ng generate directive highlight
</pre>

下圖是ng generate可以帶的參數  
![](/source/images/generate.png)
### Service providers
<p>
一個service可以注入至組件(@NgModule.providers)或者是元件(@Component.providers)。

如果NgModule與Component同時有分別設定相同的類別，會產生兩個不同的實體，在該元件內則會優先注入使用元件裡自己設定的服務。

一般而言，如果這個服務被廣泛的使用在很多的元件裡，我們會將providers宣告在根模組上。但如果這個服務只有這個元件在使用，則會放在Component的providers去宣告。
</p>

下面的CLI指令可以創立一個user服務並加到模組裡使用
<pre>
ng generate service user --module=app
</pre>
可以看到app.module.ts的providers會增加一個名為UserService的類別
<p>
在使用上，如果已經有設定好providers後，只要在元件的constructor裡面宣告一個變數是providers裡面設定好的service，
就可以在元件裡直接取用了
</p>
<pre>
 constructor(userService: UserService) {
    this.user = userService.userName;
  }
</pre>

其依賴注入的概念大概是這樣  
![](/source/images/injector-injects.png)  
所有在providers裡面被宣告的物件實體會被存在angular的服務列表裡，因此在下面不同的元件裡可以直接取用到相同的服務實體。  

### NgModule imports
如果要使用ngIf這個directive，我們需要在app.module.ts導入BrowserModule
<pre>
imports: [ BrowserModule ],
</pre>

這樣就可以在所有的元件中使用像下面這樣的模板語法

```html
    <p *ngIf="user">
    <i>Welcome, {{user}}</i>
    <p>
```
## Re-exported NgModules
其實上面的例子裡，ngIf是在CommonModule之下的，但是我們import了BrowserModule，我們可以看看官網對於BrowserModule的介紹
![ngModules](/source/images/ngmodules.png)

由於BrowserModule在exports的地方將自己所import的CommonModule的功能再export出去，所以當我們import了BrowserModule後，也可以使用CommonModule所有export的功能，這個就是Re-exported。


## 使用ModuleWithProviders
<p>
如果我們有一個子元件，而我們希望那個子元件能夠如同核心元件一般，比所有其他子元件都更優先被載入（就像*ngIf）。
或者是希望能夠在被所有元件初始化之前，優先設定好裡面的值時，可以自行將元件包成ModuleWithProviders然後在appModule用import的方式來import我們自己寫的核心元件。

在Angular最廣泛使用這個功能的是Routing，我們必需先傳入一個導航設定的資料來設定Routing。

我們也可以在自己所自製的模組裡使用這個功能。  
  
假如我們有一個服務是這樣的
<pre>
    import { Injectable } from '@angular/core';

    @Injectable()
        export class UserService {
        userName = 'Sherlock Holmes';
    }
</pre>

我們希望能夠讓模組在使用這個服務前，先設定一個新的userName再引入使用。
首先，要在剛剛的服務裡加上這些程式碼
<pre>
    constructor(@Optional() config: UserServiceConfig) {
       if (config) { this._userName = config.userName; }
    }
</pre>

然後在CoreModule裡加上forRoot方法，這個forRoot的方法接受一個service class，並回傳一個ModuleWithProviders物件
<pre>
    static forRoot(config: UserServiceConfig): ModuleWithProviders {
    return {
        ngModule: CoreModule,
        providers: [
         {provide: UserServiceConfig, useValue: config }
        ]
    };
    }
</pre>

最後在imports列表中呼叫它：
<pre>
    imports: [
        BrowserModule,
        CoreModule.forRoot({userName: 'Miss Marple'}),
        AppRoutingModule
    ],
</pre>
這時，我們會看到userName是“Miss Marple”，而不是“Sherlock Holmes”。  
注：只在應用的根模塊AppModule中調用forRoot。如果在其它模塊（特別是惰性加載模塊）中調用它則違反了設計意圖，並會導致運行時錯誤。
若我們想要防止有人重覆import這個CoreModule，可以用下面的方法
<pre>
    constructor (@Optional() @SkipSelf() parentModule: CoreModule) {
        if (parentModule) {
            throw new Error(
            'CoreModule is already loaded. Import it in the AppModule only');
        }
    }
</pre>