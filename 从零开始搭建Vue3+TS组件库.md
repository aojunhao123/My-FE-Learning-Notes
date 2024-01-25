# 从零到一实现Vue3+TS组件库

## 项目目录

```text
-- .husky // git钩子函数
-- node_modules 开发依赖
-- packages
	-- cli 脚手架
	-- components 组件库开发
		-- script 脚本目录(打包,发布)
		-- src 组件存放目录
	-- veloui 组件库打包后的文件
-- play 组件库调试
-- site 组件库文档站点
-- .eslintignore 
-- .eslintrc.js // 编码规范配置文件
-- .gitignore
-- .prettierrc.cjs // 格式化代码的配置文件
-- .stylelintrc.cjs // 规范样式的配置文件
-- commitlint.config.cjs // 规范git提交的配置文件
-- package.json
-- pnpm-lock.yaml // 锁定依赖版本
-- pnpm-workspace.yaml // 管理项目子包
-- readme.md // 项目说明文档
-- tsconfig.json // Typescript配置
```



## Monorepo

### Monorepo简介

**Monorepo** 是一种软件开发的策略模式，它代表"单一代码仓库"（Monolithic Repository）。在 **Monorepo** 模式中，所有相关的项目和组件都被存储在一个统一的代码仓库中，而不是分散在多个独立的代码仓库中。

简单理解：所有的项目在一个代码仓库中，但并不是说代码没有组织的都放在 ./src 文件夹里面。

## pnpm搭建Monorepo项目环境

### 为什么是pnpm?

对于一个Monorepo项目,尤其是组件库,项目下可能会有多个package(包),而这些包在我们本地是需要相互关联测试的,而pnpm就对其天然支持.而yarn和lerna虽然也能做到,但相对繁琐

### 为什么pnpm能实现Monorepo?

使用了类似Linux软链接的方式,实现了一个模块多处复用

### 包管理

由于Monorepo架构下一个项目有多个包,而这些包在逻辑上是相互独立的,但也可能需要相互关联测试,因此我们要对其进行管理

以本项目为例,在项目根路径下新建pnpm-workspace.yaml

```yaml
# 将项目目录下的包全部关联
packages:
  - "packages/**" #关联packages下的所有包
```

这样就将packages下的所有子包都关联起来了

而对于项目中的子包,他们之间可能也存在互相引用的情况

以components和play为例,play是我们的组件开发调试环境,因此需要引用我们本地开发的组件,如何引用呢?

很简单,只需要在play目录下执行`pnpm add @velo/components`,之后你便会发现play目录下的`package.json`中出现了相应依赖

![image-20230728105930324](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230728105930324.png)

## 组件库环境搭建

本项目是使用Vue3+Typescript搭建的组件库,并且使用sass管理样式,因此我们需要配置相应环境

首先安装相应依赖

`pnpm add vue@next typescript less -D -w`

小tips:使用pnpm安装依赖时,若要安装在根目录下要加 `-w`

### 初始化TS

根目录执行`npx tsc --init`,项目目录下便会自动生成配置文件`tsconfig.json`,接下来进行相应配置

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "jsx": "preserve",
    "strict": true,
    "target": "ES2015",
    "module": "ESNext",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "moduleResolution": "Node",
    "lib": ["esnext", "dom"]
  }
}
```

各配置项的相应解释如下:

1. `"baseUrl": "."`: 这个选项指定了项目中解析非相对模块名称的基准目录。在这里，它设置为当前目录，意味着非相对模块名称将从当前目录开始解析。
2. `"jsx": "preserve"`: 这个选项指定了TypeScript如何处理JSX语法。在这里，设置为"preserve"，意味着TypeScript将保留JSX代码而不进行转换。
3. `"strict": true`: 这个选项启用了TypeScript的严格模式，其中包含了一系列的严格类型检查和规范，有助于提高代码质量和类型安全性。
4. `"target": "ES2015"`: 这个选项指定了TypeScript编译后生成的JavaScript代码的目标版本。在这里，代码将被编译为符合ES2015（ES6）规范的JavaScript代码。
5. `"module": "ESNext"`: 这个选项指定了生成的JavaScript模块的类型。在这里，模块将被编译为ESNext，表示使用最新的JavaScript模块语法。
6. `"skipLibCheck": true`: 这个选项告诉TypeScript编译器在类型检查时跳过声明文件（.d.ts文件）的检查。这可以加快编译速度，但有时也可能导致类型错误的隐藏。
7. `"esModuleInterop": true`: 这个选项启用了对ES模块和CommonJS模块之间互操作性的支持。它使得在导入和导出模块时更加方便。
8. `"moduleResolution": "Node"`: 这个选项指定了TypeScript解析模块名时使用的模块解析策略。在这里，设置为"Node"，表示使用Node.js的模块解析规则。
9. `"lib": ["esnext", "dom"]`: 这个选项指定了在编译过程中可供使用的JavaScript内置库。在这里，编译器将包含"esnext"和"dom"两个库，使得你可以在代码中使用ESNext和DOM相关的特性和接口。

### 配置组件库调试环境

使用Vite快速搭建一个Vue3项目,用于对开发完成的组件进行调试

具体搭建过程就不写了,需要注意的就是关联本地开发的组件库,以及将play目录放在pnpm包管理下

### 开发组件

挑几个重点讲

#### 如何让组件库支持全局引入

```typescript
// 导入 App 和 Plugin 类型。App 类型代表 Vue 应用实例，Plugin 类型用于声明插件对象。
import type { App, Plugin } from 'vue';
// 定义交叉类型
type SFCWithInstall<T> = T & Plugin;

