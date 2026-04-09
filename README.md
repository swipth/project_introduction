# 项目介绍系统（静态 HTML）

基于 `images/` 目录中的系统截图构建的项目介绍页面，支持：

- 顶部导航锚点跳转
- 业务价值指标卡
- 系统总览 + 分系统详情
- 系统关系图与建设里程碑
- 截图画廊点击放大
- 回到顶部与滚动高亮
- 响应式布局（桌面/移动端）

## 目录结构

```text
project_introduction/
├─ index.html
├─ assets/
│  ├─ styles.css
│  └─ app.js
└─ images/
   └─ ...（各系统截图目录）
```

## 本地预览

推荐使用任意静态服务器运行（避免浏览器直接打开 `file://` 带来的路径和安全限制）：

```bash
python -m http.server 8080
```

然后访问：`http://localhost:8080`

## 部署方式（Nginx）

将整个项目目录上传到静态站点根目录，例如 `/var/www/project-introduction`，并配置：

```nginx
server {
    listen 80;
    server_name your-domain.com;

    root /var/www/project-introduction;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

重载 Nginx：

```bash
nginx -t && nginx -s reload
```

## 内容维护说明

- **替换现有截图**：保持图片文件名不变，直接覆盖即可，页面会自动展示新图片内容。
- **新增/删除系统或截图**：需要同步更新 `assets/app.js` 中 `systemsData` 的目录与图片列表。
- **修改文案**：在 `assets/app.js` 的 `copyMap` 中修改系统描述和能力点。

## 可扩展建议

- 若后续需要“新增图片后自动入库而无需改代码”，可增加一个小脚本在部署前自动扫描 `images/` 并生成数据文件。
- 若需要统一品牌风格，可在 `assets/styles.css` 中调整主色、圆角和阴影变量。
