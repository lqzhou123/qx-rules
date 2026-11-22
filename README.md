# Quantumult X 广告规则

本仓库提供 Quantumult X 的广告分流和请求重写。将 AdvertisingLite、AdvertisingScript 和 ZhihuAds 的广告条目合并为一份分流和一份重写，便于在 QX 中每类只保留一个广告引用资源。

## 规则拉取地址

| 资源 | QX 配置区段 | Raw 拉取地址 |
| --- | --- | --- |
| 统一广告分流 | `[filter_remote]` | [AdvertisingMerged.list](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.list) |
| 统一广告重写 | `[rewrite_remote]` | [AdvertisingMerged.conf](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.conf) |

把两个 Raw 地址分别添加到 Quantumult X 的「分流 → 规则资源」和「重写 → 规则资源」。如果直接编辑配置，把示例里的两条引用加入现有的对应区段。

**广告分流须省略 `force-policy`**，由规则文件逐条决定 `DIRECT` 或 `REJECT`。强制 `REJECT` 会覆盖 `h.trace.qq.com` 的直连规则。使用重写资源时需开启重写，HTTPS 重写还依赖 QX 的 MitM 设置。

```ini
[filter_remote]
https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.list, tag=广告分流, update-interval=604800, opt-parser=false, enabled=true

[rewrite_remote]
https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.conf, tag=广告重写, update-interval=604800, opt-parser=false, enabled=true
```

### 已有广告订阅时

如果已订阅本仓库的旧版，可在配置副本中用上面两条新资源替换原来的分流和重写引用。使用了其他来源的规则时，可按实际覆盖范围停用重复的条目。

## 附带分流

[DirectAllowlist.list](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/DirectAllowlist.list) 是可选的直连白名单，基于 blackmatrix7 的 `Direct.list`。启用时，将它放在广告分流之前并设置 `force-policy=direct`，让名单里的域名优先直连。已有 blackmatrix7 的同类白名单时，可以原位替换地址。

```ini
[filter_remote]
https://raw.githubusercontent.com/lqzhou123/qx-rules/main/DirectAllowlist.list, tag=直连规则, force-policy=direct, update-interval=604800, opt-parser=false, enabled=true
```

- iCloud 改为 `icloud.com` 和 `icloud.com.cn` 两个域名后缀，避免把带相同字符串的第三方域名一起直连。[Apple 网络文档](https://support.apple.com/zh-cn/101555)
- Trip.com／携程改为明确的域名后缀，保留 `trip.com`、`ctrip.com`、素材域 `c-ctrip.com` 和携程的 `.com.cn`、`.com.hk` 地址。[携程官网](https://www.ctrip.com/)
- 移除已停服的 Let's Encrypt OCSP `o.lencr.org` 关键词条目。[停服公告](https://letsencrypt.org/2025/08/06/ocsp-service-has-reached-end-of-life/)

此前使用上游白名单的读者，可通过恢复 [原 Direct.list 地址](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/Direct/Direct.list) 回退。

## 起点读书报表处理

`h.trace.qq.com` 的调整源于 iOS 版起点读书翻页时的异常发热排查。真机对照中，原分流规则在 DNS 层拒绝该主机时，报表随翻页积压。加入精确直连后，旧报表消退，随后日常试用中暂未复现异常发热。

广告分流包含精确的 `HOST,h.trace.qq.com,DIRECT`。重写匹配该主机的明文 HTTP `POST /kv`，由 QX 在手机上返回空的 HTTP 200。此前以本地规则实测，命中请求的上游收发均为 `0 B`。此前手动添加过相同规则的用户，可在试用新订阅的配置副本中停用重复条目。

## 旧版与回退

回退时可恢复切换前的 QX 配置副本，包括原有的订阅和本地规则。以下是本仓库早期的 AD 精简资源，旧版分流使用 `force-policy=reject`。[旧版分流](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/116_AdvertisingLite.list) · [旧版重写](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/127_AdvertisingLite.conf)

### 新版沿用的 AD 精简调整

- OpenX 分流收窄为 `openx.net`、`openxenterprise.com` 和 `openxmarket.asia` 三个域名后缀，避免误拦 OpenXLab。 [ACL4SSR 的修正](https://github.com/ACL4SSR/ACL4SSR/commit/431a0bdd5ff58f7544a73cf23d7f623ebaf5930e)
- 移除 `wixsite.com` 关键词规则，避免把整个 Wix 免费托管域纳入拒绝范围。 [Wix 域名说明](https://support.wix.com/en/article/about-domains)
- 新增小红书开屏素材域名 `ads-img-bak.xhscdn.com`、`ads-vp5.xhscdn.com`，以及领页广告竞价域名 `bid.shanghailingye.cn`，均按精确主机匹配。 [秋风小红书变更](https://github.com/TG-Twilight/AWAvenue-Ads-Rule/commit/a5af99ea6ed76facd752428cd684dad1f12e21bf) · [秋风领页变更](https://github.com/TG-Twilight/AWAvenue-Ads-Rule/commit/02737455da627c55b636e26057a422df9a59df01) · [anti-AD 规则对照](https://github.com/privacy-protection-tools/anti-AD/blob/master/anti-ad-quanx.txt)

## 来源与许可

规则基于 blackmatrix7 的 `ios_rule_script`，按 GPL-2.0 发布。[上游仓库](https://github.com/blackmatrix7/ios_rule_script) · [许可证](LICENSE)
