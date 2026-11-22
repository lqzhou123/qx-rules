# Quantumult X 广告规则

本仓库提供 Quantumult X 的广告分流和请求重写。将 AdvertisingLite、AdvertisingScript 和 ZhihuAds 的广告条目合并为一份分流和一份重写，便于在 QX 中每类只保留一个广告引用资源。

## 规则拉取地址

| 资源 | QX 配置区段 | Raw 拉取地址 |
| --- | --- | --- |
| 统一广告分流 | `[filter_remote]` | [AdvertisingMerged.list](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.list) |
| 统一广告重写 | `[rewrite_remote]` | [AdvertisingMerged.conf](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.conf) |

将两个地址分别添加到分流和重写的「引用资源」。**统一分流须省略 `force-policy`**。文件已逐条指定 `DIRECT` 或 `REJECT`，强制 `REJECT` 会覆盖 `h.trace.qq.com` 的直连规则。

```ini
[filter_remote]
https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.list, tag=广告分流, update-interval=604800, opt-parser=false, enabled=true

[rewrite_remote]
https://raw.githubusercontent.com/lqzhou123/qx-rules/main/AdvertisingMerged.conf, tag=广告重写, update-interval=604800, opt-parser=false, enabled=true
```

### 从旧配置切换

在 QX 配置副本中，将统一分流放在「直连规则」之后、「Zhihu」之前。停用旧广告分流 `ZhihuADs`、`AD补充`、`AD 精简` 以及旧重写 `AD 补充`、`AD 精简`，并在副本中停用两条旧本地报表规则。核对新订阅的加载顺序，再确认起点报表命中 `REWRITE / REJECT-200`，上游收发均为 `0 B`。

## 起点读书报表处理

`h.trace.qq.com` 的调整源于 iOS 版起点读书翻页时的异常发热排查。真机对照中，原分流规则在 DNS 层拒绝该主机时，报表随翻页积压。加入精确直连后，旧报表消退，随后日常试用中暂未复现异常发热。

统一分流内置 `HOST,h.trace.qq.com,DIRECT`，统一重写对该主机的明文 HTTP `POST /kv` 由 QX 本地返回空 200。这两条在此前的本地配置中已验证命中和 `0 B` 上游流量。合并版已通过离线回归，实际订阅加载与命中待真机确认。

## 旧版与回退

回退时停用两份统一资源，恢复原先启用的广告资源和两条本地报表规则。旧 AD 精简分流使用 `force-policy=reject`。[旧版分流](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/116_AdvertisingLite.list) · [旧版重写](https://raw.githubusercontent.com/lqzhou123/qx-rules/main/127_AdvertisingLite.conf)

### 已继承的 AD 精简调整

- OpenX 分流收窄为 `openx.net`、`openxenterprise.com` 和 `openxmarket.asia` 三个域名后缀，避免误拦 OpenXLab。 [ACL4SSR 的修正](https://github.com/ACL4SSR/ACL4SSR/commit/431a0bdd5ff58f7544a73cf23d7f623ebaf5930e)
- 移除 `wixsite.com` 关键词规则，避免把整个 Wix 免费托管域纳入拒绝范围。 [Wix 域名说明](https://support.wix.com/en/article/about-domains)
- 新增小红书开屏素材域名 `ads-img-bak.xhscdn.com`、`ads-vp5.xhscdn.com`，以及领页广告竞价域名 `bid.shanghailingye.cn`，均按精确主机匹配。 [秋风小红书变更](https://github.com/TG-Twilight/AWAvenue-Ads-Rule/commit/a5af99ea6ed76facd752428cd684dad1f12e21bf) · [秋风领页变更](https://github.com/TG-Twilight/AWAvenue-Ads-Rule/commit/02737455da627c55b636e26057a422df9a59df01) · [anti-AD 规则对照](https://github.com/privacy-protection-tools/anti-AD/blob/master/anti-ad-quanx.txt)

## 来源与许可

规则基于 blackmatrix7 的 `ios_rule_script`，按 GPL-2.0 发布。[上游仓库](https://github.com/blackmatrix7/ios_rule_script) · [许可证](LICENSE)
