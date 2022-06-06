# jira

## 一.基础配置

### （一）创建项目

npx create-react-app csdn-admin --template typescript

要求：

1.node版本非14以上

2.非全局安装create-react-app

### （二）配置 ESLint、 prettier 和 commitlint 规范工程

这一步非常复杂，请按照步骤一步步操作。

#### 1.配置eslint

eslint用于代码质量的检查，它的目标是保证代码的一致性和避免错误。

（1）默认创建的React项目自带ESLint配置，先删除掉，我们将配置更好的选项。打开package.json文件，删除下面的代码

```javascript
"eslintConfig": {
   "extends":[
      "react-app",
      "react-app/jest"
   ]
}
```

（2）安装 ESLint package

在项目根目录，下面两行代码二选一运行

```javascript
//yarn安装法
yarn add eslint -D
//npm安装法
npm install eslint --save-dev
```

安装完成后，可以在`package.json`文件中看到：

```js
"devDependencies": {
      "eslint": "^8.17.0"
}
```

版本号不一致无碍。

（3）配置eslint

在项目根目录，运行：

```js
npx eslint --init
```

运行此命令时，您需要回答有关配置的一些问题：

```js
How would you like to use ESLint?
选择： To check syntax, find problems, and enforce code style
What type of modules does your project use?
选择：JavaScript modules (import/export)
Which framework does your project use?
选择：React
Does your project use TypeScript?
选择：Yes
Where does your code run?
选择：Browser
How would you like to define a style for your project?
选择：Use a popular style guide
Which style guide do you want to follow?
选择：Airbnb: https://github.com/airbnb/javascript
What format do you want your config file to be in?
选择：JSON
```

之后，它将检查需要安装的依赖项，然后询问：

```js
Would you like to install them now with npm?
选择： Yes
```

检查package.json

完成上述步骤后，检查package.json文件是否存在一下属性

```js
"devDependencies": {
        "@typescript-eslint/eslint-plugin": "^4.26.1",
        "@typescript-eslint/parser": "^4.26.1",
        "eslint": "^7.28.0",
        "eslint-config-airbnb": "^18.2.1",
        "eslint-plugin-import": "^2.23.4",
        "eslint-plugin-jsx-a11y": "^6.4.1",
        "eslint-plugin-react": "^7.24.0",
        "eslint-plugin-react-hooks": "^4.2.0"
}
```

版本号不一致无碍。

（4）运行命令

运行以下命令后，会出现大量错误

```js
npx eslint src/* 
```

修复一下，仍有大量错误

```js
npx eslint src/* --fix
```

（5）修复错误

安装`eslint-import-resolver-typescript`，以下命令二选一

```js
//yarn安装法
yarn add eslint-import-resolver-typescript -D
//npm安装法
npm install eslint-import-resolver-typescript --save-dev
```

打开eslintrc.json文件,在rules节点内增加：

```js
"rules": {
        "no-use-before-define": "off",
        "@typescript-eslint/no-use-before-define": ["error"],
        "react/jsx-filename-extension": ["warn", { "extensions": [".tsx"] }],
        "import/extensions": [
            "error",
            "ignorePackages",
            {
                "ts": "never",
                "tsx": "never"
            }
        ],
        "no-shadow": "off",
        "@typescript-eslint/no-shadow": ["error"],        
        "max-len": ["warn", { "code": 80 }],
        "react-hooks/rules-of-hooks": "error",
        "react-hooks/exhaustive-deps": "warn",
        "import/prefer-default-export": "off",
        "react/prop-types": "off"
}
```

在同文件增加settings节点

```js
"settings": {
        "import/resolver": {
            "typescript": {}
        }
}
```

在extends节点中增加：

```js
"extends": [
        "plugin:react/recommended",
        "airbnb",
        "plugin:@typescript-eslint/recommended"
    ]
```

在plugins节点增加

```js
"plugins": ["react", "@typescript-eslint", "react-hooks"]
```

- 在项目根目录增加.eslintignore文件，内容如下：

```js
*.css
*.svg
```

（6）在vscode中，按照eslint插件

