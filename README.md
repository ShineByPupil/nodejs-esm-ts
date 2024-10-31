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

## 运行本地项目

安装依赖

```bash
npm install
```

运行

```bash
npm run start
```
