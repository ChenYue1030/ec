# ec

# 工程化配置 本文讲解如何构建一个工程化的前端库，并结合 Github Actions，自动发布到 Github 和 NPM 的整个详细流程。

# 本文计划配置以下清单
Eslint
Prettier
Commitlint
Husky
Jest
GitHub Actions
Semantic Release

# 初始化
本文从创建一个 TypeScript 项目开始...

为了避免兼容性问题，建议使用 node 最新的长期支持版本（node v14.18.2）

查看已安装 node 版本：nvm list
使用 node v14.18.2 版本：nvm use v14.18.2

初始化项目：npm init -y

package.json 生成之后添加如下配置项:
+  "type": "module"

创建src文件夹，写入index.ts

# 配置
# TypeScript
安装 TypeScript：npm i typescript -D

使用 tsc 命名生成 tsconfig.json：npx tsc --init

添加修改 tsconfig.json 的配置项，如下：
{
  "compilerOptions": {
    /* Basic Options */
    "baseUrl": ".", // 模块解析根路径，默认为 tsconfig.json 位于的目录
    "rootDir": "src", // 编译解析根路径，默认为 tsconfig.json 位于的目录
    "target": "ESNEXT", // 指定输出 ECMAScript 版本，默认为 es5
    "module": "ESNext", // 指定输出模块规范，默认为 Commonjs
    "lib": ["ESNext", "DOM"], // 编译需要包含的 API，默认为 target 的默认值
    "outDir": "dist", // 编译输出文件夹路径，默认为源文件同级目录
    "sourceMap": true, // 启用 sourceMap，默认为 false
    "declaration": true, // 生成 .d.ts 类型文件，默认为 false
    "declarationDir": "dist/types", // .d.ts 类型文件的输出目录，默认为 outDir 目录
    /* Strict Type-Checking Options */
    "strict": true, // 启用所有严格的类型检查选项，默认为 true
    "esModuleInterop": true, // 通过为导入内容创建命名空间，实现 CommonJS 和 ES 模块之间的互操作性，默认为 true
    "skipLibCheck": true, // 跳过导入第三方 lib 声明文件的类型检查，默认为 true
    "forceConsistentCasingInFileNames": true, // 强制在文件名中使用一致的大小写，默认为 true
    "moduleResolution": "Node", // 指定使用哪种模块解析策略，默认为 Classic
  },
  "include": ["src"] // 指定需要编译文件，默认当前目录下除了 exclude 之外的所有.ts, .d.ts,.tsx 文件
}

将编译后的文件路径添加到 package.json，并在 scripts 中添加编译命令。
+  "main": "dist/index.js",
+  "types": "dist/types/index.d.ts"

+   "scripts": {
+     "dev": "tsc --watch",
+     "build": "npm run clean && tsc",
+     "clean": "rm -rf dist"
+  },
types 配置项是指定编译生成的类型文件，如果 compilerOptions.declarationDir 指定的是dist，也就是源码和 .d.ts 同级，那么types可以省略。

在 index.ts 验证配置是否生效
const calc = (a: number, b: number) => {
  return a - b
}
console.log(calc(1024, 28))

在控制台中执行：npm run build && node dist/index.js
在 dist 目录中会生成 types/index.d.ts、index.js、index.js.map，并打印 996。

# Eslint & Prettier
安装 eslint：npm i eslint -D

eslint 的命令行工具生成基本配置：npx eslint --init

依次选择符合我们项目的配置 @eslint/create-config，以下是本项目配置：
Ok to proceed? (y) y
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · none
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser, node
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · standard-with-typescript
✔ What format do you want your config file to be in? · JavaScript
Checking peerDependencies of eslint-config-standard-with-typescript@latest
The config that you've selected requires the following dependencies:

eslint-config-standard-with-typescript@latest @typescript-eslint/eslint-plugin@^5.0.0 eslint@^8.0.1 eslint-plugin-import@^2.25.2 eslint-plugin-n@^15.0.0 eslint-plugin-promise@^6.0.0 typescript@*
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · npm
Installing eslint-config-standard-with-typescript@latest, @typescript-eslint/eslint-plugin@^5.0.0, eslint@^8.0.1, eslint-plugin-import@^2.25.2, eslint-plugin-n@^15.0.0, eslint-plugin-promise@^6.0.0, typescript@*

最后，生成好的文件为 .eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: 'standard-with-typescript',
  overrides: [
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module'
  },
  rules: {
  }
}
