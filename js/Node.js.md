## fs
### fs.realpathSync(path[, options])
返回解析的路径

## process
process.cwd()方法返回 Node.js 进程当前工作的目录。
```
console.log(process.cwd()) //F:\test\mangopayH5-test\src
```

## path
path.resolve() 方法会把一个路径或路径片段的序列解析为一个绝对路径。
```
const appDirectory = fs.realpathSync(process.cwd()); //F:\test\mangopayH5-test

const resolveApp = relativePath => path.resolve(appDirectory, relativePath);

console.log(resolveApp('../build')) //F:\test\build
console.log(resolveApp('./build')) //F:\test\mangopayH5-test\build

```

## Module
require.resolve(request[, options])
```
request <string> 需要解析的模块路径。
options <Object>
paths <Array> 解析模块的起点路径。此参数存在时，将使用这些路径而非默认解析路径。 注意此数组中的每一个路径都被用作模块解析算法的起点，意味着 node_modules 层级将从这里开始查询。
Returns: <string>
```

用内部的 require() 机制查询模块的位置, 此操作只返回解析后的文件名，不会加载该模块。

## url
url.parse(urlString[, parseQueryString[, slashesDenoteHost]])

url.parse() 方法会解析一个 URL 字符串并返回一个 URL 对象