# vue-jsx-typescript

> 使用jsx和typescript配置vue开发环境

## 使用vue-cli初始化vue环境

``` bash
vue init webpack my-project
```

## 添加tsconfig.json

```json
{
  "compilerOptions": {
    // this aligns with Vue's browser support
    "target": "es5",
    // this enables stricter inference for data properties on `this`
    "strict": true,
    // if using webpack 2+ or rollup, to leverage tree shaking:
    "module": "es2015",
    "moduleResolution": "node",
    "experimentalDecorators": true,
    "jsx": "preserve",
    "jsxFactory": "h"
  }
}
```

## 安装ts-loader

``` bash
npm i -D typescript ts-loader
```

## 安装vue-class-component

``` bash
npm i vue-class-component
```

## 修改webpack.base.conf.js

### entry.app

```javascript
entry: {
  app: './src/main.ts'
}
```

### resolve.extensions

```javascript
resolve: {
  extensions: ['.ts', '.tsx', '.js', '.vue', '.json'],
}
```

### module.rules

```javascript
module: {
  rules: [
    {
      test: /\.ts$/,
      exclude: /node_modules|vue\/src/,
      loader: "ts-loader",
      options: {
        appendTsSuffixTo: [/\.vue$/]
      }
    },
    {
      test: /\.tsx$/,
      exclude: /node_modules|vue\/src/,
      loader: ['babel-loader', 'ts-loader']
    }
  ]
}
```

## 创建sfc.d.ts

```typescript
declare module "*.vue" {
  import Vue from 'vue'
  export default Vue
}
```

## 创建jsx.d.ts

```typescript
import Vue, { VNode } from "vue"

declare global {
  namespace JSX {
    interface Element extends VNode {}
    interface ElementClass extends Vue {}
    interface IntrinsicElements {
      [elem: string]: any
    }
  }
}
```

## 把main.js改为main.ts

在ts文件中引用vue文件时，需要加上后缀.vue

```typescript
import App from './App.vue'
```

## 修改vue文件

script标签修改为`<script lang="ts">`

```typescript
import Vue from 'vue';
import Component from 'vue-class-component';

@Component
export default class Hello extends Vue {
  helloTimes: number = 0;

  sayHello () {
    this.helloTimes++;
  }
}
```

## jsx文件扩展名为tsx

```tsx
import Vue, { CreateElement } from 'vue'
import Component from 'vue-class-component'

@Component
export default class World extends Vue {
  render(h: CreateElement) {
    return <p>This is rendered via TSX</p>
  }
}
```