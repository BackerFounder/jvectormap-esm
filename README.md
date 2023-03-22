# jvectormap-esm

主要是針對 [jvectormap](https://www.npmjs.com/package/jvectormap-next) 套件產生出來的變體，將 jvectormap 可以透過 ESM 載入

### 補充說明

* 目前 jvectormap 只有 backme 後台的轉址功能有使用
* 目前 backme 前端打包工具為 esbuild ， 而 esbuild 目前只支援 esm
* 不直接 fork [jvectormap](https://www.npmjs.com/package/jvectormap-next) 而是直接使用該套件輸出的檔案來調整的原因，是因為 jvectormap 是使用 UglifyJS 來打包，但 UglifyJS 本身不支援 es6 的語法，同時也代表不支援 esm，因上述原因才直接針對輸出的檔案調整
