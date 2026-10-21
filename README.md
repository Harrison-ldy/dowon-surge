# Dowon Surge

个人 Surge 配置 · **Beta**，尚未完成全部 Mac/iPhone 场景验证。

[下载配置](https://raw.githubusercontent.com/Harrison-ldy/dowon-surge/main/config/Dowon-Surge.conf)

## 使用

1. 下载配置并导入 Surge。
2. 预留 `机场1`～`机场6`：在需要启用的机场行末添加 `, policy-path=你的Surge订阅链接`；不用的位置保持原样。例如：
   ```ini
   机场1 = select, Fail Closed, external-policy-name-prefix=A-, update-interval=0, category=SPEC, policy-path=YOUR_SUBSCRIPTION_URL
   ```
3. 在本地将 `Korea VPS` / `USA VPS` 的 `reject` 占位替换为实际代理节点定义，保留名称；未填写的出口不能上网。
4. 选择本配置并使用「规则判定」。`Entry` 选机场地区，`VPS Exit` 选 VPS；`Video Exit` 默认香港机场。

真实订阅及 VPS 凭据只保存在本地，不提交 GitHub。中国直连与视频列表当前为空：[中国白名单](rules/china-direct.list) · [视频列表](rules/video.list)。

## 更新

主配置需手动下载新版；「重载配置」只重新读取本地文件。更新前导出备份，将真实订阅和 VPS 参数迁入新版，再切换使用。旧版「我的节点」中的订阅移至新版「机场1」，不要直接覆盖已填写的文件。

## 来源

基于 [Rabbit-Spec/Surge · Surge-Mini](https://github.com/Rabbit-Spec/Surge/blob/Master/Conf/Spec/Surge-Mini.conf) 裁剪定制。
