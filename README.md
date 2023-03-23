# jvectormap-esm

主要是針對 [jvectormap](https://www.npmjs.com/package/jvectormap-next) 套件產生出來的變體，將 jvectormap 可以透過 ESM 載入

### 使用 ESM 來載入的原因：

主要是解決 `jquery 找不到 vectorMap 方法的問題`，原因如下：

仔細看了一下 `node_modules/jquery-jvectormap.js` 的檔案，發現裡面的行為滿奇怪的，裡面同時載入了兩個版本的介面的 jVectorMap，但一定要載入到 3+ 的版本，才不會出現 `jquery 找不到 vectorMap 的錯誤`，如下面 code 所示：

如果單純使用 `commonjs` 了話，會只引用到 3.1.9 版本所以會出現錯誤（另外一提如果同時支援 amd 與 commonjs 的方式載入了話，根據下面的 code 也能同時載入兩個版本）

這邊改成 esm 的方式，主要是確保載入兩個版本的 code

```
# line: 184
/**
 * jVectorMap version 3+
 *
 * Copyright 2011-2014, Kirill Lebedev
 *
 */
(function (factory) {
    if ( typeof define === 'function' && define.amd ) {
        // AMD. Register as an anonymous module.
        define(['jquery'], factory);
    } else if (typeof exports === 'object') {
        // Node/CommonJS style for Browserify
        module.exports = factory;
    } else {
        // Browser globals
        factory(jQuery);
    }
}(function ($) {
  .....
  $.fn.vectorMap = function(options) {...}
  .....
}

# line: 254
/*! Copyright (c) 2013 Brandon Aaron (http://brandon.aaron.sh)
 * Licensed under the MIT License (LICENSE.txt).
 *
 * Version: 3.1.9
 *
 * Requires: jQuery 1.2.2+
 */
(function (factory) {
    if ( typeof define === 'function' && define.amd ) {
        // AMD. Register as an anonymous module.
        define(['jquery'], factory);
    } else if (typeof exports === 'object') {
        // Node/CommonJS style for Browserify
        module.exports = factory;
    } else {
        // Browser globals
        factory(jQuery);
    }
}(function ($) {.....}
```

### 補充說明

* 目前 jvectormap 只有 backme 後台的轉址功能有使用
* 不直接 fork [jvectormap](https://www.npmjs.com/package/jvectormap-next) 而是直接使用該套件輸出的檔案來調整的原因，是因為 jvectormap 是使用 UglifyJS 來打包，但 UglifyJS 本身不支援 es6 的語法，同時也代表不支援 esm，因上述原因才直接針對輸出的檔案調整