const withInstall = <T>(comp: T) => {
  (comp as SFCWithInstall<T>).install = (app: App) => {
    const name = (comp as any).name;
    //注册组件
    app.component(name, comp as SFCWithInstall<T>);
  };
  return comp as SFCWithInstall<T>;
};
export const Button = withInstall(_Button);
export default Button;
```

#### 如何在setup语法糖中给组件命名

有几种方案

方案一是写两个`script`标签,在其中一个标签(未使用setup语法糖)中定义组件名

方案二:使用`unplugin-vue-define-options`插件

介绍:`unplugin-vue-define-optionsTypeScript`是一个用于Vue3的Typescript 插件，用于增强组件选项的类型推导和编辑器支持，以提供更好的代码提示和类型检查功能

使用:安装插件后,在vite配置文件中进行相应的配置,之后我们便能在组件文件中使用`defineOptions`函数定义组件名了

```vue
<script setup lang="ts">
    defineOptions({ name: 've-button' });
</script>
```

## Vite打包组件库

Vite专门提供了库模式的打包,非常方便

全局安装`Vite`和`@vitejs/plugin-vue`,components目录下新建vite配置文件`vite.config.ts`,

进行相应配置并在package.json中配置相应打包命令后,就可以开始打包了

关于打包的配置文件如下

```json
export default defineConfig({
build: {
    //打包文件目录
    outDir: 'es',
    //压缩
    //minify: false,
    rollupOptions: {
      //忽略打包vue文件
      external: ['vue', /\.scss/],
      input: ['index.ts'],
      output: [
        {
          //打包格式
          format: 'es',
          //打包后文件名
          entryFileNames: '[name].mjs',
          //让打包目录和我们目录对应
          preserveModules: true,
          exports: 'named',
          //配置打包根目录
          dir: '../veloui/es'
        },
        {
          //打包格式
          format: 'cjs',
          //打包后文件名
          entryFileNames: '[name].js',
          //让打包目录和我们目录对应
          preserveModules: true,
          exports: 'named',
          //配置打包根目录
          dir: '../veloui/lib'
        }
      ]
    },
    lib: {
      entry: './index.ts'
    },
    plugins: [vue()],
    });
```

#### 声明文件

打包后的组件库其实只能供js使用,若在ts环境下运行不仅会报错,而且也失去了类型提示的功能,这样与我们开发TS组件库的目的相悖.为了解决这个问题,我们需要让组件库打包后的文件有ts声明文件

安装`vite-plugin-dts`,并在vite中引入

再次打包后就便能发现打包后的文件中出现了我们需要的声明文件

#### 关于按需引入

由于现在的大部分前端构建工具都支持ESM了,而ESM天然支持按需加载,因此组件库打包后的es格式自带tree shaking,不需要额外配置按需引入.不过vite打包时会将组件库样式一并打包,导致组件样式无法按需引入,因此我们需要引入其他工具对组件样式进行**单独打包**



## gulp

### gulp是什么？

前端流程化控制工具

比如我们要把一个大象放进冰箱里就需要 打开冰箱门->把大象放进冰箱->关上冰箱门，这就是一个简单的流程，使用gulp就可以规定这些流程，将这个流程自动化。

### 使用场景

项目开发过程中自动执行常见任务。比如打包一个组件库，我们可能要移除文件、copy文件，打包样式、打包组件、执行一些命令还有一键打包多个package等等都可以由gulp进行自定义流程的控制，非常的方便。

### 使用

#### 创建一个任务(Task)

每个gulp任务（task）都是一个异步的JavaScript函数，此函数是一个可以接收callback作为参数的函数，或者返回一个Promise等异步操作对象，比如创建一个任务可以这样写

```js
exports.default = (cb) => {
  console.log("my task");
  cb();
};
```

终端输入**gulp**就会执行我们这个任务

#### 串行(series)和并行(parallel)

串行就是任务一个接一个执行，并行就是所有任务一起执行

```js
const { series, parallel } = require("gulp");

const task1 = () => {
  console.log("task1");
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, 5000);
  });
};
const task2 = () => {
  console.log("task2");
  return Promise.resolve();
};

