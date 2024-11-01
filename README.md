# nodejs-esm-ts

nodejs+esm+ts

# 流程

## express生成器初始化nodejs项目

```bash
npx express-generator
```

## esm 改造

1. package.json 的 type 设置为 module
2. cjs语法改esm: require 改 import 和 module.exports 改 export default
3. 引用文件需要完整的文件扩展名
4. 在 es 模块中，__dirname 和 __filename 这两个全局变量并不存在

package.json

```diff
{
+ "type": "module",
}
```

app.js

```diff
+ import { fileURLToPath } from 'url';

+ const __filename = fileURLToPath(import.meta.url);
+ const __dirname = path.dirname(__filename);
```

<details>
<summary>ESM改造其他问题</summary>

ESM 需要明确的文件扩展名，不能省略

```plaintext
Error [ERR_MODULE_NOT_FOUND]: Cannot find module XXX imported from XXX
```

在 ES 模块中，__dirname 和 __filename 这两个全局变量并不存在

```plaintext
ReferenceError: __dirname is not defined in ES module scope
```

</details>

## ts 改造

安装 typescript、ts-node、声明文件

```bash
npm i -D typescript ts-node @types/node @types/debug @types/express @types/cookie-parser @types/morgan
```

初始化ts配置文件

```bash
npx tsc --init
```

tsconfig.json 修改配置文件
```diff
{
  "compilerOptions": {
-    "module": "commonjs",
+    "module": "ESNext",
  }
}
```
package.json 修改配置文件
```diff
{
  "scripts": {
-   "start": "node ./bin/www"
+   "start": "node --loader ts-node/esm ./bin/www.ts"
  },
}
```

把文件扩展类型js文件修改ts，并解决ts语法错误

<details>
<summary>ts改造 过程中的问题</summary>

Q:
```plaintext
TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".ts" for xxx
```
A:运行命令改用ts-node

Q:
```plaintext
node:internal/process/esm_loader:34
      internalBinding('errors').triggerUncaughtException(
                                ^
[Object: null prototype] {
  [Symbol(nodejs.util.inspect.custom)]: [Function: [nodejs.util.inspect.custom]]
}
```
A:文件扩展类型js文件修改ts后，需要解决ts语法错误

Q:
```plaintext
error TS2307: Cannot find module 'http' or its corresponding type declarations.
```
A:需要额外安装声明文件

</details>

## 运行本地项目

安装依赖

```bash
npm install
```

运行

```bash
npm run start
```
