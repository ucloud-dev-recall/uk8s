# 网络插件 升级

UK8S 提供的 CNI （Container Network Interface）基于 UCloud VPC 网络实现，因此会随着 VPC 的功能迭代同步更新版本，以提升容器网络的稳定性及性能。
UK8S提供 CNI 在线升级的功能，插件升级不会影响现有 Pod 的网络。CNI 升级成功后：

1. 其实现的网络特性将作用于新申请的 Pod；
2. 老的 Pod 如果也需要获得对应的网络特性，则需要滚动升级以触发 Pod 重建;

下面将介绍下如何在线升级网络插件。

## 1. 网络插件升级

在 UK8S 集群控制台管理页面「插件-网络插件」页面，开启 CNI 网络插件升级功能，开启 CNI 插件升级功能会在集群中执⾏ CNI 插件查询任务，⼤约需要 3
分钟，在此过程中请不要操作集群。升级功能开启后，即可看到 CNI 插件版本信息，点击「升级 CNI」即可进行升级。

如控制台页面无法查看 CNI 版本信息，请在集群中任一节点执行 `/opt/cni/bin/cnivpc version` 查看。

升级过程约需要 1-3
分钟，升级过程中「当前版本」字段会显示为「升级中」，升级完成后显示最新版本号，如升级失败，可以再尝试「强制升级」，或与我们技术支持联系。部分节点由于版本原因，可能无法获取到「当前CNI版本」，直接点击强制升级即可。

支持单节点和批量升级，建议先升级单台节点，如果升级成功，则再进行批量升级。当所有节点都升级成功后，可关闭插件升级服务，后续有升级需求时再开启。

> ⚠️ 集群网络插件升级时，请勿进行服务发布等操作

## 2. 网络插件更新纪要

| 版本      | 类型          | 更新说明                                                                                                        | 发布时间        |
| ------- | ----------- | ----------------------------------------------------------------------------------------------------------- | ----------- |
| 21.12.2 | Feature     | ipamd 支持开启 calico policy 特性，IP 会写入到 Pod 的 annotation，calico 可以通过感知该 IP 来及时下发规则                              | 2021年12月28日 |
| 21.12.1 | Enhancement | 1. 优化 ipamd 申请 IP 流程，ipamd 会在申请到 IP 以后立刻发送 garp ；<br>2. 优化释放 IP 时候获取 mac 的流程。                               | 2021年12月14日 |
| 21.10.1 | Bugfix      | 1. 修复固定 IP 意外释放导致 StatefulSet Pod 不可用的问题；<br>2. 修复 CNI 抢占文件锁超时导致释放 IP 失败的问题；<br>3. IPAMD 插件开启后，将默认使用其管理 IP。 | 2021年11月4日  |
| 21.07.1 | Bugfix      | 解决部分节点无法获取 CNI 版本问题                                                                                         | 2021年7月1日   |
| 21.06.1 | Feature     | 支持 Pod 固定 IP（[固定 IP 使用方法](/uk8s/network/static_ip)）                                                         | 2021年6月23日  |
| 21.01.3 | Bugfix      | 兼容开启了弹性网卡的UHost节点，解决其无法出外网的问题                                                                               | 2021年1月29日  |
| 21.01.2 | Feature     | 将 Pod 的默认 MTU 设置为1452                                                                                       | 2021年1月15日  |
| 21.01.1 | Enhancement | ipamd 申请 IP 机制优化                                                                                            | 2021年1月1日   |
| 20.07.1 | Enhancement | 支持 garp 机制，优化 Pod 网络首包延时问题                                                                                  | 2020年7月16日  |

文档更新可能滞后，最新版本请以产品页面为准。
