> 以上內容均抄錄自 用30天深入Angular 5的世界系列 第 28 篇  
> https://ithelp.ithome.com.tw/articles/10195369
  
# TypeScript設定
### TypeScript配置
TypeScript是Angular應用開發中使用的主語言。瀏覽器不能直接執行TypeScript，得先用tsc編譯器轉譯(transpile)成JavaScript，而且編譯器需要進行一些配置。而配置的檔案名稱就是`tsconfig.json`
  
這邊是官方關於此配置文件的詳細說明：<a href="http://www.typescriptlang.org/docs/handbook/tsconfig-json.html">tsconfig.json</a>
  
### tsconfig.json
下面為一個`tsconfig.json`的範例
```javascript
{
  "compileOnSave": false,
  "compilerOptions": {
    "outDir": "./dist/out-tsc",
    "sourceMap": true,
    "declaration": false,
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "target": "es5",
    "typeRoots": [
      "node_modules/@types"
    ],
    "lib": [
      "es2017",
      "dom"
    ],
    "noImplicitAny": true,
    "suppressImplicitAnyIndexErrors": true
  }
}
```
  
關於設定檔裡的`noImplicitAny`意思是是否不允許typescript編譯時隱性將沒有設定類型的變數設定為any。如果設定為true的話，如果typescript裡面有沒有設定類型的變數則會產生錯誤訊息。當這個值設定為true時，記得要將suppressImplicitAnyIndexErrors也設定為true，不然會發生隱式報錯。
  
### lib.d.ts 文件
TypeScript有一個特殊的聲明文件`lib.d.ts`。包含了JavaScript運行庫和DOM的各種常用Javascript環境聲明。 基於--target，TypeScript添加額外的環境聲明，例如如果目標為es6時將添加Promise。
```javascript
"lib": ["es2017", "dom"]
```