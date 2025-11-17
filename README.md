# TG发卡机器人搭建/电报发卡机器人源码/telegram直登号发卡机器人tdata+session号铺搭建/支持USDT+支付宝/WEB后台管理
# Telegram自动发卡机器人 (已修复版本)
查看视频教程 https://www.youtube.com/watch?v=k4rizxoEnC8

版本: 2.4.3
更新日期: 2025-11-15
技术支持: @niu444

## 📝 本版本修复内容

### 1. 商品管理修复
- ✅ 修复了创建分类后不显示的问题
- ✅ 修复了删除分类不同步的问题
- ✅ 修复了更新分类名称不同步的问题
- ✅ 添加国家时自动创建分类

### 2. Telegram按钮修复
- ✅ 修复了Web管理按钮点击无反应的问题
- ✅ 添加了正确的WEB_ADMIN_URL配置
- ✅ 使用服务器真实IP代替localhost

### 3. 用户管理修复
- ✅ 修复了用户管理不显示用户的问题
- ✅ 修复了消费排名和余额排名按钮无反应
- ✅ 添加了详细的调试日志

### 4. 前端构建优化
- ✅ 自动清除旧的bundle文件
- ✅ 确保每次安装重新构建
- ✅ 验证构建结果

### 5. USDT支付配置修复 (v2.4.1)
- ✅ 修复了Telegram机器人中配置USDT支付后重启丢失的问题
- ✅ 修复了USDT接口地址和密钥无法持久化保存到config.js
- ✅ 修复了TokenPay配置不实时生效的问题
- ✅ 确保配置修改后立即生效,无需重启

### 6. 客服设置修复 (v2.4.2)
- ✅ 修复了客服设置重启后丢失的问题
- ✅ 修复了配置项名称错误导致无法保存
- ✅ 添加了配置重载机制
- ✅ 统一了通知频道设置的持久化逻辑

### 7. 易支付(支付宝)修复 (v2.4.3)
- ✅ 修复了二维码生成错误问题
- ✅ 修正了签名算法 - 密钥拼接方式错误
- ✅ 使用正确的API接口(mapi.php)
- ✅ 添加必需参数clientip
- ✅ 正确处理API返回结果

## 🚀 快速安装

### 系统要求
- Ubuntu 18.04+ / Debian 9+ / CentOS 7+
- Node.js 18.x (脚本会自动安装)
- 至少 1GB 内存
- 至少 10GB 磁盘空间

### 安装步骤

1. **下载安装包**
```bash
# 如果您有安装包文件
unzip tgbot-fixed-package.zip
cd tgbot-fixed-package
```

2. **赋予执行权限**
```bash
chmod +x install-tgbot.sh
```

3. **运行安装脚本**
```bash
sudo bash install-tgbot.sh
```

4. **按提示输入信息**
- Telegram Bot Token (从 @BotFather 获取)
- 管理员 Telegram ID
- 服务器IP会自动获取

5. **等待安装完成**

## 🌐 Web管理后台

### 访问地址
```
http://您的服务器IP:3000
```

### 默认登录信息
- 账号: `admin`
- 密码: `admin123`

### 首次访问注意
1. 请使用 **Ctrl+Shift+R** (Windows) 或 **Cmd+Shift+R** (Mac) 强制刷新浏览器
2. 如果仍有问题，清除浏览器缓存后重新访问
3. 登录后建议立即修改密码

## 📋 常用命令

### PM2 管理命令
```bash
# 查看服务状态
pm2 status

# 查看实时日志
pm2 logs tgbot

# 重启机器人
pm2 restart tgbot

# 重启Web管理
pm2 restart tgbot-web

# 停止服务
pm2 stop tgbot tgbot-web

# 删除服务
pm2 delete tgbot tgbot-web
pm2 save
```

### 配置文件
```bash
# 编辑配置
nano /srv/tgbot/config.js

# 编辑后重启服务
pm2 restart tgbot
```

### 数据库管理
```bash
# 备份数据库
cp /srv/tgbot/database.sqlite /tmp/database-backup-$(date +%Y%m%d).sqlite

# 查看数据库
sqlite3 /srv/tgbot/database.sqlite
```

## 🔧 升级说明

如果您已经安装了旧版本：

1. **运行新的安装脚本**
```bash
sudo bash install-tgbot.sh
```

2. **脚本会自动：**
   - 停止当前运行的服务
   - 备份配置文件到 `/tmp/tgbot-config-backup-时间戳.js`
   - 备份数据库到 `/tmp/tgbot-database-backup-时间戳.sqlite`
   - 备份商品数据到 `/tmp/tgbot-tdata-backup-时间戳.tar.gz`

