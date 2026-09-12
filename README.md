# 商品教程中心

一个纯静态的商品图文教程网站。左侧是商品列表，右侧展示该商品对应的教程步骤。
无需任何框架、无需构建、无需服务器，双击 `index.html` 即可运行。

## 目录结构

    tutorial-site/
    ├── index.html          页面结构（HTML）
    ├── css/
    │   └── style.css       全站样式 + 动画（CSS）
    ├── js/
    │   ├── data.js         商品与教程数据（改内容只动这里）
    │   └── app.js          交互逻辑（JS）
    └── README.md           说明文档

## 怎么改成自己的内容

1. 打开 `js/data.js`
2. 修改 `CATEGORIES` 数组：主类目写在最外层，子类目写在该主类的 `children` 中
3. 修改 `PRODUCTS` 数组，每个对象就是该主类的教程模板；子类会自动继承同 `category` 的教程说明：

    {
      id: "唯一ID",              // 用于生成分享链接 ?p=xxx
      name: "商品名称",
      icon: "🔊",                 // 用 emoji 即可
      category: "分类名",         // 必须出现在 CATEGORIES 里
      tag: "热门",                // 可选，会显示成橙色小标签
      color: "#6366f1",           // 该商品的主题色
      desc: "一句话简介",
      keywords: "搜索关键词 空格分隔",
      steps: [
        {
          title: "步骤标题",
          icon: "📦",
          body: [
            { type: "p", text: "段落文字，支持 **加粗** 和 `代码`" },
            { type: "list",  items: ["无序列表项"] },
            { type: "olist", items: ["有序列表项"] },
            { type: "tip",   text: "蓝色提示框" },
            { type: "warn",  text: "黄色警告框" },
            { type: "code",  lang: "bash", text: "代码内容" },
            { type: "img",   src: "图片地址", caption: "图注" },
            { type: "table", head: ["列1","列2"], rows: [["值1","值2"]] }
          ]
        }
      ]
    }

## 新增、修改、删除子类目

子类目只在 `js/data.js` 的 `CATEGORIES[].children` 中维护，例如：

    {
      value: "手持云台相机",
      label: "手持云台相机",
      icon: "🎥", // 下方大目录图标
      children: [
        { id: "gimbal-phone", value: "手机云台版", label: "手机云台版", icon: "📱" },
        { id: "gimbal-pro",   value: "专业跟拍版", label: "专业跟拍版", icon: "🎬" }
      ]
    }

- 新增：向对应 `children` 数组增加一个对象
- 改名：修改子类对象的 `label`（页面显示名称）
- 删除：从对应 `children` 数组移除该对象
- 删除整个主类：从 `CATEGORIES` 移除该主类对象（对应的 `PRODUCTS` 模板也可一并删除）
- 下方大目录图标由 `CATEGORIES[].icon` 控制，可按类目名称自行替换
- 子类会自动继承对应 `PRODUCTS.category` 的 `desc`、`keywords` 和 `steps`
- 如需子类单独显示不同说明，可在子类对象中填写 `desc`、`keywords` 或 `steps`
- `id` 用于分享链接，创建后建议不要再随意修改

### 目录展开与收纳

- 点击主类名称：筛选该主类，并自动展开它下面的子类
- 点击主类右侧箭头：只展开或收纳该主类，不改变当前筛选
- 点击「全部展开」：展开所有主类的子类
- 点击「全部收纳」：收起所有主类的子类，只保留主类按钮
- 下方教程列表按主类分组，子类教程卡片收纳在对应主类中
- 上方分类开关与下方主目录开关状态同步
- 点击教程或搜索结果时，对应主目录会自动展开

## 已有功能

- 商品列表 + 主类/子类折叠筛选 + 关键词搜索
- 点击商品查看教程，卡片有涟漪与上浮动画
- 教程步骤可折叠展开，带过渡动画
- 代码块一键复制
- 深浅主题切换（记忆在 localStorage）
- 顶部滚动进度条、回到顶部按钮
- 移动端抽屉式侧栏
- 支持 ?p=商品id 直接分享某个教程

## 部署

直接把整个 tutorial-site 文件夹上传到任意静态托管即可：
GitHub Pages、Vercel、Netlify、对象存储、虚拟主机等均可。

## 关于 index.html 里的 </script>

打包时，内嵌的 index.html 中原本的 script 结束标签被写成了 </script>，
下载时会自动还原成正常标签。如果你手动复制 index.html 内容，请把这两处
</script> 改回标准的 script 结束标签即可。
