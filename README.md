# toolkit-collection
npm 工具包集合，方便大家工作和查找。

## 目录
- [*Server Tools*](#server-tools) 服务相关工具
- [*File Tools*](#file-tools) 文件处理相关工具
- [*Command Line Tools*](#command-line-tools) 命令行相关工具
- [*Promise Tools*](#promise-tools) Promise 相关工具
- [*Browser Tools*](#browser-tools) 浏览器相关工具
- [*Counter Tools*](#counter-tools) 计算相关
- [*Javascript Tools*](#javascript-tools) Javascript 工具（仅限于语言，没有明确的使用场景）

## Server Tools
- [portfinder](https://www.npmjs.com/package/portfinder) 自由端口查找器。当你所编写的服务需要一个端口，并且，你不确定哪些端口可以使用时。这个包可以找到目前未被占用的端口。

  ```js
  const portfinder = require('portfinder');
 
  portfinder.getPortPromise()
    .then((port) => // ... 可以使用的端口)
    .catch((e) => // ... 出问题了);
  ```
 
- [node-schedule](https://www.npmjs.com/package/node-schedule) NodeJS 定时任务工具。

  ```js
  const schedule = require('node-schedule');

  const job = schedule.scheduleJob('42 * * * *', function(){
    console.log('The answer to life, the universe, and everything!');
  });
  ```
  ```
  *    *    *    *    *    *
  ┬    ┬    ┬    ┬    ┬    ┬
  │    │    │    │    │    │
  │    │    │    │    │    └ day of week (0 - 7) (0 or 7 is Sun)
  │    │    │    │    └───── month (1 - 12)
  │    │    │    └────────── day of month (1 - 31)
  │    │    └─────────────── hour (0 - 23)
  │    └──────────────────── minute (0 - 59)
  └───────────────────────── second (0 - 59, OPTIONAL)
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

## Command Line Tools
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

- [minimist](https://www.npmjs.com/package/minimist) 轻量级命令行参数解析器

  ```js
  const argv = require('minimist')(process.argv.slice(2));
  console.log(argv);
  ```

  ```base
  $ node example/parse.js -x 3 -y 4 -n5 -abc --beep=boop foo bar baz
  {
    _: [ 'foo', 'bar', 'baz' ],
    x: 3,
    y: 4,
    n: 5,
    a: true,
    b: true,
    c: true,
    beep: 'boop'
  }
  ```
  
- [prompts](https://www.npmjs.com/package/prompts) 命令行交互工具

  ```js
  const prompts = require('prompts');

  (async () => {
    const response = await prompts({
      type: 'number',
      name: 'value',
      message: 'How old are you?',
      validate: value => value < 18 ? `Nightclub is 18+ only` : true
    });

    console.log(response); // => { value: 24 }
  })();
  ```
  demo:
  ![prompts demo](https://github.com/terkelg/prompts/raw/master/media/example.gif)
  
  相同功能的工具还有 [inquirer](https://www.npmjs.com/package/inquirer)（下载量最多，**推荐！！**）、[enquirer](https://www.npmjs.com/package/enquirer)（跟 inquirer 类似）
  
- [chalk](https://www.npmjs.com/package/chalk) 命令行输入彩色文本

  ```js
  import chalk from 'chalk';

  console.log(chalk.blue('Hello world!'));
  ```

- [semver](https://www.npmjs.com/package/semver) semver, 是一个语义化版本号管理的模块，可以实现版本号的解析和比较，规范版本号的格式。

  ```js
  const semver = require('semver')

  semver.valid('1.2.3') // '1.2.3'
  semver.valid('a.b.c') // null
  semver.clean('  =v1.2.3   ') // '1.2.3'
  semver.satisfies('1.2.3', '1.x || >=2.5.0 || 5.0.0 - 7.2.3') // true
  semver.gt('1.2.3', '9.8.7') // false
  semver.lt('1.2.3', '9.8.7') // true
  semver.minVersion('>=1.0.0') // '1.0.0'
  semver.valid(semver.coerce('v2')) // '2.0.0'
  semver.valid(semver.coerce('42.6.7.9.3-alpha')) // '42.6.7'
  ```
  了解关于版本号的更多内容，可以[点击这里](./desc/version.md)

## Promise Tools
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
  ```

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

## Browser Tools
- [ua-parser-js](https://www.npmjs.com/package/ua-parser-js) `navigator.userAgent` 解析器，能提供详细的数据：浏览器名称、系统名称、设备型号/类型等等。并且可以针对自定义的 `navigator.userAgent` 进行扩展。

  ```js
  import UAParser from 'ua-parser-js';
  
  const uaInfo = UAParser;
  // { ua: '', browser: {}, cpu: {}, device: {}, engine: {}, os: {} }
  ```
- [web-vitals](https://www.npmjs.com/package/web-vitals) 用于测量真实用户测的性能指标：[CLS](https://web.dev/cls/)、[FID](https://web.dev/fid/)、[LCP](https://web.dev/lcp/)、[FCP](https://web.dev/fcp/) 和 [TTFB](https://web.dev/ttfb/)。

  ```js
  import {getLCP, getFID, getCLS} from 'web-vitals';

  getCLS(console.log);
  getFID(console.log);
  getLCP(console.log);
  ```

## Counter Tools
- [bytes](https://www.npmjs.com/package/bytes) 将字符串解析为字节，或者字节解析为字符串。

  ```js
  const bytes = require('bytes');
  
  bytes(1024);
  // output: '1KB'
  
  bytes('1KB');
  // output: 1024
  ```
  
  bytes 的参数如果是数字，就会返回字符串的单位格式。如果是字符串，就会返回数字格式的字节数。如果我们希望**不管是数字还是字符串，都返回字节数**。那么可以用 `bytes.parse()`。如果你为了方便，也可以使用 [humanize-bytes](https://www.npmjs.com/package/humanize-bytes), 它只是对 `bytes()` 做了一个小小的包装。
  
- [crypto-js](https://www.npmjs.com/package/crypto-js) js 加密算法标准库，几乎涵盖了工作用到的加密算法。支持 Nodejs 和 浏览器中运行。

  ```js
  import sha256 from 'crypto-js/sha256';
  import hmacSHA512 from 'crypto-js/hmac-sha512';
  import Base64 from 'crypto-js/enc-base64';

  const message, nonce, path, privateKey; // ...
  const hashDigest = sha256(nonce + message);
  const hmacDigest = Base64.stringify(hmacSHA512(path + hashDigest, privateKey));
  ```

## Javascript Tools
- [deepmerge](https://www.npmjs.com/package/deepmerge) js 对象深度合并。支持自定义合并策略。

  ```js
  const x = {
    foo: { bar: 3 },
    array: [{
        does: 'work',
        too: [ 1, 2, 3 ]
    }]
  }

  const y = {
      foo: { baz: 4 },
      quux: 5,
      array: [{
          does: 'work',
          too: [ 4, 5, 6 ]
      }, {
          really: 'yes'
      }]
  }

  const output = {
      foo: {
          bar: 3,
          baz: 4
      },
      array: [{
          does: 'work',
          too: [ 1, 2, 3 ]
      }, {
          does: 'work',
          too: [ 4, 5, 6 ]
      }, {
          really: 'yes'
      }],
      quux: 5
  }

  merge(x, y) // => output
  ```
