# toolkit-collection
npm 工具包集合，方便大家工作和查找。

## 目录
- [*Server Tools*](#server-tools) 服务相关工具
- [*File Tools*](#file-tools) 文件处理相关工具
- [*Command Line Tools*](#command-line-tools) 命令行相关工具
- [*Promise Tools*](#promise-tools) Promise 相关工具
- [*Browser Tools*](#browser-tools) 浏览器相关工具
- [*Counter Tools*](#counter-tools) 计算相关
- [*Builder Tools*](#builder-tools) 构建相关
- [*Javascript Tools*](#javascript-tools) Javascript 工具（仅限于语言，没有明确的使用场景）
- [*Canvas Tools*](#canvas-tools) Canvas 工具
- [*AI Tools*](#ai-tools) AI 工具
- [*React Ecology*](#react-ecology) React 生态
- [*Vue Ecology*](#vue-ecology) Vue 生态
- [*Cross platform Framework*](#cross-platform-framework) 跨端框架
- [*Other*](#other) 跟前端无关的工具

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

- [uuid](https://www.npmjs.com/package/uuid) uuid 生成器

  ```js
  import { v4 as uuidv4 } from 'uuid';
  uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'
  ```

- [pm2](https://www.npmjs.com/package/pm2)  Node.js 应用程序的生产流程管理器，具有内置负载均衡器。它允许您使应用程序永远保持活动状态，无需停机即可重新加载它们，并简化常见的系统管理任务。

  ```js
  pm2 start app.js
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

- [execa](https://www.npmjs.com/package/execa) execa 是对 child_process 的封装，提供了更友好的方式操作子进程。

  ```js
  import {execa} from 'execa';

  const {stdout} = await execa('echo', ['unicorns']);
  console.log(stdout);
  //=> 'unicorns'
  ```

- [ora](https://www.npmjs.com/package/ora) ora 是一个命令行 loading 执行器，它内部使用了 [cli-spinners](https://www.npmjs.com/package/cli-spinners) 提供的丰富 loading 素材，通过 [cli-cursor](https://www.npmjs.com/package/cli-cursor) 来实现是否隐藏光标的功能。

  ```js
  import ora from 'ora';

  const spinner = ora('Loading unicorns').start();
  
  setTimeout(() => {
  	spinner.color = 'yellow';
  	spinner.text = 'Loading rainbows';
  }, 1000);
  ```
  demo:<br>
  ![ora demo](https://github.com/sindresorhus/ora/blob/HEAD/screenshot.svg)

  cli-spinners 内置效果展示:<br>
  ![cli-spinners demo](https://github.com/sindresorhus/cli-spinners/blob/main/screenshot.svg)


- [listr2](https://www.npmjs.com/package/listr2) 这是一个任务列表的形式来展示loading的工具，如果需要列表（同步/异步）来展示 loading，可以用这个包。
  ```ts
  import { Listr } from 'listr2'
  
  interface Ctx {
    /* some variables for internal use */
  }
  
  const tasks = new Listr<Ctx>(
    [
      /* tasks */
    ],
    {
      /* options */
    }
  )
  ```
  demo:<br>
  ![Listr2 Demo](https://media.githubusercontent.com/media/listr2/listr2/master/examples/renderer-default.gif)

- [clipboardy](https://www.npmjs.com/package/clipboardy) nodejs 读写 剪切版 功能。支持文本+图片
  ```ts
  import clipboard from 'clipboardy';

  await clipboard.write('🦄');
  
  await clipboard.read();
  //=> '🦄'
  
  // Or use the synchronous API
  clipboard.writeSync('🦄');
  
  clipboard.readSync();
  //=> '🦄'
  ```


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

- [stagehand](https://www.stagehand.dev/) 操作无头浏览器+AI加持，对比 [Puppeteer](https://puppeteer.bootcss.com/api)，支持 Chrome + Fixfox。也是 [Puppeteer](https://puppeteer.bootcss.com/api) 的开发团队开发的。如果需要做自动化测试，也可以参考一下 [playwright](https://playwright.dev/)

  ```js
  page.goto("browserstore.com/cookies");
  ```

- [capo.js](https://rviscomi.github.io/capo.js/) 检测网页 header 资源的加载循序进行优化。有 Chrome 插件。

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

## Builder Tools
- [acorn](https://www.npmjs.com/package/acorn) 把 string javascript 代码解析成 AST 语法树。

  ```js
  let acorn = require("acorn");
  console.log(acorn.parse("1 + 1", {ecmaVersion: 2020}));
  // output
  // {
  //   "type": "Program",
  //  "start": 0,
  //  "end": 5,
  //  "body": [
  //    {
  //      "type": "ExpressionStatement",
  //      "start": 0,
  //      "end": 5,
  //      "expression": {
  //        "type": "BinaryExpression",
  //        "start": 0,
  //        "end": 5,
  //        "left": {
  //          "type": "Literal",
  //          "start": 0,
  //          "end": 1,
  //          "value": 1,
  //          "raw": "1"
  //        },
  //        "operator": "+",
  //        "right": {
  //          "type": "Literal",
  //          "start": 4,
  //          "end": 5,
  //          "value": 1,
  //          "raw": "1"
  //        }
  //      }
  //    }
  //  ],
  //  "sourceType": "module"
  //}
  ```
  rollup 和 webpack 内部都在使用 `acorn` 做 AST 语法树的解析。同样类似功能的还有：[recast](https://www.npmjs.com/package/recast)，它提供了多种解释器，非常的强大。
  当然，你也可以选择我们最为熟悉的 `@babel/paser`，它最初也是对 `acorn` 的一个 fork。
  这里有一篇关于各种 js 解释器的介绍，可以了解一下：[JavaScript 各种解析器的介绍](https://segmentfault.com/a/1190000038645422);

  很多时候，我们的多数工作仅仅只是对源代码进行轻微的修改，[recast](https://www.npmjs.com/package/recast) 这样的库对我们来说完全没有必要。这里另外推荐一个。
  [magic-string](https://www.npmjs.com/package/magic-string): 它能让你操作代码字符串和生成源映射。

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

- [AnimeJS](https://animejs.com/) 一个特别优秀的 JS 动画库，非常值得关注。

- [Slidev](https://cn.sli.dev/) 用 Markdown 来编写 PPT，网站也提供了一下精美的案例

- [Lucide](https://lucide.dev/) 图标 Icon 组件，能用于任何框架。Icon数量有上千个，且全部免费。

- [shadcn/ui](https://ui.shadcn.com/) 目前最火的 UI 组件库了，完全开源，它是直接发送源代码的形式，让你可以自定义自己的 UI 组件。好多 UI 组件库都基于 [shadcn/ui](https://ui.shadcn.com/) 来做的二次开发。如果需要整体的主体色，这里有个工具 [tweakcn](https://tweakcn.com/)，可以可视化的调整主体色，然后再一键导出。


## Canvas Tools
- [snapdom](https://www.npmjs.com/package/@zumer/snapdom) 比 html2canvas 更优秀的网页截屏包

  ```js
  snapdom.toPng(document.body).then(img => {
    document.body.appendChild(img);
  });
  ```

- [Rough.js](https://www.npmjs.com/package/roughjs) 手绘风格的图形

  ```js
  const rc = rough.canvas(document.getElementById('canvas'));
  rc.rectangle(10, 10, 200, 200); // x, y, width, height
  ```
  ![Rough.js Demo](https://camo.githubusercontent.com/c83edb407f007aa00118a596bb98d2fd30484be34b9fc4a2c55a98b4da528291/68747470733a2f2f726f7567686a732e636f6d2f696d616765732f6361705f64656d6f2e706e67)

- [canvas confetti](https://www.npmjs.com/package/canvas-confetti) 彩带效果。[demo 网站](https://www.kirilv.com/canvas-confetti/).

- [fireworks.js](https://fireworks.js.org/) 烟花效果，官网给了配置调整功能，可实时预览效果


## AI Tools
- [Flowise](https://flowiseai.com/) 使用工具流的方式构建一个 AI Agent

- [AI.SDK](https://ai-sdk.dev/) 抹平各个 AI 大语言模型的 API 差异，直接可以介入业务逻辑的开发。

- [Chat SDK](https://chat-sdk.dev/) 就是聊天机器人搭建。基于 Next.js


## React Ecology
- [React Bits](https://reactbits.dev/get-started/index) React 视觉工具库，非常多的效果。

- [Zustand](https://zustand.docs.pmnd.rs/) React 状态管理器，相比于 Redux，更加的简单。对于小型项目非常合适。
demo:<br>
```js
import { create } from 'zustand'

const useBear = create((set) => ({
  bears: 0,
  increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
  removeAllBears: () => set({ bears: 0 }),
  updateBears: (newBears) => set({ bears: newBears }),
}))
```

- [React Flow](https://reactflow.dev/) 专门提供 Flow 这种形式的开发框架，具体你用 Flow 干什么它不关系，它只提供 Flow 的能力

## Vue Ecology
- [Vue Bits](https://vue-bits.dev/text-animations/split-text) Vue 视觉工具库，非常多的效果。

## Cross platform Framework
- [Lynx](https://lynxjs.org/zh/) IOS、安卓、鸿蒙和WEB 跨端框架，使用原生组件，编译成原生代码。有中文文档，字节跳动开源。

- [Expo](https://expo.dev/) 基于 RN 做了开发环境的集成，用户直接通过 ts + react 开发就行，不需要配置开发环境（因为 RN 的开发环境配置需要安卓等原生知识）。

- [INK](https://github.com/vadimdemedes/ink) 用 React 语法构建命令行 CLI 界面。最终打包产物是一个可运行文件，它运行环境还是 nodejs。

## Other
- [BentoPDF](https://www.bentopdf.com/) 一个免费的 PDF 工具集网站
