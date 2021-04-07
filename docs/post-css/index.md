### postCss
    postCss 是一个允许使用js插件转换样式的工具
      插件功能： 
        - 检查css
        - 使用未来版本的css（can i use）, 配置浏览器厂商前缀 Autoprefixer
### px2VW
  ## postcssrc.js
      module.exports = {
        'plugins': {
          'postcss-import': {}, // 用于@import导入css文件
          'postcss-url': {}, // 路径引入css文件或node_modules文件
          'postcss-aspect-ratio-mini': {}, // 用来处理元素容器宽高比
          'postcss-write-svg': { utf8: false }, // 用来处理移动端1px的解决方案。该插件主要使用的是border-image和    background来做1px的相关处理。
          'postcss-cssnext': {}, // 该插件可以让我们使用CSS未来的特性，其会对这些特性做相关的兼容性处理。
          'postcss-px-to-viewport': { // 把px单位转换为vw、vh、vmin或者vmax这样的视窗单位，也是vw适配方案的核心插件之一。
            'viewportWidth': 750, // 视窗的宽度 ，对应的是我们设计稿的宽度，一般是750
            'viewportHeight': 1334, // 视窗的高度 根据750设备的宽度来指定，一般指定1334，也可以不配置
            'unitPrecision': 4, // 将px转化为视窗单位值的小数位数
            'viewportUnit': 'vw', // 指定要转换成的视窗单位值
            'selectorBlackList': ['.ignore', '.hairlines'], // 指定不转换视窗单位值得类，可以自定义，可以无限添加
            'minPixelValue': 1, // 小于等于1px不转换为视窗单位
            'mediaQuery': false, // 允许在媒体查询中使用px
          },
          'postcss-viewport-units': {}, // 给vw、vh、vmin和vmax做适配的操作,这是实现vw布局必不可少的一个插件
          'cssnano': { // 主要用来压缩和清理CSS代码。在Webpack中，cssnano和css-loader捆绑在一起，所以不需要自己加载它。
            'preset': 'advanced', // 重复调用
            'autoprefixer': false, // cssnext和cssnano都具有autoprefixer,事实上只需要一个，所以把默认的autoprefixer删除掉，然后把cssnano中的autoprefixer设置为false。
            'postcss-zindex': false // 只要启用了这个插件，z-index的值就会重置为1
          }
        }
      }

官方地址 (http://github.com/michael-ciniawsky/postcss-load-config)

### h5页面布局适配问题
  flexible 布局 为了让页面适配各种不同的终端， 
  通过hack的手段根据设备的dpr值相应更改<meta>标签的viewport值,
  从而让页面实现缩放的效果。
    - 通过修改viewport值 实现1px的线
    - 通过修改html 的fontsize 实现 rem 的等比缩放
    - 使用hack 手段用 rem 模拟vw 特性
  
  vw布局 vw已经得到了众多浏览器的支持，可以将vw单位直接运用在我们的适配布局中
  我们的设计图通常是750px==100vw, 所以1vw = 7.5px
  通过插件解决 1px问题 盒子宽高比问题 兼容处理问题
  参阅资料：https://www.w3cplus.com/css/vw-for-layout.html