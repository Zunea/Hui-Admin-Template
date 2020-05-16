# Hui-Admin-Template
## 基础框架简洁版
        
1. 简述：当前是[码云 Hui-Admin-Pro](https://gitee.com/zengxiaohui2019/Hui-Admin-Pro) /
[GitHub Hui-Admin-Pro](https://gitgub.com/zengxiaohui2019/Hui-Admin-Pro) 的母版，也就是基础Template版，只有基础组件和框架
   - Hui-Admin-Pro是基于[基础版 Hui-Admin-Template](https://github.com/zengxiaohui2019/Hui-Admin-Template) 增加更多复杂功能 
   - [Hui-Admin-Pro 在线演示](https://www.zengxiaohui.com/Hui-Admin-Pro)
   - [Hui-Admin-Template 在线演示](https://www.zengxiaohui.com/Hui-Admin-Template)
   - 说明：本人是写这个框架是抱着学习态度，无意冲突商业大佬利益。
   - 兼容说明：初测兼容ie11 360极速模式 谷歌 火狐 其他未测；界面适配笔记本和日常PC；不适配平板和手机。 
2. 主要技术：

        iview4.0 + vuecli3.0
        vue-router路由跳转 (层级嵌套适配、跳转前的拦截、跳转后页面自动滚到顶部)
        vuex管理 (导航高亮、多分页标签切换、根据用户动态菜单路由控制等)
        less flex弹性布局
        
3. 更新日志：
        
        1.1.0 2019-10-03 01:10
            - 历时两天多1小时，每一行代码纯手打。
            - 第1天：VueCli3.0基本环境搭建，插件安装，观摩iview-admin-pro布局搭配，做了基本框架页面布局
            - 第2天：新增导航 多栏目切换 通知下拉功能 路由鉴权
        1.1.1 2019-10-03
            - 新增主题风格切换、多页签、顶栏通顶功能，增加配置文件
        1.1.2 2019-10-04 am
            - 新增刷新页面功能
        2.1.0 2020-05-16
            - 更换登录接口为在线mock
            - 重构了动态路由权限校验，实现根据登录接口返回的权限集合，动态渲染菜单和加载路由
            - 新增 按钮组件级别权限控制

## 启动 

        npm install 安装依赖包
        npm run dev 开发模式运行
        npm run build 打包
        
### 已安装插件
1. axios 0.19.0 // 接口请求控件
2. iview 3.5.1
3. mockjs 1.0.1-beta3 // 数据模拟控件 不用可删除
4. moment 2.24.0 // 时间处理控件 不用可删除
5. vue-router 3.0.3
6. vuex 3.0.1
7. less 3.0.4
8. babel-polyfill 6.26.0 // ie兼容必须

### 目录说明

        Hui-Admin-Template
            node_modules            npm install 后安装的依赖包
            other                   md文档需要的图片
            public                  公共目录
            src                     核心业务目录
                api                 接口请求封装
                    request.js          axios配置和请求返回拦截
                    user.js             登录接口封装
                assets                  图片
                    main                    主框架用到的图片
                components              组件
                    main                    主框架
                    message                 消息通知
                config                  基础配置
                    config.js               基础配置文件
                utils                   工具
                    permission.js           按扭级权限校验
                    util.js                 常用业务文件(判断是否为空 等)
                css                     css
                    public.less             全局公共css
                router                  路由配置
                    permission.js           路由权限校验
                    router.js               路由操作处理文件
                views                   页面
                    tmp.vue                 模板页面 (已包含常用的vue生命周期 可删除)
                store                   vuex目录
            App.vue                 主入口vue
            main.js                 主入口js
            .env.development        开发环境 上下文配置 VUE_APP_BASE_API = '/dev-api'
            .env.production         生产环境 上下文配置 VUE_APP_BASE_API = ''
            babel.config.js         babel配置文件
            package.json            npm包管理文件
            README.md               说明文档
            vue.config.js           开发打包配置文件
        
### 配置说明

1. 基本配置 config->config.js 里面有详细的说明
        
        // 默认系统名称
        export const siteTitle = 'Hui-Admin-Pro 企业级中台前端解决方案'
        // 默认首页
        export const indexPage = 'workplace'
        
        // 主题风格
         export const themeData = {
             themeType: 'dark', // 主题风格配置 dark(经典酷黑) 或者 light(极简雅白) 默认dark经典酷黑
             isTabsShow: true, // 是否显示多页签 默认true
             headMaxWidth: false // 栏目头部是否通顶（最大宽度） 默认false
        }
        
2. 用户数据 在store/modules/user.js
        
        state: {
            // 用户信息数据
           userData: sessionStorage.getItem('userData') ? JSON.parse(sessionStorage.getItem('userData')) : {},
           // 是否登陆
           isLogin: sessionStorage.getItem('userData') ? true : false,
           // 角色权限集合
           roles: []
        }
    
3. 路由鉴权 (根据登录接口返回的权限集合 生产动态路由)

            // 一级栏目
            {
                path: "/form",
                name: "form",
                component: Main,
                meta: {
                    hide: false, // 是否显示
                    title: "Dashboard", // 显示文字
                    icon: "md-speedometer", // 显示图标
                    roleId: 8,// 权限id
                },
                // 二级栏目
                children: [
                    {
                        path: "/form/basic_form",
                        name: "basic-form",
                        meta: {
                            hide: false, // 是否显示
                            title: "基础表单", // 显示文字
                            roleId: 9, // 权限 数组
                        },
                        component: () => import("@/views/form/basic_form")
                    }
                ]
            }
4. 路由权限测试

        测试权限需要登录不同账户
        test账户 账号：test 密码：123456
        admin账户 账号：admin 密码：123456
        
        说明：根据登录接口返回
        admin 拥有1-10的权限[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
        test 拥有6-10的权限[6, 7, 8, 9, 10]
        
### git地址
码云 https://gitee.com/zengxiaohui2019/Hui-Admin-Template

GitHub https://github.com/zengxiaohui2019/Hui-Admin-Template

### 效果

在线演示：https://www.zengxiaohui.com/Hui-Admin-Template
![时尚酷黑主题](other/admin1.jpg)
![极简雅白主题](other/admin2.jpg)

### 支持作者

1. 本软件Hui-Admin-Template，是一个完全免费开源的产品，您可以任意使用，包括修订、发布、出售等。
2. 如果你觉得它给你的项目带来了帮助，提高了开发效率，您可以通过以下的方式来表示你的谢意！
3. 网站程序开发、管理系统、小程序开发请找我，WeiXin：badiweier  昵称：[曾小晖]

4. 使用 支付宝 或 微信 请我喝杯咖啡

![收款码](https://gitee.com/zengxiaohui2019/Hui-Admin-Pro/raw/master/other/skm.jpg)
     
### 免责申明

1. 本软件代码全为曾小晖本人手写，不存在侵犯任何第三方著作权。
2. 禁止使用本软件从事违法犯罪的事情，在使用时产生的任何法律刑事责任于本软件无关 (想不出来能做什么违法的事情，但还是要申明一下)。
3. 由于个人才识技术浅薄，在使用本软件造成的一切损失于本软件无关 (应该不存在，只是一个前端页面而已)。
4. 有问题可联系本人 (大前提：请先百度尝试解决后最终没有找到解决办法)，可付费或免费获得支持。

### 项目使用图片版权说明

1. 本项目使用的图片来源网络，版权归原作者所有
2. 由于没有联系到原作者，若涉及版权问题，请联系本人，予以删除，谢谢
