# Browserslist

Broserslist是一个JS库，用于在不同的前端工具之间共享支持的浏览器列表。
它使用在 [Autoprefixer], [Stylelint], [eslint-plugin-compat]
and [babel-env-preset]中。

所有的工具，都依赖Browserslist，Browserslist将自动查找它的配置。当你在package.json配置了如下参数:

```json
{
  "browserslist": [
    "> 1%",
    "last 2 versions"
  ]
}
```

或者在`browserslist`中配置：

```yaml
# Browsers that we support

> 1%
Last 2 versions
IE 10 # sorry
```

开发人员在浏览器中设置浏览器列表，如`last 2 version`，不需要手动更新浏览器版本。
Browserslist使用 [Can i Use] 数据作为这个查询来源。

Browserslist将从tool option，`browserslist`配置，`package.json`中的`browserslist` key或者环境变量（environment variables）中拿浏览器查询。

在线Demo [online demo]。

[eslint-plugin-compat]: https://github.com/amilajack/eslint-plugin-compat
[babel-env-preset]:     https://github.com/babel/babel-preset-env
[Autoprefixer]:         https://github.com/postcss/autoprefixer
[online demo]:          http://browserl.ist/
[Stylelint]:            http://stylelint.io/
[Can I Use]:            http://caniuse.com/

## 查询

Browserslist使用下面来源之一作为浏览器查询：

1. Tool option。例如Autoprefixer中的`browsers`参数。
2. `BROWSERSLIST`环境变量
3. 当前或父目录中的`browserslist`配置文件。
4. 当前或父目录`package.json`文件中的`browserslist` key。
5. 如果上面方法中没有得到一个有效的结果，那么Browserslist将使用默认参数：`> 1%, last 2 versions, Firefox ESR`。

我们推荐在`browserslist`中配置或`package.json`中写查询。

可以通过查询指定版本（不区分大小写）:

* `last 2 versions`：每个主流浏览器的最后2个版本（即当前版本与当前版本的上一个版本）。
* `last 2 Chrome versions`：chrome浏览器的最后2个版本（即当前版本与当前版本的上一个版本）。
* `> 5%`：全局使用统计选择的版本，即市场上占用比大于5%的浏览器版本。
* `> 5% in US`：使用美国使用统计。它接受[two-letter country code]（两个字母的国家代码）。
* `> 5% in my stats`：使用[custom usage data]（自定义数据使用来源）。
* `ie 6-8`：选择一个包含的浏览器版本范围。
* `Firefox > 20`：火狐更新版本号大于20。
* `Firefox >= 20`：火狐更新版本号大于或等于20。
* `Firefox < 20`：火狐更新版本号低于20。
* `Firefox <= 20`：火狐更新版本号低于或等于20
* `Firefox ESR`：最新 [Firefox ESR] 版本
* `iOS 7`：直接指定IOS浏览器版本号（当前选择为版本7）。
* `not ie <= 8`：排除以前查询前选定的浏览器（也就是只支持IE9及以上浏览器）。可以在任何查询量添加`not `。


Browserslist工作是分离浏览器版本，但应该避免使用像`Firefox > 0`这样的无意义查询。

多个标准通过`OR`进行组合。一个浏览器版本必须与要选择的标准中的至少一个匹配。

所有查询都是基于 [Can I Use] 支持表。
例如：`last 3 iOS versions`可以选择IOS的 `8.4, 9.2, 9.3`版本（混合主流和少数），而`last 3 Chrome versions` 可以选择 chrome `50, 49, 48`版本（仅主要）。

[two-letter country code]: http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements
[custom usage data]:        #custom-usage-data
[Can I Use]:                http://caniuse.com/

## 浏览器

浏览器名称不区分大小写

### 主流浏览器

* `Chrome` for Google Chrome.
* `Firefox` or `ff` for Mozilla Firefox.
* `Explorer` or `ie` for Internet Explorer.
* `Edge` for Microsoft Edge.
* `iOS` or `ios_saf` for iOS Safari.
* `Opera` for Opera.
* `Safari` for desktop Safari.
* `ExplorerMobile` or `ie_mob` for Internet Explorer Mobile.

### 其他

* `Android` for Android WebView.
* `BlackBerry` or `bb` for Blackberry browser.
* `ChromeAndroid` or `and_chr` for Chrome for Android
  (in Other section, because mostly same as common `Chrome`).
* `FirefoxAndroid` or `and_ff` for Firefox for Android.
* `OperaMobile` or `op_mob` for Opera Mobile.
* `OperaMini` or `op_mini` for Opera Mini.
* `Samsung` for Samsung Internet.
* `UCAndroid` or `and_uc` for UC Browser for Android.
* `Electron` for Electron framework. It will be converted to Chrome version.

### Electron