exports.default = series(task1, task2);
```

### src()和dest()

这是实际项目开发中经常用到的两个函数.其中,src()表示创建一个读取文件系统的流;dest()是创建一个写入文件系统的流.

如下示例:将src目录下的所有文件复制并写入dist目录

```js
const { src, dest } = require('gulp');
// 创建一个任务，将src下的文件拷贝到dist下
 const copy = () => {
     return src('src/*').pipe(dest('dist'));
 }
 exports.default = copy;
```

如下示例:处理sass文件,将其编译成css文件并写入指定目录

```js
const sass = require('gulp-sass')(require('sass'));
const autoprefixer = require('gulp-autoprefixer');
const sassTask = () => {
    return src('src/style/*.scss')
        .pipe(sass())
        .pipe(
            // 添加浏览器前缀
            autoprefixer({
                overrideBrowserslist: ["> 1%", "last 2 versions"],
                cascade: false, //  是否美化属性值
            })
        )
        .pipe(dest('dist/style'));
}
```

### browser sync

**browser-sync**是一个十分好用的**浏览器同步测试工具**，它可以搭建静态服务器，监听文件更改，并刷新页面（HMR）

#### 使用

首先在src目录下创建index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
        hello world
</body>
</html>
```

创建启动该页面的任务和监听页面变化的任务

```js
const { watch } = require("browser-sync");
const browserSync = require("browser-sync");
const { series } = require("gulp");

const reloadTask = () => {
  browserSync.reload();
};

const browserTask = () => {
  browserSync.init({
    server: {
      baseDir: "./src", // 启动服务器的目录
    },
  });
  // 监听页面的变化  
  watch("./src/*", series(reloadTask));
};

exports.default = browserTask;
```

### 模拟一个简单的构建流

编译sass样式-->将css写入dist/style-->触发页面更新

```js
// 导入gulp模块
const {  src, dest } = require('gulp');
const browserSync = require("browser-sync");
const { watch } = require("browser-sync");
const { series } = require("gulp");
const sass = require('gulp-sass')(require('sass'));
const autoprefixer = require('gulp-autoprefixer');

// 将sass文件转换为css文件
const sassTask = () => {
    return src('src/style/*.scss')
        .pipe(sass())
        .pipe(
            // 添加浏览器前缀
            autoprefixer({
                overrideBrowserslist: ["> 1%", "last 2 versions"],
                cascade: false, //  是否美化属性值
            })
        )
        .pipe(dest('dist/style'));
}


// 监听文件变化
const reloadTask = () => {
    browserSync.reload();
};
// 创建一个任务，启动一个服务器
const browserTask = () => {
    browserSync.init({
        server: {
            baseDir: "./", // 服务器启动的目录
        },
    });
    // 监听文件变化，执行reloadTask
    watch("./*.html", series(reloadTask));
    // 监听样式文件变化，执行sassTask和reloadTask
    watch("./src/style/*", series(sassTask, reloadTask));
};
exports.default = browserTask;
```

这样一来,无论我们是修改页面结构还是样式,都会自动触发页面更新

## 使用gulp打包组件库并实现按需加载

### 为什么使用gulp打包?vite打包不能实现按需加载吗?

当我们使用vite库模式打包组件库时,会将样式文件全部打包到同一个文件中,这样的话我们每次都要全量引入所有样式文件,无法做到按需加载.因此,我们在打包的时候可以不让vite打包样式文件,而是使用gulp对样式文件进行打包.

### 自动按需引入插件

现在很多组件库的按需引入都是借助插件来解决的,比如`ElementPlus`是使用`unplugin-vue-components`和`unplugin-auto-import`,这两个插件可以实现

```js
import { Button } from "easyest";

//相当于
import "easyest/es/src/button/style/index.css";
import "easyest/es/src/button/index.mjs";
```

