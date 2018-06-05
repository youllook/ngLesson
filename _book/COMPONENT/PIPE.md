# Pipes
### Pipes有那些？
常用的Pipes有<a href="https://angular.io/api/common/DatePipe">DatePipe</a>，
<a href="https://angular.io/api/common/LowerCasePipe">UpperCasePipe</a>，
<a href="https://angular.io/api/common/CurrencyPipe">LowerCasePipe</a>，
<a href="https://angular.io/api/common/PercentPipe">CurrencyPipe</a>，
和PercentPipe。它們都可用於任何模板。
  
### 下面是angular內建所有的的Pipes說明：
  
**AsyncPipe**：給Observable或Promise返回已發出的最新值  
**CurrencyPipe**：格式化數字為錢幣格式  
**DatePipe**：格式化一串日期文字  
**DecimalPipe**：格式化小數點  
**DeprecatedCurrencyPipe**：Use currency to format a number as currency.  
**DeprecatedDatePipe**  
**DeprecatedDecimalPipe**  
**DeprecatedPercentPipe**  
**I18nPluralPipe**：Maps a value to a string that pluralizes the value according to locale rules.  
**I18nSelectPipe**：Generic selector that displays the string that matches the current value.  
**JsonPipe**：Converts value into JSON string.  
**LowerCasePipe**：轉文字為小寫  
**PercentPipe**：轉為百分比  
**SlicePipe**：Creates a new List or String containing a subset (slice) of the elements.  
**TitleCasePipe**：Transforms text to titlecase.  
**UpperCasePipe**：轉文字為大寫  
  
### 如何使用Pipes
管道將數據作為輸入並將其轉換為所需的輸出。下面是使用DatePipe的範例。
```html
<p>The hero's birthday is {{ birthday | date:"MM/dd/yy" }} </p>
<p>The hero's birthday is {{ birthday | date:"shortDate" }} </p>
<p>The hero's birthday is {{ birthday | date:"fullDate" }} </p>
```
  
### 在Pipes裡使用參數

請見下面的範例
```html
<p>The hero's birthday is {{ birthday | date:"MM/dd/yy" }} </p>
<p>The hero's birthday is {{ birthday | date:"shortDate" }} </p>
<p>The hero's birthday is {{ birthday | date:"fullDate" }} </p>
```  
這樣會顯示結果如下：  
MM/dd/yy：04/15/88  
shortDate：04/15/1988  
fullDate：Friday, April 15, 1988  
  
### 串接多個管道
```html
{{ birthday | date:'fullDate' | uppercase}}
```  
  
結果會顯示  
FRIDAY, APRIL 15, 1988  

### 定義自己的Pipes
下面是一個自製Pipes的例子：  
```javascript
import { Pipe, PipeTransform } from '@angular/core';
/*
 * Raise the value exponentially
 * Takes an exponent argument that defaults to 1.
 * Usage:
 *   value | exponentialStrength:exponent
 * Example:
 *   {{ 2 | exponentialStrength:10 }}
 *   formats to: 1024
*/
@Pipe({name: 'exponentialStrength'})
export class ExponentialStrengthPipe implements PipeTransform {
  transform(value: number, exponent: string): number {
    let exp = parseFloat(exponent);
    return Math.pow(value, isNaN(exp) ? 1 : exp);
  }
}
```
  
在上面的例子中可以看到
  
* 會以`@Pipe({name:'XXXX'})`來宣告這個class是一個pipe
* pipe類別需implements `PipeTransform`介面並依照要input的值來選擇要實作的transform方法
* transform有一個可選參數exponent，可讓使用者帶要帶入的參數進Pipes
* Pipe的名字需要是一個合法的Javascript命名
  
下面是一個使用範例
  
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-power-booster',
  template: `
    <h2>Power Booster</h2>
    <p>Super power boost: {{2 | exponentialStrength: 10}}</p>
  `
})
export class PowerBoosterComponent { }
```
顯示的結果如下：
  
![](/source/images/pipe.png)
  
在每次被綁定的值更動時，都會再跑一次Pipes的功能，一般來說，Pipe只會偵測值的變化才會執行pure Pipes。
如對象是String、Number、Boolean、Symbol、Date、Array、Function、Object。**但如果裡面是一個物件，則pure Pipes會忽略他的更動**。   
這是因為效能的考量，若為純粹物件的值的更動在偵測上較快，但是在物件上屬性的更改的偵測效能會較差。會建議改使用元件的方式去偵測改變。  
但angular還是提供了impure pipes的方式可以偵測物件的改變，但使用上要小心不能因此而拖慢系統速度。 它看起來會是像這樣：  
```javascript
@Pipe({
  name: 'flyingHeroesImpure',
  pure: false
})
export class FlyingHeroesImpurePipe extends FlyingHeroesPipe {}

```