[`electron-to-chromium`](https://www.npmjs.com/package/electron-to-chromium)
could return a compatible Browserslist query
for your (major) Electron version:

```js
const e2c = require('electron-to-chromium')
autoprefixer({
    browsers: e2c.electronToBrowserList('1.4') //=> "Chrome >= 53"
})
```

## 配置文件

Browserslist配置文件应该使用`browserslist`作为文件名并且有多个浏览器查询时，每行仅包含一条查询多条查询使用多行。注释使用`#`开关，例如：

```yaml
# Browsers that we support

> 1%
Last 2 versions
IE 8 # sorry
```

Browserslist将在`path`中的每一个目录中检测配置文件。
因此，如果工具处理`app/styles/main.css`路径，你就可以将配置文件放在根目录，`app/` 或 `app/styles`中。

你也能够在`BROWSERSLIST_CONFIG`环境变量中直接指定路径。

## `package.json`

你也可以在项目根目录中的`package.json`中，添加`browserslist`key，在`package.json`中进行配置，如下：

```js
{
  "private": true,
  "dependencies": {
    "autoprefixer": "^6.5.4"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions"
  ]
}
```

## Environments（多环境）

你也可以通过环境变量指定不同的浏览器查询。
Browserslist将`BROWSERSLIST_ENV` 或 `NODE_ENV`环境变量作为选择查询的依据。如果没有声明，Browserslist将首先寻找`development`key 的查询，然后使用默认值。

在 `package.json`:

```js
{
  …
  "browserslist": {
    "production": [
      "last 2 version",
      "ie 9"
    ],
    "development": [
      "last 1 version"
    ]
  }
}
```

在 `browserslist` config:

```ini
[production]
last 2 version
ie 9

[development]
last 1 version
```

## Environment Variables（环境变量）

如果一些工作在内部使用Browserslist，则可以通过[environment variables]（环境变量）来改变浏览器设置：

* `BROWSERSLIST` with browsers queries.

   ```sh
  BROWSERSLIST="> 5%" gulp css
   ```

* `BROWSERSLIST_CONFIG` with path to config file.

   ```sh
  BROWSERSLIST_CONFIG=./config/browserslist gulp css
   ```

* `BROWSERSLIST_ENV` with environments string.

   ```sh
  BROWSERSLIST_ENV="development" gulp css
   ```

* `BROWSERSLIST_STATS` with path to the custom usage data
  for `> 1% in my stats` query.

   ```sh
  BROWSERSLIST_STATS=./config/usage_data.json gulp css
   ```

[environment variables]: https://en.wikipedia.org/wiki/Environment_variable

## Custom Usage Data（自定义使用数据）

如果有自己的站点，那么则可以使用自己站点的统计数据，具体方法如下：

1. 导入你的Google分析数据到[Can I Use]。在设置页面点击`Import…`按钮。 
2. 在[Can I Use]页面打开浏览器开发者工具并粘贴如下代码到浏览器控制台：

    ```js
   var e=document.createElement('a');e.setAttribute('href', 'data:text/plain;charset=utf-8,'+encodeURIComponent(JSON.stringify(JSON.parse(localStorage['usage-data-by-id'])[localStorage['config-primary_usage']])));e.setAttribute('download','stats.json');document.body.appendChild(e);e.click();document.body.removeChild(e);
    ```
3. 将运行后的下载的数据保存到项目，并更改文件名为`browserslist-stats.json`。

当然，咱们也可以使用其他方法进行数据统计，具体的格式应该如下：

```js
{
  "ie": {
    "6": 0.01,
    "7": 0.4,
    "8": 1.5
  },
  "chrome": {
    …
  },
  …
}
```

注意，您可以查询您的自定义使用数据，同时也对全局或区域数据进行查询。例如查询`> 1% in my stats, > 5% in US, 10%`是可以的。

[Can I Use]: http://caniuse.com/

## JS API

```js
var browserslist = require('browserslist');

// Your CSS/JS build tool code
var process = function (source, opts) {
    var browsers = browserslist(opts.browsers, {
        stats: opts.stats,
        path:  opts.file,
        env:   opts.env
    });
    // Your code to add features for selected browsers
}
```

查询可以是字符串 `"> 5%, last 1 version"`
也可以是一个数组 `['> 5%', 'last 1 version']`.

如果查询失败，Browserslist将寻找一个配置文件。从提供的`path`参数（也可以是个文件）去找这个配置文件。

对于非js环境和调试的目的，可以使用CLI工具：

```sh
browserslist "> 1%, last 2 versions"
```

## Coverage（范围）

可以获取总的用户范围作为选择浏览器查询参数在JS API中使用：

```js
browserslist.coverage(browserslist('> 1%')) //=> 81.4
```

```js
browserslist.coverage(browserslist('> 1% in US'), 'US') //=> 83.1
```

或直接通过CLI:

```sh
$ browserslist --coverage "> 1%"
These browsers account for 81.4% of all users globally
```

```sh
$ browserslist --coverage=US "> 1% in US"
These browsers account for 83.1% of all users in the US
```