3. **询问是否使用旧配置**
   - 选择 `y` 使用旧配置（推荐）
   - 选择 `n` 创建新配置

4. **脚本会自动更新配置文件，添加Web管理配置**

## ⚠️ 重要提醒

### 必须配置项
在 `/srv/tgbot/config.js` 中配置：

1. **TokenPay 配置** (如果使用USDT支付)
```javascript
TOKENPAY_URL: 'https://your-tokenpay-url',
TOKENPAY_KEY: 'your_tokenpay_key',
```

2. **机器人用户名** (从 @BotFather 获取)
```javascript
BOT_USERNAME: 'your_bot',
```

3. **联系方式**
```javascript
CUSTOMER_SERVICE: '@your_customer_service',
NOTIFICATION_CHANNEL: '@your_notification_channel',
```

### 添加商品
1. 商品文件放在 `/srv/tgbot/tdata/` 目录
2. 目录结构: `tdata/分类名/国家名/账号文件夹/`
3. 详细说明参考原项目文档

### 防火墙设置
```bash
# Ubuntu/Debian (使用ufw)
sudo ufw allow 3000/tcp
sudo ufw allow 3001/tcp

# CentOS/RHEL (使用firewalld)
sudo firewall-cmd --permanent --add-port=3000/tcp
sudo firewall-cmd --permanent --add-port=3001/tcp
sudo firewall-cmd --reload
```

## 🐛 故障排除

### 问题1: Web管理打不开
```bash
# 检查服务状态
pm2 status

# 检查端口是否监听
netstat -tlnp | grep 3000

# 查看日志
pm2 logs tgbot-web

# 重启服务
pm2 restart tgbot-web
```

### 问题2: 用户管理不显示用户
1. 按 F12 打开浏览器开发者工具
2. 查看 Console 标签页是否有错误
3. 按 Ctrl+Shift+R 强制刷新
4. 清除浏览器缓存

### 问题3: 商品分类不显示
1. 检查是否正确添加了分类
2. 查看数据库:
```bash
sqlite3 /srv/tgbot/database.sqlite "SELECT * FROM category_orders;"
```
3. 如果为空，说明需要手动创建分类

### 问题4: Telegram按钮无反应
1. 检查配置文件中的 `WEB_ADMIN_URL` 是否正确
2. 必须使用服务器真实IP，不能用localhost
3. 修改后重启机器人:
```bash
pm2 restart tgbot
```

### 问题5: USDT支付配置后重启丢失 (已修复 v2.4.1)
如果您使用的是v2.4.1或更高版本:
1. 配置会自动保存到config.js文件
2. 重启后配置会自动加载
3. 无需手动编辑配置文件

如果仍然遇到问题:
1. 检查config.js文件是否有写权限:
```bash
ls -la /srv/tgbot/config.js
```
2. 查看机器人日志确认配置是否成功:
```bash
pm2 logs tgbot --lines 50
```

## 📞 技术支持

如遇到问题，请联系:
- Telegram: @niu444
- 提供问题时请附上:
  - 错误日志 (`pm2 logs tgbot`)
  - 系统版本 (`cat /etc/os-release`)
  - Node.js版本 (`node --version`)

## 📄 许可证

本项目仅供学习交流使用

## 🔄 更新日志

### v2.4.3 (2025-11-15)
- **修复易支付(支付宝)二维码生成错误** - 签名算法和接口调用错误
- 修正MD5签名算法 - 密钥拼接方式
- 使用mapi.php API接口替代submit.php
- 添加必需参数clientip和device
- 正确处理返回的qrcode/payurl

### v2.4.2 (2025-11-15)
- **修复客服设置重启后丢失问题** - 配置项名称错误和缺少重载机制
- 修复客服设置配置项从CUSTOMER_SERVICE_HANDLE改为CUSTOMER_SERVICE
- 添加客服和通知频道设置的配置重载机制
- 统一所有机器人设置的持久化逻辑

### v2.4.1 (2025-11-15)
- **修复USDT支付配置丢失问题** - 通过Telegram机器人配置USDT后重启不再丢失
- 修复USDT接口地址和密钥无法持久化到config.js的问题
- 修复TokenPay配置不实时生效的问题
- 优化支付宝配置持久化机制

### v2.4.0 (2025-11-12)
- 修复商品管理创建分类问题
- 修复Telegram Web管理按钮问题
- 修复用户管理显示问题
- 优化前端构建流程
- 添加详细的调试日志

### v2.3.0 (之前版本)
- 基础功能实现
