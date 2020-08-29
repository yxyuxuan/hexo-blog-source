---
title: 关于husky > pre-commit问题
date: 2020-08-29 14:22:29
tags: Git
categories: Git
---

> 最近入职了新的公司，接手了一个新项目。git commit时遇到一个问题:  husky > pre-commit (node v12.13.0) Stashing changes...，检查后了解到是项目中安装了husky。

```json
$ git commit -m "修改内容"
husky > pre-commit (node v12.16.1)
Stashing changes... [started]
Stashing changes... [skipped]
→ No partially staged files found...
Running linters... [started]
Running tasks for **/*.less [started]
Running tasks for **/*.{js,jsx} [started]
Running tasks for **/*.{js,ts,tsx,json,jsx,less} [started]
stylelint --syntax less [started]
npm run lint-staged:js [started]
node ./scripts/lint-prettier.js [started]
node ./scripts/lint-prettier.js [completed]
git add [started]
git add [completed]
Running tasks for **/*.{js,ts,tsx,json,jsx,less} [completed]
stylelint --syntax less [failed]
→
Running tasks for **/*.less [failed]
→
npm run lint-staged:js [failed]
→
Running tasks for **/*.{js,jsx} [failed]
→
Running linters... [failed]
```

**解决方法：**

1. 执行npm run lint ，根据提示修改错误。

   ```
   npm run lint
   ```

2. 使用git commit -m "" --no-verify ，绕过lint的检查。

   ```
   git commit -m "" --no-verify
   ```

**git hooks**

如同其他许多的版本控制系统一样，Git 也具有在特定事件发生之前或之后执行特定脚本代码功能。

在git中提供了hook,就是在触发代码提交,push等一系列操作的时候,提供了触发其他程序的钩子.

`hooks`默认路径是 `.git/hooks`

**husky**

husky是一个npm包，安装后，可以很方便的在`package.json`配置`git hook` 脚本 。

```json
// package.json 

"scripts": {
    "lint": "eslint src"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint"
    }
  },
```

设置后，在每一次`git commit` 之前，都会执行一次对应的 hook 脚本`npm run lint` 。

在安装 `husky` 的时候，`husky`会根据 package.json里的配置，在`.git/hooks` 目录生成[所有](https://link.zhihu.com/?target=https%3A//git-scm.com/docs/githooks)的 hook 脚本（如果你已经自定义了一个hook脚本，`husky`不会覆盖它）。然后根据`package.json`里的设置，执行对应的hook脚本。