![img](https://img-blog.csdnimg.cn/20210612165822773.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0Lzg2NjUwNDg=,size_16,color_FFFFFF,t_70#pic_center)

在项目根目录下创建.vscode目录
在.vscode目录内创建文件settings.json，内容如下:

```js
{
    "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.formatOnSave": true,
    "eslint.alwaysShowStatus": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

接着，重启一下VSCode使之生效，打开src/App.tsx文件，我们可以看到VSCode右下角ESLint已生效了，如出现未生效的图标，点击它，在弹出窗口上选择`Allow`.
![ESLint](https://img-blog.csdnimg.cn/20210612170558415.jpg#pic_center)

好了，这部分就完成了。

#### 2.按照配置Prettier

（1）安装prettier

在项目根目录，下面两行代码二选一运行

```javascript
//yarn安装法
yarn add prettier -D
//npm安装法
npm install --save-dev prettier
```

（2）配置.prettierrc.json

在项目根目录下新建.prettierrc.json文件，内容如下：

```js
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": false,
    "singleQuote": true
}
```

（3）配置.prettierignore

在项目根目录新建.prettierignore文件，内容如下：

```js
node_modules
# Ignore artifacts:
build
coverage
```

（4）执行Prettier命令

可以看到它已经可以格式化代码了。但执行完毕后，文件的格式就不符合eslint的规范了，后续需要解决冲突。

```js
npx prettier --write src/App.tsx
```

接着，在package.json文件中的scripts节点中增加：

```js
"format": "prettier --write \"**/*.{ts,tsx,js,json,css,scss}\""
```

这样，就可以执行如下命令，对整个项目进行代码格式化

```js
//yarn版本
yarn format
//npm版本
npm run format
```

（5）vscode中安装prettier插件

安装Prettier VSCode extension

![img](https://img-blog.csdnimg.cn/20210612175857763.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0Lzg2NjUwNDg=,size_16,color_FFFFFF,t_70#pic_center)

修改.vscode目录中的settings.json文件，如下：

```js
{
    "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.formatOnSave": true,
    "eslint.alwaysShowStatus": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "[json]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "editor.formatOnSave": true
    }
}
```

#### 3.解决eslint和prettier的冲突

（1）安装eslint-config-prettier

为了解决eslint和prettier的冲突，安装eslint-config-prettier包。在项目根目录下，以下代码二选一

```javascript
//yarn安装法
yarn add  eslint-config-prettier -D
//npm安装法
npm install --save-dev eslint-config-prettier
```

（2）修改.eslintrc.json文件

在extends节点最后增加prettier：

```javascript
"extends": [
        "plugin:react/recommended",
        "airbnb",
        "plugin:@typescript-eslint/recommended",
        "prettier"
]
```

检查以下冲突是否解决，运行以下两个命令检查结果是否一致

```javascript
npx eslint src/App.tsx --quiet --fix
npx prettier --write src/App.tsx
```

（3）让 ESLint 使用 Prettier 的规则

安装 eslint-plugin-prettier，在项目根目录下，以下代码二选一

```javascript
//yarn安装法
yarn add eslint-plugin-prettier -D
//npm安装法
npm install --save-dev eslint-plugin-prettier
```

修改.eslintrc.json文件

在extends节点最后增加plugin:prettier/recommended：

```javascript
"extends": [
        "plugin:react/recommended",
        "airbnb",
        "plugin:@typescript-eslint/recommended",
        "prettier",
        "plugin:prettier/recommended"
]
```

运行以下代码，检查以下：

```javascript
npx eslint src/App.tsx --quiet --fix
```

做完这些步骤，务必先commit+push到git仓库上！！！！！如果是个新仓库，需要与远程地址链接一下

```javascript
git remote add origin https://github.com/xxxsod1/jira.git
```

3.安装配置commitlint

（1）安装commitlint

```javascript
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

（2）配置commitlint

```javascript
echo module.exports = {extends: ['@commitlint/config-conventional']} > commitlint.config.js
```

（3）安装和配置husky

执行如下命令:

```sh
# 安装 Husky v6
npm install husky --save-dev
# 或者
yarn add husky --dev

# 激活 hooks
npx husky install
# 或者
yarn husky install

# 添加 hook
npx husky add .husky/commit-msg 
```

接着打开husky文件夹下的commit-msg文件，将undefined字符串用npx --no-install commitlint --edit "$1"替换掉。

此时，在commit时提交的消息必须遵从的规范如下（注意冒号后有一个空格）：

```sh
# 主要type
feat: 增加新功能
fix: 修复bug

# 特殊type
docs: 只改动了文档相关的内容
style: 不影响代码含义的改动，例如去掉空格、改变缩进、增删分号
build: 构造工具的或者外部依赖的改动，例如webpack，npm
refactor: 代码重构时使用
revert: 执行git revert打印的message

# 暂不使用type
test: 添加测试或者修改现有测试
perf: 提高性能的改动
ci: 与CI（持续集成服务）有关的改动
chore: 不修改src或者test的其余修改，例如构建过程或辅助工具的变动
```

