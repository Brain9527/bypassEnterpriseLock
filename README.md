# Mac 免费完美绕过监管锁 (MDM Bypass)

这是一个基于 Vue 3 + Vite + TypeScript 构建的静态教程页面，旨在帮助用户绕过苹果 Mac 电脑的远程管理锁（MDM）。

## 🚀 项目特点

- **Vue 3 + TS**: 使用最新的前端技术栈构建。
- **Tailwind CSS**: 响应式布局，完美适配移动端和桌面端。
- **PrismJS**: 专业的终端代码语法高亮。
- **一键复制**: 核心脚本和清理命令均支持一键复制，并配有成功提示气泡。
- **GitHub Pages 部署**: 已配置完善的 GitHub Actions，支持自动化构建与部署。

## 🛠️ 本地开发

### 环境要求
- **Node.js**: >= 24
- **npm**: 最新版本

### 启动步骤
1. 克隆或下载本项目。
2. 安装依赖：
   ```bash
   npm install
   ```
3. 启动开发服务器：
   ```bash
   npm run dev
   ```
4. 访问地址：`http://localhost:5173`

### 项目构建
生成生产环境静态文件：
```bash
npm run build
```
构建产物将存放在 `dist` 目录。

## 🌐 部署说明

项目已配置 GitHub Actions 自动部署脚本：
1. 将代码推送到 GitHub 仓库的 `main` 分支。
2. 在 GitHub 仓库设置中开启 Pages 功能，并选择 **GitHub Actions** 作为部署源。
3. 访问 `https://<your-username>.github.io/<repo-name>/` 即可查看。

## 📝 免责声明

本页面及相关代码仅供学习交流使用，请支持正版软件并从合法渠道购买苹果设备。

---

如果本项目对你有帮助，欢迎点个 Star！⭐️
