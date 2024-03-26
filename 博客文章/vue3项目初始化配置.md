

## **一、技术栈说明**

技术栈： Vue3 + Vite4 + TypeScript + Pinia + Vue Router 等当前主流框架

## **二、项目搭建**

### **创建项目**

vue3的项目我们使用Vite来搭建

Vite官网：[https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project](https://link.zhihu.com/?target=https%3A//cn.vitejs.dev/guide/%23scaffolding-your-first-vite-project)

执行以下命令

```js
npm init vite@latest vue3-element-test --template vue-ts
```

接下来下载依赖

```js
cd vue3-element-test
npm install
```

下载好依赖，就可以打开本地项目看效果了

```js
npm run dev
```

### **vscode打开项目代码**

首先查看代码结构，src目录下就是我们的资源文件；

有可能`tsconfig.json`文件会报错，原因就是`moduleResolution`的配置为`bundler`；

我们可以将`"moduleResolution":node`，并删除`"allowImportingTsExtensions": true`。

**"moduleresolution": "bundler"**

- `bundler`：是TypeScript中的一个配置选项，它表示模块解析策略使用的是打包工具。在这种情况下，TypeScript 会查找已经打包的模块，而不是直接查找源代码文件。这通常用于在打包后的代码中使用模块别名或路径映射。

**"moduleresolution": "node"**

- node是 TypeScript 中的一个配置选项，它表示模块解析策略使用 Node.js 的模块解析算法。在这种情况下，TypeScript 会按照 Node.js 的模块查找规则来查找模块，并且允许使用 Node.js 的内置模块、第三方模块以及项目中的自定义模块。这是 TypeScript 默认的模块解析策略。

如果你创建项目后，默认得到的配置是Node，那就无需修改，直接正常运行就可以了。

## **三、TypeScript配置**

tsconfig.json配置文件有很多配置项，所有配置项我们都可以参考TypeScript官网

地址：[https://www.tslang.cn/docs/handbook/compiler-options.html](https://link.zhihu.com/?target=https%3A//www.tslang.cn/docs/handbook/compiler-options.html)

在tsconfig.json文件中，有很多配置，每个配置含义如下：

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "node",
    // "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

- `compilerOptions`：代表编译选项，因为我们项目vue3+ts来进行开发，每个ts的项目都会有一个tsconfig.json配置文件，里面所有的配置项都是ts在开发过程中编译的选项。

- `target`:代表TS代码编译后的JS版本。你可以填写“ES2015、ES2016”等等。

- `useDefineForClassFields`:它表示在编译期间是否将类字段声明为 JavaScript 的 `defineProperty` 调用。如果该选项设置为 true，则会在编译后的 JavaScript 代码中使用 `Object.defineProperty` 来定义类字段，从而使其成为类的属性访问器（accessor），这样可以提供更好的封装性和类型安全性。如果该选项设置为 false 或未设置，则会将类字段直接声明为 JavaScript 对象的属性，这种方式虽然简单但不具备类似于 `getter/setter` 的优势，也不利于进行类型检查。默认值为 false

- `module`:它表示使用的模块系统，即用于组织和加载代码的方式。模块系统的选择会影响到编译后的 JavaScript 代码的输出格式。

- - 可选值包括 `CommonJS`、`AMD`、`System`、`ES2015` 和 `ESNext`，默认为 `CommonJS`。
  - 同时，也可以将其设置为 `none`，这意味着不生成模块化的 JavaScript 代码，而是将所有模块合并到一个文件中输出（不推荐）。
  - ESNext是ECMAScript Next的缩写，它是ECMAScript标准的下一个版本的开发版本。换句话说，它是JavaScript语言的未来版本的提议和实验性功能的集合



- `lib`:配置项指定了要包括的目标平台API库,TypeScript编译器可以检查您的代码是否使用了特定的API，并防止意外使用不兼容的API。例如，如果您将"lib"设置为"ES2015"，则编译器将仅允许使用ECMAScript 2015规范中定义的API，而禁止使用ES6之前的API

- `skipLibCheck`:选项用于控制 TypeScript 编译器是否检查引用的库文件的类型定义文件（.d.ts 文件）。如果设置为 true，则编译器将跳过对这些库的类型定义文件的检查，从而可以加快编译速度。但是，这也可能导致在使用库时出现类型错误或其他问题。默认情况下，该选项为 false，即会检查库文件的类型定义文件。

- `resolveJsonModule`:用于指示编译器是否应该解析 JSON 文件并将其作为模块来处理。如果设置为 true，则编译器将对以 .json 扩展名结尾的文件执行模块解析，并使它们可用于 import 语句中。

- `isolatedModules`:用于检测模块之间的循环依赖并防止生成不适合执行的 JavaScript 代码。启用 "isolatedModules" 选项后，TypeScript 编译器会将每个文件视为独立的模块，并在编译时禁止跨文件共享变量、函数等实体。

- `noEmit`:用于控制编译器是否生成 JavaScript 代码文件。当 "noEmit" 设置为 true 时，TypeScript 编译器将不会生成任何 JavaScript 代码文件，只进行类型检查和语法分析。这在某些场景下可能很有用，比如只需要对 TypeScript 代码进行语法检查或者作为编辑器插件工作时

- `jsx`:通过 "jsx" 字段来配置如何处理 JSX 语法。其选项包括：

- - "preserve"：将 JSX 保留为原始字符串并输出。
  - "react-native"：生成适用于 React Native 应用的代码。
  - "react"：生成适用于 React 应用的代码。
  - "none"：禁用 JSX 支持。



- `strict`:代表是否编译的代码采用严格模式
- `noUnusedLocals`:如果将 "nouncusedlocals" 设置为 true，则编译器会在编译时检测到函数中未使用的本地变量，并发出错误或警告。如果设置为 false，则编译器不会检查未使用的本地变量
- `noUnusedParameters`:当将该选项设置为 true 时，TypeScript 编译器会在编译时检查每个函数或方法的参数列表中是否存在未使用的参数，并在发现未使用的参数时发出错误或警告。该选项可以在 tsconfig.json 文件中进行配置
- `noFallthroughCasesInSwitch`:当将该选项设置为 true 时，TypeScript 编译器会在编译时检查每个 switch 语句中的 case 块是否存在 fall-through（落入）的情况。如果存在 fall-through 的情况，TypeScript 编译器会发出一个错误以提示开发者

## **四、路径别名配置**

在我们项目中引入自定义组件或者JS\TS文件，我们会采用相对路径来寻找，比如：

```js
import Book from '../components/Bppk.vue'
```

这个配置在Vue\cli脚手架中默认可以使用，但是在Vite搭建的Vue3项目中我们需要配置一下；

**整体配置如下：**

```js
// vite.config.ts
import vue from '@vitejs/plugin-vue'
import { UserConfig, ConfigEnv, loadEnv, defineConfig } from "vite";

import path from "path";
const pathSrc = path.resolve(__dirname, "src");

// https://vitejs.dev/config/
export default defineConfig(({ mode }: ConfigEnv): UserConfig => {
  return {
    resolve: {
      alias: {
        "@": pathSrc,
      },
    },
    plugins: [
      vue()
    ]
  }
})
// tsconfig.json
"compilerOptions": {
    "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
    "paths": { // 路径映射，相对于baseUrl
        "@/*": ["src/*"] 
    }
}
```

在配置完成后，可能会报错为：`找不到模块path或其相应的类型声明`

**解决方案**：官方已经提供了声明文件，无需自己写d.ts文件，下载依赖即可

```js
npm i @types/node --save-dev
```

下载node包对应的types声明文件，当你下载完毕后，报错信息就消失了。

## **五、安装自动导入**

基本概念：为了避免在多个页面重复引入 `API` 或 `组件`，由此而产生的自动导入插件来节省重复代码和提高开发效率。

两种自动导入对于的不同的vite插件来实现。

| 名字                    | 含义             | 详情                                          |
| ----------------------- | ---------------- | --------------------------------------------- |
| unplugin-auto-import    | 按需自动导入API  | ref，reactive,watch,computed 等API            |
| unplugin-vue-components | 按需自动导入组件 | Element Plus 等三方库和指定目录下的自定义组件 |

### **开发步骤**

首先，下载依赖；

```js
npm install -D unplugin-auto-import unplugin-vue-components 
```

然后再src目录下创建types文件夹，来存放我们自己的类型声明文件、相关约束的类型文件；

打开`viteconfig.ts`文件夹：

```js
import vue from '@vitejs/plugin-vue'
import { UserConfig, ConfigEnv, loadEnv, defineConfig } from "vite";
//配置路径别名
import path from "path";
const pathSrc = path.resolve(__dirname, "src");
//自动导入插件
import AutoImport from "unplugin-auto-import/vite";
import Components from "unplugin-vue-components/vite";

// https://vitejs.dev/config/
export default defineConfig(({ mode }: ConfigEnv): UserConfig => {
  return {
    resolve: {
      alias: {
        "@": pathSrc,
      },
    },
    plugins: [
      vue(),
      AutoImport({
        // 自动导入 Vue 相关函数，如：ref, reactive, toRef 等
        imports: ["vue"],
        eslintrc: {
          enabled: true, // 是否自动生成 eslint 规则，建议生成之后设置 false 
          filepath: "./.eslintrc-auto-import.json", // 指定自动导入函数 eslint 规则的文件
        },
        dts: path.resolve(pathSrc, "types", "auto-imports.d.ts"), // 指定自动导入函数TS类型声明文件路径
      }),
      Components({
        dts: path.resolve(pathSrc, "types", "components.d.ts"), // 指定自动导入组件TS类型声明文件路径
      }),
    ]
  }
})
```

配置完成后，会在types文件下生成功两个文件`auto-imports.d.ts`、`components.d.ts`，以及在根目录下生成`.eslintrc-auto-import.json`文件；

## **六、scss预编译处理器，全局变量定义**

我们在vue中我们可以引入scss来配合我们css样式开发设计。

当然你还可以将很多公共的样式，提取为变量和函数，放在指定文件中。达到以后一键切换效果。

下载依赖

```js
npm install -D sass
```

在src目录下创建styles文件夹，`/src/styles/variables.scss`

```css
//定义全局变量
$bg-color:#ff0000;
```

在`vite.config.ts`文件中配置

```js
css: {
    // CSS 预处理器
    preprocessorOptions: {
        //define global scss variable
        scss: {
            javascriptEnabled: true,//开启sass
            additionalData: '@import "@/styles/sass/variables.scss";'
        }
    }
}
```

在页面中使用scss编写样式，并引入全局变量

```js
<template>
  <div>sass</div>
</template>

<script lang="ts" setup>
import { reactive, ref } from "vue";
</script>

<style lang="scss" scoped>
div {
  color: $bg-color;
}
</style>
```

## **七、less全局变量定义**

下载依赖

```js
npm install -D less
```

在src目录下创建styles文件夹，`/src/styles/variables.less`

```css
//定义全局变量
@bg-color:#002aff;
```

在`vite.config.ts`文件中配置

```js
css: {
    // CSS 预处理器
    preprocessorOptions: {
        //define global less variable
        less: {
            javascriptEnabled: true,
        	charset: false, //禁用字符集声明(charset 选项用于控制是否在生成的 CSS 文件的头部添加 @charset "UTF-8";)
          	additionalData: '@import "./src/styles/less/variables.less";'
        }
    }
}
```

在页面中使用scss编写样式，并引入全局变量

```js
<template>
  <div>less</div>
</template>

<script lang="ts" setup>
import { reactive, ref } from "vue";
</script>

<style lang="less" scoped>
div {
  color: @bg-color;
}
</style>
```

## **八、配置环境变量**

项目开发中会有多种环境

- 开发环境
- 测试环境
- 生产环境

vite官网提供了环境变量的配置信息：[https://cn.vitejs.dev/guide/env-and-mode.html](https://link.zhihu.com/?target=https%3A//cn.vitejs.dev/guide/env-and-mode.html)

### **开始配置**

在根目录下分别创建`.env.development`、`.env.production`、`.env.test`；

**注意：# 变量必须以 VITE_ 为前缀才能暴露给外部读取**

`.env.development`代表开发环境的配置，内容如下：

```js
# 应用标题
VITE_APP_TITLE = '开发环境'
# 应用端口
VITE_APP_PORT = '7010'
# 请求接口
VITE_APP_DEV_URL = 'http://192.168.0.144:7010' 
# API基础路径(反向代理)
VITE_APP_BASE_API = '/dev-api'
```

`.env.production`代表生产环境配置，内容如下：

```js
# 应用标题
VITE_APP_TITLE = '生产环境'
# 接口地址
VITE_APP_BASE_API = 'http://bg.zmyun.com:7001'
```

`.env.test`代表生产环境配置，内容如下：

```js
# 应用标题
VITE_APP_TITLE = '测试环境'
# 应用端口
VITE_APP_PORT = 5555
# 请求接口
VITE_APP_DEV_URL = 'http://192.168.0.144' 
# API基础路径(反向代理)
VITE_APP_BASE_API = '/pro-api'
```

## **九、配置代理服务器**

浏览器默认情况下会出现跨域报错的问题。这是因为浏览器同源策略的影响。

本地开发环境通过 `Vite` 配置反向代理解决浏览器跨域问题，生产环境则是通过 `nginx` 配置反向代理 。

也就说本地你配置Proxy代理服务只能在本地运行，一旦项目打包后，本地代理无效了，需要在nginx配置反向代理来解决这个问题。

在`vite.config.js`中配置

```js
server: {
 	host: '0.0.0.0',//指定服务器应该监听哪个 IP 地址
 	port: 3000,//指定服务器端口号
 	strictPort: true,//端口被占用直接退出
 	https: false,//启用 TLS + HTTP/2
 	open: false,//在开发服务器启动时自动在浏览器中打开应用程序
 	proxy: {
 		//配置自定义代理规则
 		[env.VITE_APP_BASE_API]: {
 		target: env.VITE_APP_DEV_URL,
 		changeOrigin: true,
  		rewrite: path => path.replace(new RegExp("^" + env.VITE_APP_BASE_API), "")
 	},
        // '/api': {
        //   target: 'http://192.168.0.144:7010',
        //   changeOrigin: true,//是否跨域
        //   rewrite: (path) => path.replace(/^\/api/, ''),
        // },
 	}
},
```

## **十、下载ElementUI Plus，并配置按需引入**

**注意，这里只是演示某个UI库，你也可以使用 Antd**

官网地址：[https://element-plus.gitee.io/zh-CN/guide/design.html](https://link.zhihu.com/?target=https%3A//element-plus.gitee.io/zh-CN/guide/design.html)

下载依赖：

```js
npm install element-plus
```

### **配置按需引入**

前面我们在第五步的时候，以及下载了按需引入的插件，现在我们直接开始配置

打开`vite.config.ts`文件

```js
import { defineConfig } from 'vite'
// 自动引入插件
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
// ElementPlus
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

## **十一、下载Antd，并配置按需引入**

网址：[https://next.antdv.com/docs/vue/introduce-cn](https://link.zhihu.com/?target=https%3A//next.antdv.com/docs/vue/introduce-cn)

下载依赖：

```js
npm install ant-design-vue --save
```

### **配置按需引入**

前面我们在第五步的时候，以及下载了按需引入的插件，现在我们直接开始配置

打开`vite.config.ts`文件

```js
// vite.config.js
import Components from 'unplugin-vue-components/vite';
import { AntDesignVueResolver } from 'unplugin-vue-components/resolvers';

export default {
  plugins: [
    /* ... */
    Components({
      resolvers: [
         	AntDesignVueResolver({
            	importStyle: false, // css in js(将css样式进行内联，不单独成为一个文件引入)
         	}), 
      ],
    }),
  ],
};
```

### **配置icon全局引入**

main.ts文件

```js
import { createApp } from 'vue';
import App from '@/App.vue';
import Antd from 'ant-design-vue';
import '@/assets/style/theme/myAntd.less';//引入配置后的主题色



const app = createApp(App);
app.use(Antd);
/**
 * 全局注册图标组件
 * 出现的问题：元素隐式具有 "any" 类型，因为类型为 "string" 的表达式不能用于索引类型
 * 解决方案1：
 *      首先通过 typeof 操作符获取 Icons 变量的类型，然后通过 keyof 操作符获取该类型的所有键
 *      key as keyof typeof antIcons
 * 解决方案2：
 *      在 tsconfig.json 文件中添加 "suppressImplicitAnyIndexError": true 规避错误。
 */
Object.keys(antIcons).forEach((key: string) => {
    // app.component(key, antIcons[key as keyof typeof antIcons])
    // @ts-ignore
    app.component(key, antIcons[key])
});
app.config.globalProperties.$antIcons = antIcons;


app.mount('#app');
```



## **十二、路由环境搭建**

下载路由依赖

```js
npm i vue-router@next
```

在 `src` 中创建 `router/index.ts` 文件

```js
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router';
import Login from "@/pages/Login.vue"
import Register from "@/pages/Register.vue"
import { useStore } from "@/stroe";
import { getUserRoutes } from "@/api/user";

const routes: Array<RouteRecordRaw> = [
    {
        path: '/login',
        component: Login
    },
    {
        path: '/Register',
        component: Register,
        beforeEnter: (to, from, next) => {
            console.log(123);
        }
    }
]
const router = createRouter({
    routes,
    // 路由模式
    history: createWebHistory()
})

# 根据项目需求进行调整
// 全局导航守卫 
router.beforeEach(async (to, _from, next) => {
    /* 路由发生变化修改页面title */
    if (to.meta.title) {
        // to.meta.title:名称挂载在路由表中
        document.title ='myDome_' +  String(to.meta.title);
    }
    let token = localStorage.getItem('token');//获取本地token
    if (token) {
        const store = useStore();//实例化
        if (store.routes.length < 1) {//当动态路由的数组对象为0时候，需要重新请求，并更新动态路由
            let res: any = await getUserRoutes();//请求数据
            if (200 === res.code) {
                store.addRoutes(res.data.routes, router);//添加动态路由
                // 最后手动添加重定向信息
                router.addRoute({
                    path: "/:catchAll(.*)",
                    redirect: "/404",
                })
                next({ ...to, replace: true });
            } else {
                next()
            }
        } else {
            next()
        }
    } else {
        if (to.path == "/login") {
            next();
        } else {
            next("/login")
        }
    }
})

// 暴露出去
export default router;
```

在`main.ts`文件中引入

```js
import { createApp } from 'vue'
import './style.css'// 全局样式
import App from './App.vue'
import router from './router'// 路由


const app = createApp(App);
app.use(router);
app.mount('#app');
```

## **十三、i18n国际化配置**

## **十四、axios请求封装**

下载依赖

```js
npm install axios
```

在src下创建`https/newAxios.ts`

```js
# 以下是基本结构，需要的时候，有必要改造以下
import { message } from 'ant-design-vue';
import axios from 'axios';
import modelTool from '@/util/modelTool';//自己封装的方法函数
import router from '@/router';
import { errorCodeType } from '../errorCodeType';//不同状态码，返回不同的值

// 创建axios实例
const newAxios = axios.create({
    baseURL: import.meta.env.VITE_APP_BASE_API,//拿到执行环境下的请求接口
    // 超时设置
    // timeout: 15000,
    headers: { 'Content-Type': 'application/json;charset=utf-8' }
});

// 请求拦截
newAxios.interceptors.request.use(
    config => {
        // 本地拿到token，添加到请求头中
        // let tokenObj = modelTool.tokenBoolean();
        // if (tokenObj.state) config.headers['Authorization'] = `Bearer ${tokenObj.token}`;
        return config;
    },
    error => {
        Promise.reject(error);
    }
);

// 响应拦截
newAxios.interceptors.response.use(
    response => {
        if (response.data['code'] === 200) {
            return Promise.resolve(response)
        } else {
            return Promise.reject(response)
        }
    },
    // 服务器状态码不是200的情况
    error => {
        console.log(error.response);

        // if (error.response.status) {
        //     switch (error.response.data.code) {
        //         // 登录有效期已过，请用户重新登录！
        //         case 401:
        //             setTimeout(() => {
        //                 message.warning(error.response.data.message)
        //                 modelTool.localStorageDel()
        //                 router.push('/login')
        //             }, 1000)
        //             break;
        //         // 请求错误
        //         case 400:
        //             message.warning(error.response.data.message)
        //             break;
        //         // 其他错误
        //         default:
        //             message.error(errorCodeType(error.response.status))
        //             break;
        //     }
        //     return Promise.reject(error.response)
        // }
    }
);

// 暴露axios实例
export default newAxios;
```

在src下创建`https/request.ts`

```js
# 请求封装，看个人喜好进行封装
import newAxios from "./newAxios";//引入axios实例

// 请求封装并暴露
/**
 * get方法，对应get请求
 * @param {String} url [请求的url地址]
 * @param {Object} params [请求时携带的参数]
 */
export function get(url: string, params: any) {
    return new Promise((resolve, reject) => {
        newAxios.get(
            url,
            params
        ).then(res => {
            resolve(res.data)
        }).catch(err => {
            reject(err.data)
        })
    })
};
/**
* post方法，对应post请求
* @param {String} url [请求的url地址]
* @param {Object} params [请求时携带的参数]
*/
export function post(url: string, params: any) {
    return new Promise((resolve, reject) => {
        newAxios.post(
            url,
            params
        ).then(res => {
            resolve(res.data)
        }).catch(err => {
            reject(err.data)
        })
    })
};
```

最后在src下创建`apis/*.ts`文件

```js
# 比如创建 user.ts
import { post , get } from '@/https/request';//引入post、get方法

/**
* (params: any):接收到请求参数，并进行数据约束
* post('/login', params)：
* /login:请求接口
* params：请求参数
*/
export const loginApi = (params: any) => post('/login', params);

# 在页面中使用
import { loginApi } from '@/api/user';//导入请求方法
async function test(params:type) {
 	let loginMatch = {name:'张三',psd:'zs123456'};//请求参数
 	// 登录接口调用
 	let res: any = await loginApi(loginMatch);
 	console.log('请求返回的数据：',res)
}
```

## **十五、pinia状态机**

Pinia这个状态机目前默认在Vue3中使用；

这个状态机内部的设计思想和Vuex很多类似的，但是更偏向于组合式api；

内部现在变成三大模块：state、getters、actions；

下载依赖：

```js
npm i pinia
```

加载pinia状态机.main.ts文件中

```js
import { createApp } from 'vue'
// 全局样式
import './style.css'
import App from './App.vue'
// 状态机
import {createPinia} from "pinia"

const app = createApp(App);
app.use(createPinia())
app.mount('#app');
```

在src目录下面创建`store\useStore.ts`

```js
import { defineStore } from "pinia"
export const userStore = defineStore("userStore", {
    state: () => {
        return {
            count: 10
        }
    },
    getters: {
        doubleCount(state) {
            return state.count * 2
        },
    },
    actions: {
        //可以存放普通函数，也可以存放异步函数
        increment(count: number) {
            // context.commit("xxxx",state)
            this.count += count
        },
        decrement() {
            this.count -= 10
        },
        asyncIncrement() {
            setTimeout(() => {
                this.count += 10
            }, 1000);
        }
    }
})
```

页面中使用状态机

```js
import {userStore} from "@/store/userStore"
const userStoreData = userStore()
//userStoreData = {userStore:{}}
console.log(userStoreData)
//调用actions中的函数进行修改
userStoreData.increment(100)
```

## **十六、Vuex状态机**

下载依赖

```js
npm i vuex@next
```

在 `src` 目录下创建一个 `store/index.js` 文件，在该文件中进行 Vuex 的配置

```js
import { createStore,Store,StoreOptions } from 'vuex';
import {IRootState} from "../types/root-types"
const store = createStore<IRootState>({
    state: {
        username:"xiaowang",
        users:[
            {id:1,name:"王小二"}
        ]
    },
    getters: {},
    mutations: {
        changeUsername(state,payload){
            state.username = payload
        }
    },
    actions: {
        asyncChangeUsername(context,payload){
            setTimeout(() => {
                context.commit("changeUsername",payload)
            }, 1000);
        }
    },
    modules: {}
})
export default store
```

在`root-types`文件中的约束，一旦定义类约束，state的数据内容就已经被约束了

```js
export interface IRootState {
    //根据实际情况里面定义自己需要的类型
    username:string,
    users:Array<{id:number,name:string}>
}
```

在 `main.js` 中引入仓库对象：

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store';
const app = createApp(App);
app.use(router);
app.use(store);
app.mount('#app');
```

组件中操作仓库

由于是在vue3中使用，`this.$store` 不能使用了，辅助函数也不能使用了。

因此，如果我们要获取仓库对象，需要调用 `useStore()` 方法：

```js
// 比较：
<script setup>
import { useStore } from 'vuex'
// 等同于 this.$store
const store = useStore();
</script>   

// 实例：
<template>
  <h3>这是App</h3>
  <Prize></Prize>
  <p>{{store.state.productModel}}</p>
  <button @click="changename">修改</button>
</template> 

<script lang='ts' setup>
import { useStore } from "vuex";
const store = useStore()
const changename = ()=>{
  store.commit("productModel/incrementAge")
}
</script>

<style lang='scss' scoped>
.box {
  .h3 {
    color: red;
  }
}
</style>
```

## **十七、自定义指令**

### **全局指令**

在main.ts文件中引入下面的代码

```js
app.directive('focus', (el) => el.focus())
```

将指令挂在到app对象身上，任何一个组件都可以使用

```js
app.directive('focus', (el, binding) => {
    console.log(binding.value);
    el.focus()
})
```

### **局部指令**

## **十八、打包上线流程**

打开`vite.config.ts`文件进行上线配置

```js
// 生产环境配置
build: {
    outDir: 'dist',
    assetsDir: 'assets',
    sourcemap: false,
    terserOptions: {
        compress: {
            drop_console: true,
            drop_debugger: true,
        },
    },
},
```

打开`.env.production`，将`VITE_APP_DEV_URL`以及`VITE_APP_PORT`切换为上线后的后端接口

```js
# 应用标题
VITE_APP_TITLE = '生产环境'
# 应用端口
VITE_APP_PORT = 7001
# 请求接口
VITE_APP_DEV_URL = 'http://192.168.0.146' 
# API基础路径(反向代理)
VITE_APP_BASE_API = '/pro-api'
```

执行以下命令进行打包，会产生一个打包后的文件`dist`

```js
npm run build
```

打包完成后，可以执行以下命令对打包后代码进行本地测试

```js
npm run preview
```