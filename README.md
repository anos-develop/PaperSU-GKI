# PaperSU-GKI

给 **全 OnePlus / OPPO / realme 机型** 编译 GKI 内核的自动化工程，产出的内核内置 **paperSU**。

> **本项目基于 [Numbersf/Action-Build](https://github.com/Numbersf/Action-Build) 修改而来。**
> 原项目作者：**Numbersf**（Copyright (c) 2026 Numbersf-Action.Build）。
> 本仓库按原项目 [LICENSE](LICENSE) 第 1 条（**含有与原始源码明显可区分的修改**）进行再分发。
> **本仓库不主张对原始代码的著作权**，原始著作权归 Numbersf 所有。
> 编译产物（内核 zip）的再分发按原 LICENSE 明确允许，前提是不声称原创。

---

## 相对原项目改了什么

这些改动是实质性的，不是只改名字：

| # | 改动 | 位置 |
|---|---|---|
| 1 | **管理器源指向 `anos-develop/PaperSU-Main`** —— `setup.sh`、release tag、CI 产物全部换成 paperSU 的仓库 | `.github/workflows/Build All OnePlus Kernels.yml` |
| 2 | **版本号钉死为 `41010-1`** —— 原来是 `40000 + 提交数 - 2815` 且依赖上游 release tag，现在管理器、内核模块、内核完整版本三者一致 | 同上 + `PaperSU-Main` 的 `Kbuild` |
| 3 | **自包含** —— 助手脚本 / 补丁 / ccache 改为从本仓库克隆（原是运行时拉 `Numbersf/Action-Build`） | 同上 |
| 4 | **Git 身份换成 `anos-develop`** | 同上 |
| 5 | **清单附录仓库换成 `anos-develop/PaperSU-Manifest-Appendix`** | 同上 |
| 6 | **移除上游的宣传图片目录和 `CAll Build Start UP` 批量工作流** | 仓库结构 |

**⇒ 如果你要的是原项目的完整功能，请直接使用原项目。** ✗

---

## 怎么用

### 1. 编译一个机型

```
Actions → Build All OnePlus Kernels (paperSU) → Run workflow

  FILE        选机型（默认 oneplus_ace2_pro_b）
  KSU_META    main/main/41010-1/       ← 已设好默认值，不用改
  其它开关    按需
```

**⇒ 跑完在 Artifacts 里拿 AnyKernel3 的 zip** ✓

### 2. 机型名怎么选

见 [FILE.md](FILE.md)，规律是：

```
<品牌>_<机型>_<安卓版本代号>

  _s  Android 12        oneplus_10_pro_s
  _t  Android 13        oneplus_11_t
  _u  Android 14        oneplus_12_u
  _v  Android 15        oneplus_11_v
  _b  Android 16        oneplus_12_b
  无后缀 = 出厂安卓版本
```

### 3. `KSU_META` 格式

```
管理器分支名/内置分支名/自定义版本标识/回退提交哈希
```

- **默认 `main/main/41010-1/`** ✓ —— **⇒ 指 `PaperSU-Main` 的 `main` 分支 ✓**
- **自定义版本标识留空不改** ✓ —— **⇒ 但本项目已把 `VERSION_FULL` 钉死成 `41010-1` ✓**

### 4. 刷入

```
AnyKernel3 zip → 内核管理器刷入，或
fastboot boot / fastboot flash boot
```

**⇒ 刷之前务必备份原 boot** ✓

---

## 开关说明

| 输入 | 默认 | 说明 |
|---|---|---|
| `SUSFS_CI` | — | SUSFS 模块来源分支 |
| `KPM` | — | 内核模块实现方式 |
| `ZRAM` | `0/lz4kd/8589934592` | `开关/算法/大小` |
| `FAST_BUILD` | `true` | 极速构建（用 ccache，第一次会慢） |
| `LSM_BBG` | — | 关键分区写入保护 |
| `NETFILTER` | — | 网络功能扩展 |
| `CCM` | — | BBRv1 + BBRv3 + ECN |
| `UNICODE_BYPASS` | — | Unicode 不可见字码点绕过修复 |
| `SCHED_HMBIRD` | — | 风驰驱动 |
| `DROID_SPACES` | — | 轻量级容器支持 |
| `RE_KERNEL` | — | Re:Kernel |
| `SUFFIX` | 空 | 自定义内核后缀 |
| `RESUBLEVEL` | 空 | SUBLEVEL 欺骗 |

**⇒ 完整含义见上游 [README.md 原文](https://github.com/Numbersf/Action-Build)** ✓

---

## 版本号

```
管理器 versionCode   41010      versionName  41010-1
内核 KSU_VERSION     41010
内核完整版本          41010-1
```

**⇒ 三处一致 ✓，管理器里不会出现"版本不一致"的红框** ✓

---

## 许可与署名

- **原始项目**：[Numbersf/Action-Build](https://github.com/Numbersf/Action-Build)
- **原始著作权**：Copyright (c) 2026 Numbersf-Action.Build
- **本仓库的修改部分**：由 `anos-develop` 完成
- **许可证**：[LICENSE](LICENSE) —— 原样保留，未做修改

**⇒ 本仓库不主张对原始代码的著作权** ✓
**⇒ 编译产物可以自由分发 ✓，只要不声称原创 ✓**

---

## 已知问题

- **内核编译很慢** ✓ —— 6.1~6.12 大约 55~72 分钟 ✓，5.10~5.15 大约 20~35 分钟 ✓
- **`Initialize Repo and Sync` 这一步经常受上游 repo 工具影响** ✓ —— 超过 15 分钟建议重跑 ✓
- **第一次开 `FAST_BUILD` 会变慢** ✓（ccache 是空的 ✓）

---

## paperSU 相关

- **管理器源码**：[anos-develop/PaperSU-Main](https://github.com/anos-develop/PaperSU-Main)
- **卡密签发工具**：见 `PaperSU-Main` 的 `tools/license/`
- **授权站**：见 `PaperSU-Main` 的 `tools/authsite/`