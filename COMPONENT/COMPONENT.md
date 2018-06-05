> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 2 篇  
> https://ithelp.ithome.com.tw/articles/10192652

# 創立Angular的元件
### 使用CLI來為專案建立一個元件
用這個指令，CLI會為我們初始化一個新的元件樣版 
<pre>
ng generate component heroes
</pre>

![](/source/images/component.png)
  
這時我們開啟app/heroes/heroes.component.ts
```javascript
    import { Component, OnInit, ViewEncapsulation } from '@angular/core';

    @Component({
    selector: 'app-heroes',
    templateUrl: './heroes.component.html',
    styleUrls: ['./heroes.component.css']
    })
    export class HeroesComponent implements OnInit {
    hero = 'heroes works!';//增加一個變數
    constructor() { }

    ngOnInit() {
    }

    }
```
  
只要創建元件，都必需從Angular去import Component。 而@Component則是用來定義這一個元件的相關資訊，有三個metadata
1. <mark>selector</mark> : the components CSS element selector以及在HTML裡要宣告的TAG名稱
2. <mark>templateUrl</mark> : 要使用的HTML樣版位置
3. <mark>styleUrls</mark> : 專為這個元件設定的CSS
  
要注意的是，我們通常會使用<mark>export class</mark>，以方便在其他的模組裡可以import來使用

### 修改元件內容
打開heroes.component.ts
```javascript
    import { Component, OnInit, ViewEncapsulation } from '@angular/core';

    @Component({
    selector: 'app-heroes',
    templateUrl: './heroes.component.html',
    styleUrls: ['./heroes.component.css'],
    encapsulation: ViewEncapsulation.None
    })
    export class HeroesComponent implements OnInit {
    hero = 'heroes works!';//增加一個變數
    constructor() { }

    ngOnInit() {
    }

    }
```
  
修改heroes.component.html，使用{{hero}}來顯示剛剛在TS檔裡定義的變數
```html
<h1>{{hero}}</h1>
```
  
### 將元件放進頁面裡

打開src/app/app.component.html
```html
<app-heroes></app-heroes>
```
  
這時候就可以在頁面中看到剛剛我們所增加的內容
  
### 使用物件
創建一個Hero物件 src/app/hero.ts
```javascript
export class Hero { 
   id: number; 
   name: string; 
}
```
  
打開src/app/heroes/heroes.component.ts，給予物件正確的值
```javascript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';

@Component({
selector: 'app-heroes',
templateUrl: './heroes.component.html',
styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit 
{
//在這邊設定物件內容
hero: Hero = {
id: 1,
name: 'Windstorm'
};

constructor() { }

ngOnInit() {
}

}
```
  
顯示在heroes.component.ts所設定的值
  
```html
<h2>{{ hero.name }} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>
```
  
如果希望將變數格式化 則可以使用
```html
<h2>{{ hero.name | uppercase }} Details</h2>
```
  
這個格式化的功能叫做Pipes，更多的說明請見<a href="https://angular.io/guide/pipes">Pipes</a>說明
  
### 雙向繫結
Angular一個很方便的功能就是可以支持雙向繫結，使用[(ngModel)]能做到當欄位的值改變時，TS裡變數的值也同時被更改。
這個功能在做表單驗證時非常方便 詳細使用說明請見：<a href="https://angular.io/guide/ngmodules">NgModules</a>
  
使用方法:  

  
```html
<div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name">
    </label>
</div>
```
  
接著要去app.module.ts去加上import資訊讓專案能夠使用ngModel標籤 要注意是在app.module.ts裡喔!  
先import並且將FormsModule加進@ngModule的imports列表內，讓下面所有的元件都可以使用FormsModule的功能

```javascript
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
```

```javascript
imports: [
  BrowserModule,
  FormsModule
],
```
  
接著，要把剛剛我們所建立的元件HeroesComponent放進@NgModule.declarations裡
```javascript
import { HeroesComponent } from './heroes/heroes.component';
```
在app.module.ts的@NgModule增加
```javascript
declarations: [
  AppComponent,
  HeroesComponent
],
```
這時，我們會發現我們更動input裡的文字時，model的值也會被更改