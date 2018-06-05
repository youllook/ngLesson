> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 9 篇  
> https://ithelp.ithome.com.tw/articles/10193917
  
# 資料繫結的模版語法
### Template Syntax
透過模版語法，template可以與component做許多的溝通。
  
![](/source/images/databind.png)
  

> 對於template.html來說，所有的html標籤都可以使用，除了`<script>`以外，這是為了維護模版的安全性，去除template被攻擊的> 風險，因此在template中所有的`<script>`會被忽略，並跳出警告。

接下來我們來介紹Angular的模板語法
  
```javascript
{{...}}
```
  
以下為一個範例
```html
<h3>
  {{title}}
  <img src="{{heroImageUrl}}" style="height:30px">
</h3>
```
  
大括號之間值通常是組件屬性的名稱。Angular使用相應組件屬性的字符串值替換該名稱。  
在上面的例子中，Angular會取元件裡title和heroImageUrl的屬性，並會取代`{{title}}`及`{{heroImageUrl}}`，
因此在頁面上會顯示一個大的應用程序標題，然後一個英雄影像的位置。
  
在`{{...}}`之間，我們也可以使用模板表達式去轉換要顯示的值。  
例如在刮弧中做運算：  
```javascript
The sum of 1 + 1 is {{1 + 1}}
```
  
或者也可以呼叫component的function `getVal()`：
```javascript
The sum of 1 + 1 is not {{1 + 1 + getVal()}}
```
  
大多的運算符都可以用在表達式裡面，除了一些會影響到component的值的運算符，如=、+=、-=之類。
有時候`{{...}}`裡面要綁定的數值也可以是在template定義的(使用#符號)，請見下面的範例
```html
<div *ngFor="let hero of heroes">{{hero.name}}</div>
<input #heroInput> {{heroInput.value}}
```
  
> 「註」：如果在元件裡已經有變數名稱叫做hero，而template裡又有一個hero，在template會優先使用template內定義的變數。
在表達式中不能引用任何除了undefined外的全域變數，如window或document，也不能用consolo.log，只能使用元件內的屬性或方法，或者template裡上下文內的成員

在使用`{{...}}`時，有下面四個原則要注意：
1. No visible side effects：不應該改變任何元件內的值，在rendering整個表式示時應該是穩定的
2. Quick execution：表達式的運算應該要很快，因為它會在許多狀況下被呼叫，因此若是裡面含有許多複雜運算時，請考慮用快取以增加效能。
3. Simplicity：雖然可以在模版語法裡面寫很複雜的運算但是不建議，最多在裡面使用 `!` 符號，不然還是建議將運算放到元件內去計算，以利閱讀及開發
4. Idempotent: idempotent的意思是如果相同的操作再執行第二遍第三遍，結果還是跟第一遍的結果一樣 (也就是說不管執行幾次，結果都跟只有執行一次一樣)。
  
### 利用*ngFor來做迴圈顯示完整列表
首先開啟src/app/heroes/heroes.component.ts  
設定一個變數heroes
```javascript
 heroes: Hero[] = [
    { id: 11, name: 'Mr. Nice' },
    { id: 12, name: 'Narco' },
    { id: 13, name: 'Bombasto' },
    { id: 14, name: 'Celeritas' },
    { id: 15, name: 'Magneta' },
    { id: 16, name: 'RubberMan' },
    { id: 17, name: 'Dynama' },
    { id: 18, name: 'Dr IQ' },
    { id: 19, name: 'Magma' },
    { id: 20, name: 'Tornado' }
  ];
```
接著開啟heroes.component.html，利用*ngFor來迴圈式的顯示列表內容
  
```html
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```
結果如下圖：
  
![](/source/images/list.png)
  
<a href="更多資訊有關於ngFor">更多資訊有關於ngFor</a>
  
### 使用(click)來設定觸發事件
開啟src/app/heroes/heroes.component.ts，在li裡面增加click的事件
```html
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```
<a href="https://angular.io/guide/template-syntax#event-binding">更多資訊有關於event-binding</a>
  
### 以條件式去增加元件的類別
打開heroes.component.html，在li內增加 [class.要增加的類別名稱]="條件式為true時增加"
```html
<li *ngFor="let hero of heroes"
  [class.selected]="hero === selectedHero" 
  (click)="onSelect(hero)">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
```
  
這樣當`hero === selectedHero`為真時，這個li就會被加上selected這個類別
  
接下來再到src/app/heroes/heroes.component.css設定該類別的CSS，就能突顯現在所選擇的是那一個了
```css
.selected{
   color:red;
}
```
結果如下圖：
  
![](/source/images/list2.png)