# Quantumult X AD 精简规则

本仓库提供 Quantumult X 的广告分流和请求重写规则，基于 blackmatrix7 的 AdvertisingLite。本版本移除了 `h.trace.qq.com` 的域名级拒绝，以及三条已被同动作规则覆盖的重写。

## 规则拉取地址

| 资源 | QX 配置区段 | Raw 拉取地址 |
| --- | --- | --- |
| AD 精简分流 | `[filter_remote]` | [116_AdvertisingLite.list](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/116_AdvertisingLite.list) |
| AD 精简重写 | `[rewrite_remote]` | [127_AdvertisingLite.conf](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/127_AdvertisingLite.conf) |

在 Quantumult X 的「分流规则」和「重写规则」中，分别将对应地址添加到「引用资源」。已有「AD 精简」时，在原位置替换地址。分流资源的强制策略设为 `REJECT`（`force-policy=reject`）。

## 来源与许可

规则基于 blackmatrix7 的 `ios_rule_script`，按 GPL-2.0 发布。[上游仓库](https://github.com/blackmatrix7/ios_rule_script) · [许可证](LICENSE)
