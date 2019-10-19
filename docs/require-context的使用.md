# require.context

可用于自动注册全局组件

```js
// autoRegister.js
import Vue from 'vue';

// 不需要自动注册的组件
const blackList = ['Toast'];

const requireComponent = require.context('components', true, /\.vue$/);

requireComponent.keys().forEach(filename => {
    const componentConfig = requireComponent(filename);
    //   const start = filename.lastIndexOf('/') + 1;
    //   const end = filename.lastIndexOf('.');
    //   const componentName = filename.slice(start, end);
    const componentName = filename.replace(/\.\/|\.vue$/g, '');
    if (blackList.includes(filename)) {return;}
    // 全局注册组件
    Vue.component(
        componentName,
        // 如果这个组件选项是通过 `export default` 导出的，
        // 那么就会优先使用 `.default`，
        // 否则回退到使用模块的根。
        componentConfig.default || componentConfig
    );
});
```

> 具体可以参考src\router\router.js
