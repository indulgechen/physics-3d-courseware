# 大学物理 · 3D 交互课件

配套马文蔚《大学物理》的课堂演示课件集合：**单文件 HTML、离线可用、手机与电脑都能打开**。
每份课件都可以直接双击打开投影上课，也可以部署到 GitHub Pages 在线访问。

![制冷机 自适应版](previews/fridge-responsive.png)

---

## 课件清单

| 课件 | 文件 | 体积 | 说明 |
| --- | --- | --- | --- |
| 铁的冶炼 · 高炉 | [`courseware/blast-furnace.html`](courseware/blast-furnace.html) | 55 KB | 炉料下行、热风上行、铁水与炉渣分层 |
| 杨氏双缝干涉 | [`courseware/double-slit.html`](courseware/double-slit.html) | 84 KB | 单缝 S、双缝 d、光程差 Δr = d sinθ，可切白光 |
| 热机与制冷机 · 卡诺循环 | [`courseware/carnot-cycle.html`](courseware/carnot-cycle.html) | 63 KB | 同一循环正着走是热机、倒着走是制冷机，p–V 与能量流同步 |
| 热机（蒸汽机）· 轻量版 | [`courseware/heat-engine-2d.html`](courseware/heat-engine-2d.html) | 58 KB | 锅炉—汽缸—冷凝器—泵，Q₁ = W + Q₂ |
| 制冷机（冰箱）· 轻量版 | [`courseware/fridge-2d.html`](courseware/fridge-2d.html) | 56 KB | 蒸发器—压缩机—冷凝器—毛细管，e = Q₂/\|W\| |
| 热机 · 3D | [`courseware/heat-engine-3d.html`](courseware/heat-engine-3d.html) | 1.7 MB | Blender 模型 + three.js：剖切汽缸、活塞连杆、柴火加热 |
| 制冷机 · 3D | [`courseware/fridge-3d.html`](courseware/fridge-3d.html) | 2.06 MB | 半截面管路、状态分色、压缩机风扇 |
| 热机 · 手机版 | [`courseware/heat-engine-mobile.html`](courseware/heat-engine-mobile.html) | 1.7 MB | 竖屏可滚动：效率公式、三量关系、为什么 η < 1 |
| 制冷机 · 手机版 | [`courseware/fridge-mobile.html`](courseware/fridge-mobile.html) | 2.04 MB | 竖屏可滚动：制冷系数、三量关系、为什么必须做功 |
| 制冷机 · 自适应版 | [`courseware/fridge-responsive.html`](courseware/fridge-responsive.html) | 2.04 MB | **推荐**：一份文件兼顾手机（竖排）和电脑（左右分栏） |

> 体积大的几份是 three.js + Blender GLB 版本，模型已以 base64 内嵌，仍然是单文件、离线可用。

---

## 预览

| 热机 3D | 制冷机 3D |
| --- | --- |
| ![热机](previews/engine-3d.png) | ![制冷机](previews/fridge-3d.png) |

| 卡诺循环 | 杨氏双缝 |
| --- | --- |
| ![卡诺](previews/carnot.png) | ![双缝](previews/double-slit.png) |

手机竖版：

<img src="previews/fridge-responsive-mobile.png" width="320" alt="手机版">

---

## 使用

### 本地打开

直接双击 `courseware/` 下的任意 `.html` 文件即可，无需联网、无需安装依赖。
建议用 Chrome / Edge / Firefox；three.js 版本需要浏览器支持 WebGL。

### 部署到 GitHub Pages

1. 把本仓库内容推到 GitHub（`index.html` 在根目录）；
2. 仓库 **Settings → Pages**，Source 选 `Deploy from a branch`，分支选 `main`、目录选 `/ (root)`；
3. 等一分钟，访问 `https://<用户名>.github.io/<仓库名>/`。

根目录的 `index.html` 是导航页，会列出全部课件。

---

## 技术说明

- **轻量版（55–84 KB）**：不依赖任何库，用 Canvas 2D 手写 3D 投影（四边形排序 + 光照着色），打开即用。
- **3D 版（1.7–2.1 MB）**：Blender 脚本建模 → 导出 GLB → three.js 渲染；three.js、GLTFLoader、OrbitControls、RoomEnvironment 与 GLB 全部内嵌在单个 HTML 里。
- **自适应版**：同一份 DOM + 一个 960 px 断点，宽屏切两列 grid、窄屏竖排；画布尺寸由 `ResizeObserver` 跟随容器。
- 3D 版本做了手机优化：`devicePixelRatio` 上限 2、阴影贴图 1024、触控旋转/缩放。

### 目录结构

```
.
├── index.html                 导航页
├── courseware/                10 份单文件课件
├── previews/                  预览图
├── docs/                      每份课件的说明（操作、讲法、改造）
└── src/
    ├── blender/               Blender 脚本（程序化建模 + 导出 GLB）
    ├── build/                 打包脚本（把 CSS/JS/GLB 内联成单文件）
    └── dev/                   拆分源码（index.html / app.js / styles.css / *.glb / vendor）
```

### 重新构建

需要 Node.js；three.js 版本还需要 Blender 4.x/5.x。

```bash
# 1) 重新导出 GLB（可选）
blender -b -P src/blender/fridge_proto.py   # 制冷机模型
blender -b -P src/blender/engine_proto.py   # 热机模型
blender -b -P src/blender/make_glb.py       # 制冷机 GLB（含半截面管路）

# 2) 打包
node src/build/build-glb-resp.js         # 制冷机 自适应版
node src/build/build-glb-mobile-fridge.js # 制冷机 手机版
node src/build/build-glb.js              # 制冷机 电脑版
node src/build/build-glb-engine.js       # 热机 3D
node src/build/build-glb-mobile.js       # 热机 手机版
node src/build/build-inline.js           # 高炉（轻量版）
node src/build/build-optics.js           # 双缝（轻量版）
node src/build/build-machines.js         # 热机/制冷机 轻量版
node src/build/build-thermo.js           # 卡诺循环
```

> `src/build/` 里的脚本默认从各自 `dev/` 目录读取并输出到仓库根目录，按需调整路径即可。

---

## 说明与限制

- 模型是**教学示意**：尺寸、管径、行程都做了夸张，便于看清结构，不与真实设备等比例。
- 效率与制冷系数显示的是**卡诺理论值**（1 − T₂/T₁、T₂/(T₁ − T₂)），实际机器因摩擦、散热会更低。
- 轻量版没有外部依赖；3D 版首次加载约 1 秒（模型内嵌）。
- 所有数值、文案都写在各自的 `app.js` / HTML 里，改完重新打包即可。

## License

MIT，见 [LICENSE](LICENSE)。课件中的物理公式与表述参考公开教材，仅供教学使用。
