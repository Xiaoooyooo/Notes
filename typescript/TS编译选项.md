---
title: TypeScript编译选项
date: 2021-03-13 17:11:14
tags:
	- TypeScript
	- JavaScript
categories:
	- TypeScript
---

# TypeScript编译选项

+ `tsc`：编译当前目录下所有ts文件，需要配置文件（tsconfig.json）
+ `tsc [filename]`：编译指定文件
+ `tsc -w`：自动监视

## tsconfig.json

### 配置说明

+ `config.include`：定义希望被编译的文件所在目录

  + ```json
    [
        /* 
         * **表示任意目录，
         * *表示任意文件 
         */
        "./src/**/*"
    ]
    ```

+ `config.exclude`：排除不需要被编译的文件，默认排除node_modules，bower_components，jspm_packages

  + ```json
    [
        "./src/dist/**/*"
    ]
    ```

+ `config.extends`：继承已有的配置文件

+ `config.files`：类似`config.include`，指定需要编译的文件

+ `config.compilerOptions`：编译选项

  + ```json
    {
        "target": "es2015",
        "module": "es2015",
        "lib": [],
        "outDir": "./dist",
        "outFile": "./dist/app.js",
        "allowJs": false,
        "checkJs": false,
        "removeComments": true,
        "noEmit": false,
        "noEmitOnError": true,
        "alwayStrict": false,
        "noImplicitAny": true,
        "strictNullChecks": true
    }
    ```

  + 选项说明：

    + `target`：指定编译的目标js版本号
    + `module`：指定使用模块化的标准
    + `lib`：指定项目中要使用的库
    + `outDir`：指定编译后的文件的存放目录
    + `outFile`：被编译的文件将会被编译到指定文件中
    + `allowJs`：是否对js文件进行编译，默认false
    + `checkJs`：检查js代码是否符合语法规范
    + `removeComments`：是否移除注释
    + `noEmit`：执行编译过程，但不生成编译后的文件
    + `noEmitOnError`：当有错误时不生成编译文件
    + `strict`：所有严格检查的总开关
    + `alwaysStrict`：设置编译后的文件是否使用严格模式
    + `noImplicitAny`：允许隐式的any类型
    + `noImplicitThis`：允许隐士的this
    + `strictNullChecks`：严格检查空值