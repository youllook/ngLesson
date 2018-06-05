> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 18 篇  
> https://ithelp.ithome.com.tw/articles/10195338
  
# NgModules
### Angular modularity
Angular有提供許多的功能，如FormsModule、HttpModule、RouterModule，都是NgModules。一些第三方資源提供者也有提供許多NgModules可使用，如Material Design、Ionic、AngularFire2等...。 NgModules可將一群功能性一致的components、directives和pipes組合在一起，讓外部可以直接引用並使用這個模組。例如FormsModule裡面會提供許多和表單驗證、表單資料繫結等的功能在裡面。

在要開發一個APP時，我們可以透過@NgModule的metadata去設定這一個APP會使用到的功能模組，設定的樣子如下：

（以src/app/app.module.ts為例）
<pre>
    import { NgModule }      from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    @NgModule({
    imports:      [ BrowserModule ],
    providers:    [ Logger ],
    declarations: [ AppComponent ],
    exports:      [ AppComponent ],
    bootstrap:    [ AppComponent ]
    })
    export class AppModule { }
</pre>

### NgModule的metadata有下面幾項:
1. imports：這個模組所需用到的Angular提供的或第三方提供的Angular資源庫（如FormsModule、HttpModule等）。
2. providers：一些供這個模組使用的service，在此宣告後所有下面的元件都可以直接使用這個服務。
3. declarations：這個Module內部Components/Directives/Pipes的列表，聲明這個Module的內部成員
4. exports：用來控制將哪些內部成員暴露給外部使用。
5. bootstrap：這個屬性只有根模組需要設定，在此設定在一開始要進入的模組成員是那一個。