### gulp打包样式文件

进行相应配置让gulp支持Typescript和es6语法

`pnpm i gulp @types/gulp sucrase -D -w`

流程:

删除上一次打包后的文件

打包:

- 由于我们是使用sass管理组件库样式,因此需要安装相关依赖,这里就先不赘述了
- 同时需要安装`autoprefixer`自动给浏览器添加前缀(为了在不同浏览器中都能正确显示样式)
- 使用`gulp`的src和dest函数读取和写入sass样式文件
- 先打包样式,后打包组件

#### 总结

组件库实现按需加载的原理就是通过Vite打包组件,以及gulp对组件样式单独打包

因为Vite支持ESM,而ESM天然支持按需加载,tree shaking,但缺点就是会将样式也一并打包,导致使用组件库时需要对样式全量引入.因此我们使用gulp先对组件库样式进行单独打包,这样就实现了组件库的按需加载

## 自动管理与发布组件库

组件库打包完毕后便可以通过`pnpm publish`发布到npm上了,但是每次发布都需要手动提升版本号,手动打tag,不够方便.因此我们可以选择采用`release-it`来自动化这一流程

在打包后的文件目录的package.json下添加相应指令和GitHub地址,在components/script下添加publish目录,编写自动管理发布脚本

```json
"scripts": {
    "release": "release-it"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/aojunhao123/Velo"
  }
```

之后在项目根目录的package.json下添加用于发布组件库的指令就好啦

![image-20230728195031845](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230728195031845.png)

## 搭建组件库文档并部署到GitHub静态站点

使用Vue3官方推荐的文档搭建工具VitePress进行我们的组件库文档搭建

首先还是老一套:

- 在项目根目录下新建site文件夹,并执行`pnpm init`生成package.json
- 在package.json中添加如下脚本

<img src="C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230728200254065.png" alt="image-20230728200254065" style="zoom:50%;" />

docs:dev用于在开发环境下运行VitePress文档项目,你可以实时开发调试并预览,VitePress也支持热更新

docs:build用于对文档进行打包构建,打包后默认输出目录是`docs/.vitepress/dist`,你可以将其部署在服务器或静态站点上

docs:preview用于预览生产环境下的文档,也就是打包后的文档

之后在site下新建docs/index.md,内容就你自由发挥吧.

### 文档中引入组件

若想在组件库文档中引用你写好的组件,你需要将site包与components包相关联,并将打包后的组件库文件也进行关联

具体操作是:

- 在`pnpm-workspace.yaml`文件中关联site
- 在site目录下执行`pnpm add ajh-veloui`,将组件库添加到site依赖中(ajh-veloui就是之前发布到npm的组件库)
- **重点**:在`.vitepress`目录下新建theme/index.js,引入组件和vitepress默认主题样式

```js
import DefaultTheme from "vitepress/theme";
import veloui from "ajh-veloui"
export default {
    ...DefaultTheme,
    enhanceApp: async ({ app }) => {
        // app is the Vue 3 app instance from `createApp()`. router is VitePress'
        // custom router. `siteData`` is a `ref`` of current site-level metadata.
        app.use(veloui);
    },
};
```

执行完这一步之后,我们就可以在文档中引入组件啦

看看效果:

![image-20230728210542237](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230728210542237.png)

### 部署文档到静态站点

在.vitepress/config.js中添加如下配置,配置我们的生产环境下的文档路径为/velo/

![image-20230729110635238](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230729110635238.png)

因为部署上线的网站目录为

![image-20230729110806313](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230729110806313.png)

还需要在你的GitHub账户下新建组织(organization),在组织中新建仓库,将**打包后**的文档上传到该仓库中

![image-20230729110116999](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230729110116999.png)

选好要部署的分支,点击save后就自动上传了

## Monorepo架构下,实现根路径管理所有子项目脚本

当我们在Monorepo架构的项目中时,由于项目中有多个子包,导致每次执行脚本时需要切换到对应子包目录下,十分麻烦.

打个比方:![image-20230726112443767](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230726112443767.png)

这是site目录下的脚步,管理着文档的开发,打包和预览,而我们在正式开发时不可能总是切换目录.为了能在项目根路径中管理所有子包的脚本命令,我们可以使用`--filter`选项

![image-20230726134658542](C:/Users/1/AppData/Roaming/Typora/typora-user-images/image-20230726134658542.png)

--filter 过滤器,用于过滤特定的包,可以安装满足指定条件的包而不安装其他的包
