# Claude 注册、订阅与 KYC 防封号完全指南

> 整理自实战经验，涵盖从零开始注册 Claude 账号、订阅 Max 套餐、通过 KYC 认证的完整流程。
> 所有步骤均为真实操作记录，不承诺 100% 防封，仅供参考。

---

## 前置硬性门槛

在开始之前，确认你具备以下条件：

| 必需 | 说明 |
|------|------|
| 护照 | 用于支付绑定和 KYC 实名认证 |
| Mac 电脑 | Windows 配置方式不同，本文不涉及 |
| Codex（本地 Agent） | 代替手动执行清理和配置脚本 |
| Shadowrocket（小火箭） | 保证 Mac 和 iPhone 分流规则一致，需 $2.99 购买 |
| iPhone（可选） | 如需在手机端使用 Claude App |

---

## 一、在 iPhone 上注册全新 Gmail 账号

在 iPhone 上下载 **Chrome 浏览器**，然后注册一个新 Gmail 账号：

> 右上角头像 → 添加账号 → 创建账号

**注意**：用电脑注册 Gmail 可能触发手机号验证，而大陆 +86 手机号若后续与 Claude 绑定的号码不一致，存在一定风险。建议全程在 iPhone 上操作，通常无需手机号验证。

---

## 二、注册美区 Apple ID，购买 Shadowrocket

Shadowrocket 只在美区 App Store 上架，需要美区 Apple ID 才能搜索并购买。

**第一步：注册免费美区 Apple ID**

