# v-circle
演示地址 https://pxs623.github.io/vcircle.html

```

## 使用

```js
// ES6
import vuePayKeyboard from 'v-circle.js'
// require
var vCircle = require('vCircle')

Vue.use(vCircle)

// 或者直接使用script导入
<script src="./node_modules/vue/dist/v-circle.js"></script>

// 作为组件的方式使用
    <v-circle color="#6DE19F" border-color="#6DE19F" text-color="#6DE19F" width=80 font-size=12 :pv='pv' desc="响应延迟" bold=6 text-bg-color='#ffffff'></v-circle>
