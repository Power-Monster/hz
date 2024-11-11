----------

# <i class="fas fa-sitema😀p"></i><i class="fas fa-sitemap" style="color: #12AA9C"></i>架构设计
设计的基本原则是将事物拆分......以便能够重新组合起来。......将事物分成可以组合的部分，这就是设计😀


   如上所言，系统设计就是将系统分离，以便以后可以重新整合。而且最重要的是，不需要太多成本。我同意这个观点。但我认为架构的另一个目标是**系统的可扩展性**。需求是不断变化的。我们希望程序易于更新和修改以满足新的需求。简洁架构可以帮助实现这一目标，这也是我一直认为架构的意义。

![enter description here](./images/1730969650099.png)
     

### webpack 的技术架构图

![webpack 的技术架构图](./images/1730970601061.png)

### 生产开发过程中数据水管流梳理

 
 *   每一张图应该具有唯一的生产消费源
 *   边界和边界之间是不同的角色
 *   图例应围绕该角色围绕如何消费源, 并生产源, 具有链性

  ![项目中数据源流水线示例](./images/1730971222419.png)
### 项目开发基本规范列举
 

####  1、开发规范

 * 项目命名-全部采用小写方式,以中划线分隔
 * 目录命名-全部采用小写方式，以中划线分隔，有复数结构时，采用复数写法，缩写不用复数
 * 文件命名-全部采用小写形式，以中划线分隔。`React` 组件采用大驼峰写法（除了 `index.tsx`）。组  件命名时需要能够直观的感受出当前组件的用途，组件命名始终是多个单词的，避免跟 `html` 元素或者标签冲突。 类型声明文件后缀为 `.d.ts`，统一放在项目的 `types` 文件夹或各自业务文件夹里面
 * 路由命名-严格与页面文件命名一致。 多级路由嵌套不可超过三层。
 * 命名函数-命名函数遵循 `A/HC/LC` 模式`prefix? + action (A) + high context (HC) + low context? (LC) 前缀 动词/动作 强描述（主要内容） 低描述（辅助内容）`
 * 布尔值类型通常以 `is`、`has`、`can`、`should` 开头，  数组/集合等复数形式通常以 `s`、`List` 结尾
 * 指令推荐都使用缩写形式，用 `:` 表示 `v-bind:`、用 `@` 表示 `v-on:` 和用 `#` 表示 `v-slot:`
 * 组件引用方式，采用 PascalCase 风格，具有较高可读性，同时避免跟现有的以及未来的 HTML 元素相冲突

#### 2、代码校验规范

``` json
{ 
  "scripts": 
	{ "lint-staged": "lint-staged", 
		"prepare": "node prepare-husky.mjs", 
		"prepare-husky-win": "husky install"
		"prepare-husky-mac": "husky install && echo 'PATH=$PATH:'$PATH >>  .husky/_/husky.sh", 
	}, 
	"devDependencies": { 
	  "husky": "^8.0.3", 
	  "lint-staged": "^13.2.3", 
	}, 
	"lint-staged": { 
	  "*.{js,vue}": [ "eslint" ] 
	}, 
}
```

#### 3、技术选型规范

    📝这部分其实算是前端项目的重点吧，毕竟完成业务功能不敲代码是不行的。在已经确定了技术选型和开发流程中的规范之后，这只需要“两成的时间”的工作就是最重点。过程中可能考虑的前后端分离、模块化、组件化、脚手架、组件库、本地开发服务器、 Mock 服务、微前端等等，如果有现成的或者规定好的，那真是太好了
	

##### 开发阶段

1.   脚手架：创建前端应用的目录结构，并生成样板代码
2.   公共库：维护着可复用的 UI 组件、工具模块等公共资源
3.   包管理器：引入第三方库 / 组件，并跟踪管理这些依赖项
4.   编辑器：提供语法高亮、智能提示、引用跳转等功能，提升开发体验
5.   构建工具：提供语法校验、编译、打包、 DevServer 等功能，简化工作流
6.   调试套件：提供预览、DevTools、Mock、性能分析诊断等调试功能，加速修改-验证主循环
7.   ……

##### 测试阶段

 8. [x]   测试阶段 : 开发完成进入测试阶段，先要对整体功能进行充分自测，再移交专业的测试人员验证
 9. [ ]   单元测试框架：提供针对组件、逻辑的测试支持
 10. [ ]   静态扫描工具：从代码质量、构建产物质量、最佳实践 / 开发规约等多个维度做静态检查
 11. [ ]   自动化测试工具：针对 UI 效果和业务流程，提供测试支持
 12. [ ]   性能测试工具：监测并统计出相对准确的性能数据
 13. [ ]   ……

#### 4、构建/部署

**🏁**前端项目构建，其实就是在做依赖打包、文件压缩、代码分割、增量更新与缓存、资源定位、图片处理、 ECMAScript 与 Babel 、 CSS 预编译与 PostCSS 、持续构建和集成、类库打包、构建优化、SSR极限优化等等工作，个人理解，具体流程不详，按照公司统一的规范
   
   每个项目都规定至少有两个确定名的分支dev和master，项目中有带上dockerfile配置文件，之后在合并到指定分支时，就简单 `Gitee Webhook->code check-> make tag->Kubernetes pod(pipeline)`这样子，当然分支不同，合并者的权限不同，编译之后的操作也不同，可能测试区就直接更新了，线上的话就还是需要权责人手动确认

