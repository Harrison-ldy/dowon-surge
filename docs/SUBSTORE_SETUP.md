# Dowon Surge｜Sub-Store 使用

## 目标

Sub-Store 只负责本地节点/订阅的解析和转换，不保存到 GitHub。

当前主要用它管理两个 VPS：
- `Korea-VPS`
- `USA-VPS`

主配置读取：

```text
https://sub.store/download/Korea-VPS/Surge
https://sub.store/download/USA-VPS/Surge
```

这些请求由 Surge 本机的 Sub-Store 脚本接管。

## 首次启用

1. 使用包含 Sub-Store 的 Dowon Surge Beta。
2. 在 Surge 中安装并信任 MITM 根证书。
3. 用 Safari 打开 `https://sub.store`。
4. 页面正常打开且无 Sub-Store 后端错误，即表示本地 Sub-Store 正常。

配置包含：

```ini
[Host]
sub.store = 127.0.0.1
```

这是安全保护：若脚本没有接管，请求会失败在本机，不会误发到公网 `sub.store`。

## 添加 Korea VPS

在 Sub-Store：
1. 新建“单订阅”。
2. 名称必须填写 `Korea-VPS`。
3. 来源选择本地/手动内容。
4. 粘贴 Korea VPS 的完整 `ss://...` URI。
5. 保存。

## 添加 USA VPS

同样创建：

```text
名称：USA-VPS
内容：你的 USA VPS ss:// URI
```

真实 `ss://` 含密码/密钥，不进入 GitHub，也不要发送到聊天里。

## Surge 如何使用

`Korea VPS` 和 `USA VPS` 是 fallback 组：

```text
Sub-Store 真实 VPS 可用
→ 选择真实 VPS
→ underlying-proxy=Entry
→ 机场 → VPS → Internet

Sub-Store 未配置 / 输出为空 / VPS 不可用
→ Fail Closed
→ REJECT
```

因此不会因为 VPS 订阅为空而自动 DIRECT。

## 机场订阅

当前机场订阅仍沿用：

```ini
我的节点 = select, Fail Closed, policy-path=你的订阅地址
```

机场真实 URL 继续只在 Surge 本地填写。以后若需要多机场，再考虑把机场也迁入 Sub-Store collection。
