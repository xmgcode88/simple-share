# SimpleShare - 简享

一个基于多租户架构的现代化内容分享平台，支持SaaS模式部署，为企业和组织提供专属的内容分享服务。

## 📋 目录

- [项目概述](#项目概述)
- [技术架构](#技术架构)
- [核心功能](#核心功能)
- [项目结构](#项目结构)
- [快速开始](#快速开始)
- [本地开发](#本地开发)
- [部署指南](#部署指南)
- [环境配置](#环境配置)
- [API文档](#api文档)
- [贡献指南](#贡献指南)
- [许可证](#许可证)



## 🎯 项目概述

SimpleShare是一个多租户（SaaS）化的内容分享平台，具有以下核心特性：

- **多租户架构**：支持多个独立租户，数据严格隔离，通过域名区分不同租户
- **集中式安全策略**：租户管理员可全局控制内容复制权限和水印设置
- **现代化界面**：基于Vue 3 + Element Plus的响应式设计
- **强大的编辑器**：集成TipTap富文本编辑器，支持Markdown和富文本
- **灵活的权限系统**：基于角色的访问控制（RBAC）

## 项目预览
|  |  |
| ---- | ---- |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/1.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/2.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/3.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/4.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/5.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/6.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/7.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/8.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/9.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/10.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/11.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/12.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/13.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/14.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/15.png" width="80%"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/16.png" width="80%"> |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/17.png" width="80%"> |  |


## 🏗️ 技术架构

### 后端技术栈
- **框架**：Spring Boot 2.7.18
- **安全**：Spring Security + JWT
- **数据库**：MySQL 8.0 + MyBatis-Plus + Druid
- **构建工具**：Maven
- **API文档**：Swagger + Knife4j
- **多租户**：基于`tenant_id`字段的逻辑隔离

### 前端技术栈
- **框架**：Vue 3.5.18 + TypeScript
- **构建工具**：Vite 7.0.6
- **UI组件**：Element Plus 2.11.2
- **状态管理**：Pinia 3.0.3
- **路由**：Vue Router 4.5.1
- **样式**：Tailwind CSS 3.4.17
- **编辑器**：TipTap 3.4.4 + Vditor

### 部署架构
- **Web服务器**：Nginx
- **应用服务器**：内嵌Tomcat（Spring Boot）
- **数据库**：MySQL
- **文件存储**：本地文件系统

## ✨ 核心功能

### 租户管理
- 租户创建和配置
- 域名绑定和访问控制
- 租户数据隔离
- 租户级别配置管理

### 用户管理
- 用户注册和登录
- 基于角色的权限控制
- 用户资料管理
- 两步验证支持

### 内容管理
- 富文本文章编辑
- Markdown支持
- 文章分类和标签
- 访问级别控制（公开/会员/隐私）
- 文章审核流程

### 安全策略
- 全局复制权限控制
- 水印功能
- IP黑白名单
- 登录失败锁定
- 操作日志记录

### 其他功能
- 文章收藏
- 评论系统
- 点赞和统计
- SEO优化
- 响应式设计

## 📁 项目结构

```
simple-share/
├── backend/                          # 后端项目
│   ├── simpleshare-common/           # 通用模块
│   ├── simpleshare-framework/        # 核心框架模块
│   ├── simpleshare-module-system/    # 系统管理模块
│   ├── simpleshare-module-article/   # 文章管理模块
│   ├── simpleshare-module-infra/     # 基础设施模块
│   ├── simpleshare-server/           # 启动模块
│   ├── sql/                          # 数据库脚本
│   ├── nginx202512/                  # Nginx配置
│   └── pom.xml                       # 父POM文件
├── frontend/                         # 前端项目
│   ├── src/
│   │   ├── components/               # 通用组件
│   │   ├── views/                    # 页面组件
│   │   │   ├── portal/               # 用户端页面
│   │   │   └── admin/                # 管理后台页面
│   │   ├── api/                      # API接口
│   │   ├── stores/                   # 状态管理
│   │   ├── router/                   # 路由配置
│   │   └── utils/                    # 工具函数
│   ├── public/                       # 静态资源
│   ├── package.json                  # 依赖配置
│   └── vite.config.ts               # Vite配置
├── 需求文档.md                       # 需求文档
└── README.md                         # 项目说明
```

## 🚀 快速开始

### 环境要求
- **Node.js**：^20.19.0 || >=22.12.0
- **Java**：JDK 8+
- **Maven**：3.6+
- **MySQL**：8.0+
- **Nginx**：1.18+

### 1. 克隆项目
```bash
git clone https://gitee.com/GreenSoftware/simple-share.git
cd simple-share
```

### 2. 数据库初始化
```bash
# 创建数据库
mysql -u root -p
CREATE DATABASE `simple-share` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# 导入初始化脚本
mysql -u root -p simple-share < backend/sql/init.sql
```

### 3. 后端启动
```bash
cd backend
mvn clean package
cd simpleshare-server
java -jar target/simpleshare-server-1.0.0.jar
```

### 4. 前端启动
```bash
cd frontend
npm install
npm run dev
```

## 💻 本地开发

### 后端开发环境配置

1. **IDE配置**
   - 推荐使用IntelliJ IDEA
   - 安装Lombok插件
   - 配置Maven设置

2. **数据库配置**
   ```yaml
   # application.yml
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/simple-share?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
       username: root
       password: your_password
   ```

3. **启动类**
   ```java
   // SimpleShareApplication.java
   @SpringBootApplication
   public class SimpleShareApplication {
       public static void main(String[] args) {
           SpringApplication.run(SimpleShareApplication.class, args);
       }
   }
   ```

### 前端开发环境配置

1. **环境变量配置**
   ```bash
   # .env.development
   VITE_API_BASE_URL=http://localhost:8080
   VITE_PROXY_TARGET=http://localhost:8080
   ```

2. **开发命令**
   ```bash
   # 安装依赖
   npm install
   
   # 启动开发服务器
   npm run dev
   
   # 构建生产版本
   npm run build
   
   # 代码检查
   npm run lint
   
   # 格式化代码
   npm run format
   ```

3. **API代理配置**
   ```typescript
   // vite.config.ts
   server: {
     proxy: {
       '/api': {
         target: 'http://localhost:8080',
         changeOrigin: true,
         rewrite: (path) => path.replace(/^\/api/, '')
       }
     }
   }
   ```

## 🚀 部署指南

### 生产环境部署


#### 1. 后端部署
```bash
# 构建项目
cd backend
mvn clean package -Dmaven.test.skip=true

# 上传jar包到服务器
scp backend/simpleshare-server/target/simpleshare-server-1.0.0.jar user@server:/opt/simpleshare/

# 启动服务
nohup java -jar -Dspring.profiles.active=prod /opt/simpleshare/simpleshare-server-1.0.0.jar > /opt/simpleshare/logs/app.log 2>&1 &
```

#### 2. 前端部署
```bash
# 构建前端
cd frontend
npm run build

# 上传dist目录到服务器
scp -r dist/* user@server:/opt/simpleshare/web/dist/
```

#### 3. Nginx配置
```bash
# 复制Nginx配置文件
sudo cp backend/nginx202512/nginx.conf /etc/nginx/sites-available/simpleshare
sudo ln -s /etc/nginx/sites-available/simpleshare /etc/nginx/sites-enabled/

# 测试配置
sudo nginx -t

# 重启Nginx
sudo systemctl restart nginx
```


## ⚙️ 环境配置

### 后端配置项

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `spring.datasource.url` | 数据库连接地址 | - |
| `spring.datasource.username` | 数据库用户名 | - |
| `spring.datasource.password` | 数据库密码 | - |
| `server.port` | 服务端口 | 8080 |
| `jwt.secret` | JWT密钥 | - |
| `jwt.expiration` | JWT过期时间 | 86400秒 |

### 前端配置项

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `VITE_APP_TITLE` | 应用标题 | 简享 |
| `VITE_API_BASE_URL` | API基础地址 | /api |
| `VITE_APP_WATERMARK_TEXT` | 水印文本 | 简享 |
| `VITE_ENABLE_USER_REGISTRATION` | 是否允许用户注册 | true |

## 📚 API文档

### 认证接口
```
POST /api/auth/login          # 用户登录
POST /api/auth/register       # 用户注册
POST /api/auth/logout         # 用户登出
POST /api/auth/refresh        # 刷新Token
```

### 文章接口
```
GET    /api/articles          # 获取文章列表
POST   /api/articles          # 创建文章
GET    /api/articles/{id}     # 获取文章详情
PUT    /api/articles/{id}     # 更新文章
DELETE /api/articles/{id}     # 删除文章
```

### 系统管理接口
```
GET /api/system/users         # 获取用户列表
GET /api/system/roles         # 获取角色列表
GET /api/system/menus         # 获取菜单列表
```

完整的API文档可通过Swagger UI访问：
```
http://localhost:8080/doc.html
```

## 🤝 贡献指南

1. Fork 项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

### 代码规范
- 后端遵循阿里巴巴Java开发手册
- 前端遵循ESLint配置
- 提交信息遵循Conventional Commits规范

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

---


## 📞 联系方式

### 交流群汇总：
**微信号：xmgcode、xmgcode88**

| xmgcode | xmgcode88 | QQ群 |
| :---: | :---: | :---: |
| <img src="https://github.com/xmgcode88/simple-share/blob/main/images/xmgcode.png" width="200" title="微信1：xmgocde"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/xmgcode88.png" width="200" title="微信2：xmgocde88"> | <img src="https://github.com/xmgcode88/simple-share/blob/main/images/990035173.png" width="200" title="QQ群二维码"> |



**© 2025 小码哥. 保留所有权利.**
