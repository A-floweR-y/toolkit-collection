# toolkit-collection
npm 工具包集合，方便大家工作和查找。

## 目录
- [*Server Tools*](#server-tools)
- [*File Tools*](#file-tools)
- [*Command Line*](#command-line)
- [*Promise*](#promise)
- [*Browser*](#browser)

## Server Tools
- [portfinder](https://www.npmjs.com/package/portfinder) 自由端口查找器。当你所编写的服务需要一个端口，并且，你不确定哪些端口可以使用时。这个包可以找到目前未被占用的端口。

  ```js
  const portfinder = require('portfinder');
 
  portfinder.getPortPromise()
    .then((port) => // ... 可以使用的端口)
    .catch((e) => // ... 出问题了);
  ```

## File Tools
- [minimatch](https://www.npmjs.com/package/minimatch) 使用 glob 表达式的文件匹配工具。如果你想用 glob 表达式去匹配一下文件用它就 OK 了。

  ```js
  const minimatch = require('minimatch');

  minimatch('bar.foo', '*.foo'); // true!
  minimatch('bar.foo', '*.bar'); // false!
  minimatch('bar.foo', '*.+(bar|foo)', { debug: true }); // true
  ```
  
- [glob](https://www.npmjs.com/package/glob) 基于 [minimatch](https://www.npmjs.com/package/minimatch) 的 glob 表达式文件查找工具。

  ```js
  const glob = require('glob');

  // options 是个可选项
  glob('**/*.js', options, function (err, files) {
    // files 是一个文件数组
    // err 是一个文件对象
  })
  ```

## Command Line
- [commander](https://www.npmjs.com/package/commander) 命令行参数解析工具。支持定义参数规则，还有中文文档

  ```js
  // index.js
  const { program } = require('commander');

  program
    .option('--first')
    .option('-s, --separator <char>');

  program.parse();

  const options = program.opts();
  const limit = options.first ? 1 : undefined;
  ```
  ```bash
  # output
  $ node split.js -s / --fits a/b/c
  error: unknown option '--fits'
  (Did you mean --first?)
  $ node split.js -s / --first a/b/c
  [ 'a' ]
  ```
  
## Promise
- [promise-limit](https://www.npmjs.com/package/promise-limit) 限制 Promise 的并发数量，一般在 `Promise.all` 发起大量 Promise 时使用.

  ```js
  const promiseLimit = require('promise-limit');
  const limit = promiseLimit(2);
  const jobs = ['a', 'b', 'c', 'd', 'e'];

  Promise.all(jobs.map((name) => {
    return limit(() => job(name));
  })).then(results => {
    console.log();
    console.log('results:', results);
  })

  function job (name) {
    const text = `job ${name}`;
    console.log('started', text);

    return new Promise(function (resolve) {
      setTimeout(() => {
        console.log('       ', text, 'finished');
        resolve(text);
      }, 100);
    });
  }
  ```bash
  # output
  started job a
  started job b
          job a finished
          job b finished
  started job c
  started job d
          job c finished
          job d finished
  started job e
          job e finished

  results: [ 'job a', 'job b', 'job c', 'job d', 'job e' ]
  ```

## Browser
- [ua-parser-js](https://www.npmjs.com/package/ua-parser-js) `navigator.userAgent` 解析器，能提供详细的数据：浏览器名称、系统名称、设备型号/类型等等。并且可以针对自定义的 `navigator.userAgent` 进行扩展。

  ```js
  import UAParser from 'ua-parser-js';
  
  const uaInfo = UAParser;
  // { ua: '', browser: {}, cpu: {}, device: {}, engine: {}, os: {} }
  ```