访问 [https://account.apple.com/account](https://account.apple.com/account)，注册时地区选择 United States，填写真实存在的美国地址（免税州地址，后面可用 Google 地图或 ChatGPT 生成）。

**第二步：充值礼品卡购买 Shadowrocket**

由于没有美国银行卡，需要通过支付宝购买礼品卡：

1. 支付宝搜索【Nidiya 礼品卡】
2. 选择 **Apple Gift Card US**，充值 $3 到美区 Apple ID 绑定的邮箱
3. 用这个美区 Apple ID 分别登录 **Mac 和 iPhone 的 App Store**
4. 搜索并购买 Shadowrocket（$2.99）

---

## 三、自备科学上网工具

**这是整个流程的核心**。Claude 封号，大多数情况下与网络环境不稳定有关，因此建议使用稳定的科学上网服务，尽量选择干净的静态住宅 IP（优质 IP 段）。

**关于科学上网工具的选择，有两点核心建议：**

- **IP 类型优先住宅 IP**：机房 IP 容易被 Claude 风控识别，静态住宅 IP（优质 IP 段）稳定性更好，封号概率更低。
- **固定节点，避免频繁切换**：每次换 IP 都会增加被关联风控的风险，使用独享、固定的节点是关键。

科学上网服务需自行准备，建议选择提供静态独享住宅 IP 的服务商。准备好科学上网服务后，将获得的订阅链接导入 Shadowrocket（点击右上角 `+` → 导入链接）。不熟悉操作的可以让 Codex 帮忙完成。

---

## 四、清理旧的 Claude 登录与转发配置

历史遗留的旧配置（缓存、本地 ID、第三方转发）是账号被关联封禁的重要原因之一。此步骤建议通过 Codex 执行，避免手动改错。

### 第一步：审计现有配置

退出 Claude 桌面应用和 Claude Code，Codex 保持打开，发送以下提示词：

> ⚠️ 提示词中包含 GitHub 链接，GitHub 在国内需科学上网方可访问，执行前请确保已开启。

```
我准备使用官方Claude账号，请检查这台Mac上的Claude Code和Claude Desktop旧配置。

先获取并审查这份claude-cleanup Skill，参考它的白名单和保护范围做只读审计，暂不执行清理脚本：
https://github.com/suyuan2022/suyuan-skill/blob/423473db6b618c7e040f1a5a6879bc83e69e8b3b/claude-cleanup/SKILL.md
```

### 第二步：执行清理

确认审计清单后，发送以下提示词给 Codex：

```
按刚才列明的清单执行。先在本机完整备份相关配置，正常退出需要退出的Claude进程，再清理已确认的旧登录、账号缓存和缓存日志，重置已确认的本地标识，移除Claude Code的旧第三方转发配置。

文件只移入废纸篓；钥匙串只删除刚才明确列出的Claude记录。保留其余数据和配置，修改后逐项核验。如果切换供应商时误清了其他设置，请从备份精确恢复。

完成后告诉我：处理结果、备份位置，以及需要重新打开哪些应用。
```

> 如脚本需要在终端输入 `CONFIRM`，按提示手动输入即可。

**说明**：默认流程保留桌面端数据，若希望同时清理桌面端应用本身，在列清单时额外说明即可。

---

## 五、配置 Shadowrocket 分流规则

**目标**：确保 Mac 和 iPhone 访问 Claude 时，始终走同一个固定节点，避免因 IP 跳变触发风控。

### Shadowrocket 基础设置

| 选项 | 设置值 |
|------|--------|
| 设置 → 代理 → 代理类型 | `None（TUN Mode）` |
| 全局路由 | `配置` |
| 设置 → 隧道 → 包括所有网络 | 开启（Mac 和 iPhone 都要开） |

### Mac 配置（交给 Codex）

在小火箭首页选中固定节点，"全局路由"选择"配置"，然后将以下提示词（替换`【节点名称】`后）发给 Codex：

```
请帮我配置Mac上的Shadowrocket，固定使用节点【节点名称】。

检查并将"设置 → 代理 → 代理类型"设为None（TUN Mode），"全局路由"使用"配置"。

开启"设置 → 隧道 → 包括所有网络"，确认开关已生效，再核验Claude请求是否命中指定节点。

为以下4个域名添加DOMAIN-SUFFIX（域名后缀）规则，覆盖域名本身及其子域名，使用指定节点：
anthropic.com
claude.ai
api.anthropic.com
statsig.anthropic.com
```

### iPhone 配置（通过 iPhone 镜像 + Codex）

打开 Mac 的「iPhone 镜像」，完成首次连接（两端登录同一系统 Apple 账户，开启 Wi-Fi 和蓝牙，手机锁屏放在 Mac 旁边）。

看到手机画面后，发送以下内容给 Codex：

```
我已打开iPhone镜像，请使用Computer Use操作镜像窗口，完成手机上的Shadowrocket配置：

1. 先备份手机现有配置，保留其他分流规则，添加前面准备的4条Claude域名后缀规则。确认这些域名及其子域名走同一个固定节点，选择"配置"路由并连接。
2. 将"设置 → 代理 → 代理类型"设为None（TUN Mode），开启"设置 → 隧道 → 包括所有网络"，并开启"始终开启"。
3. 分别核验Mac和iPhone的Claude请求是否命中指定节点，以及该节点的实际出口IP。
```

---

## 六、修改时区设置

大陆时区与台北相同，而台北是 Claude 支持地区，将时区改为台北可降低地区检测风险。

### Mac

> 系统设置 → 搜索"时区" → 改成"台北市-台湾"
> 关掉"自动设定时区"
> 搜索"定位服务" → 关掉"定位服务"

### iPhone

> 设置 → 搜索"日期与时间" → 时区选"台北标准时间"，城市选"台北"
> 关掉"自动设定时间"
> 设置 → 搜索"系统服务" → 关掉"设定时区"

---

## 七、注册全新 Claude 账号

环境配置完毕后，用新注册的 Gmail 账号登录 Chrome，打开 [claude.ai](https://claude.ai)，点击 **"Continue with Google"** 完成注册。

> ⚠️ claude.ai 在国内需科学上网访问，请确保此步骤在科学上网环境下进行。

手机号验证步骤：遇到了就按下一节操作，没遇到可直接跳至订阅步骤。

---

## 八、绑定美国手机号（按需）

如果注册或订阅过程中要求验证手机号，推荐方案：**RedPocket 美国号码 + BeeSIM 实体转换卡**

> - RedPocket：约 $30/年（eBay 购买，支持中国 PayPal）
> - BeeSIM：$19.9 永久（国行 iPhone 不支持直接安装 eSIM，需要此转换卡）

### 购买 RedPocket

eBay 商品页：[https://www.ebay.com/itm/136840233242](https://www.ebay.com/itm/136840233242)

- 套餐选择：$2.5/月（$30/年），下拉框选 **eSIM Phone Kit（Most popular）**
- 地址：填写美国免税州地址（Google 地图或 ChatGPT 生成）
- 电话：填国内手机号即可
- 支付：中国 PayPal

### 激活并开启漫游

支付成功约 10-15 分钟后，邮箱会收到 RedPocket 激活邮件：

> ⚠️ redpocket.com 在国内需科学上网访问。

1. 打开 [redpocket.com/activate](https://redpocket.com/activate)
2. 填写 Device IMEI（iPhone：设置 → 通用 → 关于本机 → IMEI）
3. 在 App 内找到 **Roaming** 界面，开启漫游，下拉框选 **CHINA**
4. 显示 "It's Enabled." 即为成功

> **遇到余额为 0 的情况**：可联系 eBay 客服说明套餐额度问题申请退款，次日重新购买。

### 导入 eSIM 到 BeeSIM

> ⚠️ shop.beeesim.com 在国内需科学上网访问。

1. 购买 BeeSIM 实体卡（[九折优惠链接](https://shop.beeesim.com/register?ref=7QLE2MXR)）
2. 扫描信封上的二维码，进入 BeeSIM 微信小程序
3. 开启蓝牙，连接 BeeSIM
4. 找到 RedPocket 发送的含二维码的激活邮件，用 BeeSIM 扫描录入，完成绑定

完成后即可用该美国号码接收短信和接打电话。

---

## 九、订阅 Pro 和 Max

点击 **"Upgrade Plan"**，**建议先订阅 Pro，不要直接跳 Max**（直接跳 Max，尤其是 20× 版本，封号概率较高）。

### 填写账单信息

| 字段 | 填写建议 |
|------|----------|
| Full name | 随意填写一个英文名 |
| Country or region | United States |
| Address | 美国免税州地址（与前面保持一致） |
| Use a different name on invoices | 留空 |

**升级路径**：Pro 用满约 5 小时后，再升级 Max 5×。升级后**不要用满每周配额**，保留 10%-20% 的余量，可显著降低封号概率。

---

## 十、中国护照通过 KYC 认证

订阅 Max 后可能触发 Persona KYC 实名认证，**使用中国护照即可通过**。

**原理**：执行 KYC 的第三方平台 Persona，其政策允许中国护照。Anthropic 本身也有大量在美华人用户和留学生，不会一刀切拒绝中国护照。

账号被封的核心原因往往是**出口 IP 不稳定**和**环境配置问题**，护照本身不是障碍。

**操作流程**：按 Persona 页面提示，上传护照照片页，完成人脸识别，等待审核通过即可。

---

## 总结

整个流程的关键优先级：

```
稳定独享 IP > 环境彻底清理 > 邮箱质量 > 支付方式 > 使用行为克制
```

配置正确后，无论是 Claude Web、Claude App 还是 Claude Code，均可正常使用。KYC 认证通过后，Claude 官方赠送的额度和 Max 套餐功能均正常解锁。

---

*参考来源：逸尘 @gengdaJ，余温 @gkxspace Claude 防封号教程*