#### 5、监控

  ==前端监控的目的呢？我觉得最主要的目的肯定是为了更快的发现问题和解决问题了，此外还有做产品的决策依据、为业务扩展提供了更多可能性之类的其他作用 #983680==
  
至于监控的重点，不愿意透露姓名的老哥总结的很好，这里拿来用：

> 
> 
> *   页面的性能情况：包括各阶段加载耗时，一些关键性的用户体验指标等
> *   用户的行为情况：包括PV、UV、访问来路，路由跳转等
> *   接口的调用情况：通过http访问的外部接口的成功率、耗时情况等
> *   页面的稳定情况：各种前端异常等
> *   数据上报及优化：如何将监控捕获到的数据优雅的上报
> 

### 项目中的实际应用
  
以信息资源大屏为例。基础框架以1920*1080分辨率为基准，用来保持页面不管放在不同的屏幕中，仍然保持4:3的比例，以避免页面塌陷，样式扭曲
``` javascript
const baseWidth = 1920
const baseHeight = 1080

// * 需保持的比例（默认1.77778）
const baseProportion = parseFloat((baseWidth / baseHeight).toFixed(5))

export default {
  data() {
    return {
      // * 定时函数
      drawTiming: null
    }
  },
  mounted () {
    this.calcRate()
    window.addEventListener('resize', this.resize)
  },
  beforeDestroy () {
    window.removeEventListener('resize', this.resize)
  },
  methods: {
    calcRate () {
      const appRef = this.$refs["appRef"]
      if (!appRef) return 
      // 当前宽高比
      const currentRate = parseFloat((window.innerWidth / window.innerHeight).toFixed(5))
      if (appRef) {
        if (currentRate > baseProportion) {
          // 表示更宽
          scale.width = ((window.innerHeight * baseProportion) / baseWidth).toFixed(5)
          scale.height = (window.innerHeight / baseHeight).toFixed(5)
          appRef.style.transform = `scale(${scale.width}, ${scale.height}) translate(-50%, -50%)`
        } else {
          // 表示更高
          scale.height = ((window.innerWidth / baseProportion) / baseHeight).toFixed(5)
          scale.width = (window.innerWidth / baseWidth).toFixed(5)
          appRef.style.transform = `scale(${scale.width}, ${scale.height}) translate(-50%, -50%)`
        }
      }
    },
```

使用频率最多的echarts图表，也做了适配，选用版本5.5.0，以便后续使用升级可以支持新属性api支持

``` javascript
import { debounce } from '@/utils';
const resizeChartMethod = '$__resizeChartMethod';

export default {
  data() {
    // 在组件内部将图表 init 的引用映射到 chart 属性上
    return {
      chart: null,
    };
  },
  created() {
    window.addEventListener('resize', this[resizeChartMethod], false);
  },
  activated() {
    // 防止 keep-alive 之后图表变形
    if (this.chart) {
      this.chart.resize()
    }
  },
  beforeDestroy() {
    window.removeEventListener('resize', this[resizeChartMethod]);
  },
  methods: {
    // 防抖函数来控制 resize 的频率
    [resizeChartMethod]: debounce(function () {
      if (this.chart) {
        this.chart.resize();
      }
    }, 300),
  },
};
```

#### 项目介绍

##### 一、项目描述

- 一个基于 Vue、Datav、Echart 框架的 " **数据大屏项目** "，通过 Vue 组件实现数据动态刷新渲染，内部图表可实现自由替换。部分图表使用 DataV 自带组件，可进行更改，详情请点击下方 DataV 文档。
[DataV 官方文档](http://datav.jiaminghi.com/guide/)

##### 二、主要文件介绍

| 文件                | 作用/功能                                                              |
| ------------------- | --------------------------------------------------------------------- |
| main.js             | 主目录文件，引入 Echart/DataV 等文件                                   |
| utils               | 工具函数与 mixins 函数等                                               |
| views/ index.vue    | 项目主结构                                                             |
| views/其余文件       | 界面各个区域组件（按照位置来命名）                                     |
| assets              | 静态资源目录，放置 logo 与背景图片                                      |
| assets / style.scss | 通用 CSS 文件，全局项目快捷样式调节                                     |
| assets / index.scss | Index 界面的 CSS 文件                                                  |
| components/echart   | 所有 echart 图表（按照位置来命名）                                      |
| common/...          | 全局封装的 ECharts 和 flexible 插件代码（适配屏幕尺寸，可定制化修改）    |

### Mixins 解决自适应适配功能

使用 mixins 注入解决了界面大小变动图表自适应适配的功能，函数在 `utils/resizeMixins.js` 中，应用在 `common/echart/index.vue` 的封装渲染组件，主要是对 `this.chart` 进行了功能注入。

### 屏幕适配

项目放弃了 flexible 插件方案，将 rem 改回px，使用更流程通用的 `css3：scale` 缩放方案，通过 `ref` 指向 `views/index`，屏幕改变时缩放内容。项目的基准尺寸是 `1920px*1080px`，所以支持同比例屏幕 100% 填充，如果非同比例则会自动计算比例居中填充，不足的部分则留白。实现代码在 `src/utils/userDraw` ，如果有其它的适配方案，期待借鉴
