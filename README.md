# Quantumult X AD 精简规则

本仓库提供 Quantumult X 的广告分流和请求重写规则，基于 blackmatrix7 的 AdvertisingLite。本版本移除了 `h.trace.qq.com` 的域名级拒绝，以及三条已被同动作规则覆盖的重写。

`h.trace.qq.com` 的调整源于 iOS 版起点读书翻页时的异常发热排查。真机对照中，原分流规则在 DNS 层拒绝该主机时，应用的报表随翻页持续积压。精确放行后旧报表消退，随后在日常试用中暂未复现异常发热。

## 规则拉取地址

| 资源 | QX 配置区段 | Raw 拉取地址 |
| --- | --- | --- |
| AD 精简分流 | `[filter_remote]` | [116_AdvertisingLite.list](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/116_AdvertisingLite.list) |
| AD 精简重写 | `[rewrite_remote]` | [127_AdvertisingLite.conf](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/127_AdvertisingLite.conf) |

在 Quantumult X 的「分流规则」和「重写规则」中，分别将对应地址添加到「引用资源」。已有「AD 精简」时，在原位置替换地址。分流资源的强制策略设为 `REJECT`（`force-policy=reject`）。

## 2026-11-22 更新

- OpenX 分流收窄为 `openx.net`、`openxenterprise.com` 和 `openxmarket.asia` 三个域名后缀，避免误拦 OpenXLab。 [ACL4SSR 的修正](https://github.com/ACL4SSR/ACL4SSR/commit/431a0bdd5ff58f7544a73cf23d7f623ebaf5930e)
- 移除 `wixsite.com` 关键词规则，避免把整个 Wix 免费托管域纳入拒绝范围。 [Wix 域名说明](https://support.wix.com/en/article/about-domains)
- 新增小红书开屏素材域名 `ads-img-bak.xhscdn.com`、`ads-vp5.xhscdn.com`，以及领页广告竞价域名 `bid.shanghailingye.cn`，均按精确主机匹配。 [秋风小红书变更](https://github.com/TG-Twilight/AWAvenue-Ads-Rule/commit/a5af99ea6ed76facd752428cd684dad1f12e21bf) · [秋风领页变更](https://github.com/TG-Twilight/AWAvenue-Ads-Rule/commit/02737455da627c55b636e26057a422df9a59df01) · [anti-AD 规则对照](https://github.com/privacy-protection-tools/anti-AD/blob/master/anti-ad-quanx.txt)

## 来源与许可

规则基于 blackmatrix7 的 `ios_rule_script`，按 GPL-2.0 发布。[上游仓库](https://github.com/blackmatrix7/ios_rule_script) · [许可证](LICENSE)
