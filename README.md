# 格物 3D · 制冷机

> 「格物致知」——从看得见的装置，理解看不见的规律。

一份 **电脑 / 手机自适应** 的制冷机 3D 交互课件：宽屏左右分栏，窄屏竖排滚动。
单文件 HTML，three.js 与 Blender 模型全部内嵌，**离线可用**，双击即可上课。

![桌面版](previews/desktop.png)

<img src="previews/mobile.png" width="360" alt="手机版">

---

## 内容

- **3D 机器**：冷藏室（蒸发器盘管）、压缩机、冷凝器、毛细管，全部由 Blender 脚本建模。
- **状态分色**：高温气体（红）→ 高压液体（橙）→ 低温低压（浅蓝）→ 回气（蓝）；管路做成半截面，能看到管内流动的制冷剂。
- **能量条**：Q₂ / |W| / Q₁ 与制冷系数 e，拖动 T₂ 滑杆实时重算。
- **四步**：压缩 → 冷凝 → 节流 → 蒸发，点一下跳到该过程，说明卡同步。
- **讲解卡**：制冷系数公式（e = Q₂/|W| = Q₂/(Q₁−Q₂) = T₂/(T₁−T₂)，含代入值）、Q₁ = Q₂ + |W|、为什么必须做功、怎么提高制冷系数。
- **颜色图例**：红=高温气体、橙=高压液体、蓝=低温气体。

## 使用

- **本地**：双击 `index.html`（或用 Chrome / Edge / Firefox 打开）。
- **在线**：推到 GitHub 后开启 Pages，根目录的 `index.html` 就是课件。

### 部署到 GitHub Pages

1. 新建仓库，把本目录内容推上去（`index.html` 在根目录）；
2. **Settings → Pages** → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`；
3. 访问 `https://<用户名>.github.io/<仓库名>/`。

## 自适应规则

- **≥ 960 px**：左列 3D 舞台（可拖拽旋转、滚轮缩放），右列可滚动的卡片；
- **< 960 px**：3D 舞台在顶部（约 42vh），整页竖排滚动；单指旋转、双指缩放。
- 开关只用一个 CSS 断点 + `ResizeObserver` 跟随容器尺寸，一套 DOM、一套 JS。

## 目录结构

```
.
├── index.html              课件（单文件，2.04 MB）
├── previews/               桌面版 / 手机版预览
├── docs/guide.md           操作、讲法与改造说明
└── src/
    ├── blender/            Blender 脚本（程序化建模 + 导出 GLB）
    ├── build/              打包脚本（把 three.js 与 GLB 内联成单文件）
    └── dev-glb-resp/       拆分源码（index.html / styles.css / app.js / fridge.glb / vendor）
```

## 重新构建

需要 Node.js；改模型需要 Blender 4.x/5.x。

```bash
# 1) 重新导出 GLB（可选）
blender -b -P src/blender/fridge_proto.py
blender -b -P src/blender/make_glb.py

# 2) 打包成单文件
node src/build/build-glb-resp.js
```

> `src/build/build-glb-resp.js` 默认从 `dev-glb-resp/` 读取，输出到上一级目录；按需改路径。

## 说明

- 模型是教学示意：尺寸、管径、行程有夸张，便于看清结构。
- 制冷系数显示的是卡诺理论值 T₂/(T₁−T₂)，实际冰箱会低一些。
- 首次加载约 1 秒（模型内嵌），之后完全离线。

## License

MIT，见 [LICENSE](LICENSE)。物理公式与表述参考公开教材，仅供教学使用。
