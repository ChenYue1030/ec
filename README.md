# ec

# 工程化配置
本文讲解如何构建一个工程化的前端库，并结合 Github Actions，自动发布到 Github 和 NPM 的整个详细流程。

# 本文计划配置以下清单
Eslint
Prettier
Commitlint
Husky
Jest
GitHub Actions
Semantic Release

# 初始化
创建一个 TypeScript 项目开始...

为了避免兼容性问题，建议使用 node 最新的长期支持版本（node v14.18.2）

查看已安装 node 版本：nvm list
使用 node v14.18.2 版本：nvm use v14.18.2
