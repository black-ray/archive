```bash
# production 开发模式，development 生产模式
model: production | development

.env																# 所有情况下都会加载
.env.local													# 所有情况下都会加载，但会被 git 忽略
.env.[mode]													# 只有在指定模式下才会加载
.env.[model].local									# 只有在指定模式下才会加载，但会被 git 忽略
```

 默认只有以 `VITE_` 为前缀的变量才会暴露出去，给 vite 处理

```bash
# .env.production
VITE_BASE_API = '/prod-api'
```

```bash
# .env.development
VITE_BASE_API = '/api'
```

访问环境变量

```javascript
console.log(import.meta.env.VITE_BASE_API)
```

```javascript
const service = axios.create({
  baseURL: import.meta.env.VITE_BASE_API,
  timeout: 5000
})
```



## 初始化 rem 基准值

```javascript
export const useREM = () => {
  // 定义最大的 fontSize
  const MAX_FONT_SIZE = 40
  // 监听 html 文档被解析完成的事件
  document.addEventListener('DOMContentLoaded', () => {
    // 获取 html 标签
    const html = document.querySelector('html')
    // 获取根元素 fontSize 标准，屏幕宽度 / 10。（以 Iphone 为例 Iphone 6 屏幕宽度为 375，则标准 fontSize 为 37.5）
    let fontSize = window.innerWidth / 10
    // 获取到的 fontSize 不允许超过我们定义的最大值
    fontSize = fontSize > MAX_FONT_SIZE ? MAX_FONT_SIZE : fontSize
    // 定义根元素（html）fontSize 的大小 （rem）
    html.style.fontSize = fontSize + 'px'
  })
}
```

main.js 中导入，初始化时执行

```javascript
import { useREM } from './utils/flexible'
// 设置 rem
useREM()
createApp(App).use(...)
```



修改预设值

```javascript
module.exports = {
  // 手动切换暗模式
  darkMode: 'class',
  // Tailwind 应用范围
  content: ['./index.html', './src/**/*.{vue,js}'],
  theme: {
    extend: {
      height: {
        header: '72px',
        main: 'calc(100vh - 72px)'
      },
      fontSize: {
        // 重新设置 font-size 对应的大小
        // [font-size, line-height]
        xs: ['0.25rem', '0.35rem'],
        sm: ['0.35rem', '0.45rem'],
        base: ['0.42rem', '0.52rem'],
        lg: ['0.55rem', '0.65rem'],
        xl: ['0.65rem', '0.75rem']
      },
      boxShadow: {
        // 新增样式
        'l-white': '-10px 0 10px white',
        'l-zinc': '-10px 0 10px #18181b'
      },
      colors: {
        main: '#f44c58',
        'hover-main': '#f32836',
        'success-100': '#F2F9EC',
        'success-200': '#E4F2DB',
        'success-300': '#7EC050',
        'warn-100': '#FCF6ED',
        'warn-200': '#F8ECDA',
        'warn-300': '#DCA550',
        'error-100': '#ED7456',
        'error-200': '#f3471c',
        'error-300': '#ffffff'
      },
      backdropBlur: {
        '4xl': '240px'
      },
      variants: {
        scrollbar: ['dark']
      }
    }
  },
  plugins: [require('tailwind-scrollbar')]
}
```



- 使用 Vite 的 **vite-plugin-svg-icons** 处理 svg 图标

  ```bash
  npm i vite-plugin-svg-icons -D
  ```

- vite.config.ts 文件中添加配置

  ```javascript
  import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'
  export default defineConfig({
    plugins: [
      vue(),
      createSvgIconsPlugin({
        // 指定需要缓存的图标文件夹
        iconDirs: [path.resolve(process.cwd(), 'src/assets/icons')],
        // 指定symbolId格式
        symbolId: 'icon-[name]'
      })
    ],
  })
  
  ```

