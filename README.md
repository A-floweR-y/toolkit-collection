# toolkit-collection
npm 工具包集合，方便大家工作和查找。

## 目录
- [*Server Tools*](#server-tools)
- [*File Tools*](#file-tools)

## Server Tools
- [portfinder](https://www.npmjs.com/package/portfinder) 自由端口查找器。当你所编写的服务需要一个端口，并且，你不确定哪些端口可以使用时。这个包可以找到目前未被占用的端口。
  ```js
  const portfinder = require('portfinder');
 
  portfinder.getPortPromise()
    .then((port) => // ... 可以使用的端口)
    .catch((e) => // ... 出问题了);
  ```

## File Tools
- [minimatch]() 使用 glob 表达式的文件匹配工具。如果你想用 glob 表达式去匹配一下文件用它就 OK 了。
  ```js
  const minimatch = require("minimatch");

  minimatch("bar.foo", "*.foo"); // true!
  minimatch("bar.foo", "*.bar"); // false!
  minimatch("bar.foo", "*.+(bar|foo)", { debug: true }); // true
  ```
