- [计算机网络 - 网络层：拥塞控制](#计算机网络---网络层拥塞控制)
  - [网络供给 (Network provisioning)](#网络供给-network-provisioning)
  - [流量感知路由 (Traffic-aware routing)](#流量感知路由-traffic-aware-routing)
  - [准入控制 (Admission control)](#准入控制-admission-control)
  - [流量限制 (Traffic throttling)](#流量限制-traffic-throttling)
  - [负载脱落 (Load shedding)](#负载脱落-load-shedding)
  - [Refer Links](#refer-links)

# 计算机网络 - 网络层：拥塞控制

由于拥塞发生在网络中，网络层直接经历着拥塞，且必须由它最终确定如何处理过载的数据包，因此网络层必须提供拥塞控制的功能。

拥塞的出现以为着负载超过了资源可处理的能力，因此很容易能想到在网络层解决拥塞问题的 2 个基本方案：
- 增加资源
  - 在某些点之间使用更多的通道增加带宽
  - 把流量分散到多条路径
  - 启用空闲或备份的路由器
- 降低负载
  - 拒绝为某些用户提供服务
  - 给某些用户的服务降低等级
  - 让用户更有预见性地安排他们的需求

这些解决方案通常应用在不同的时间尺度上，要么预先避免拥塞，要么一旦发生拥塞随之做出应对措施。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/11/d0bb05f9285c61258f76a79f05c93e6f.jpg)

## 网络供给 (Network provisioning)

## 流量感知路由 (Traffic-aware routing)

## 准入控制 (Admission control)

## 流量限制 (Traffic throttling)

## 负载脱落 (Load shedding)

## Refer Links