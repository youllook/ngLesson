> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 10 篇  
> https://ithelp.ithome.com.tw/articles/10194566
  
# Hooks的生命週期
### Lifecycle Hooks
<a href="https://angular.io/guide/lifecycle-hooks">有關Lifecycle Hooks的原文</a>
  
![](/source/images/hook.png)
  
#### 一個組件有一個由Angular管理的生命週期。
  
1. Angular創建、產生元件，當元件的數據綁定屬性改變時做檢查並確認，並在元件從DOM中刪除它之前destroys掉該元件。
  
2. Angular提供lifecycle hooks，可以讓我們在各個階段加上我們要讓元件做的事情。
  
3. directive具有相同的lifecycle hooks。
  
### 如何使用Lifecycle Hooks
下面為一個使用範例
```javascript
export class PeekABoo implements OnInit {
  constructor(private logger: LoggerService) { }

  // implement OnInit's `ngOnInit` method
  ngOnInit() { this.logIt("OnInit"); }

  logIt(msg: string) {
    this.logger.log("#${nextId++} ${msg}");
  }
}
```
### Lifecycle sequence
####`ngOnChanges()`  
Angular設置數據綁定的輸入屬性。會在`ngOnInit()`之前被呼叫
  
####`ngOnInit()`  
在Angular之後初始化指令，組件首先顯示數據綁定屬性，並設置directive及component的輸入屬性。
  
####`ngDoCheck()`  
檢測Angular無法或無法自行檢測到的更改，並採取相應措施。
  
####`ngAfterContentInit()`  
在Angular將外部內容設置到template之後被呼叫。
  
####`ngAfterContentChecked()`  
在Angular檢查投影到組件中的內容之後被呼叫。
  
####`ngAfterViewInit()`  
初始化組件的template和sub-template之後被呼叫。
  
####`ngAfterViewChecked()`  
在Angular檢查組件的視圖和子視圖之後作出響應。
  
####`ngOnDestroy()`  
在Angular破壞指令/組件之前進行清理。取消訂閱Observables和事件處理程序以避免內存洩漏。