# 3D Web Viewer

一个纯前端的轻量级 **Three.js** 3D 模型查看器,单文件 HTML + 本地 ES Module, 无构建步骤, **直接双击 `index.html` 就能跑**。

支持格式: **STL / GLB / OBJ / PLY**

![three](https://img.shields.io/badge/Three.js-r160-000?logo=three.js)
![license](https://img.shields.io/github/license/xiaohe4966/3d-web)

---

## ✨ 功能特性

| 模块 | 能力 |
|---|---|
| **加载** | 拖拽 / 点击选择 / URL 远程加载 / File API 直接读 buffer |
| **格式** | STL · GLB / GLTF · OBJ · PLY |
| **交互** | 鼠标拖拽旋转 · 滚轮缩放 · 右键平移 · 自适应 DPI |
| **视图** | 自动取景 · 重置视角 · 4 种背景色 (黑/白/灰/渐变蓝) |
| **显示** | 实体 / 线框 / 棋盘格地面 · 5 种主题色 · 切换阴影 |
| **姿态** | XYZ 坐标输入精确平移 / 旋转 / 缩放 |
| **导出** | 一键 PNG 截图 (当前画布) |
| **信息** | 实时 FPS · 三角面数 · 顶点数 · 包围盒尺寸 · 当前文件名 |
| **快捷键** | `Space` 实体/线框 · `W` 切换网格 · `G` 自动取景 · `Esc` 重置 |

---

## 🚀 快速开始

### 方式一: 直接打开 (推荐)

```bash
git clone https://github.com/xiaohe4966/3d-web.git
cd 3d-web
open index.html        # macOS
# 或直接双击 index.html
```

> **不需要 npm / node / 任何构建**. 浏览器原生支持 ES Module + importmap.

### 方式二: 本地静态服务器 (解决某些浏览器 `file://` 协议限制)

```bash
python3 -m http.server 8080
# 或
npx serve .
# 然后浏览器访问 http://localhost:8080
```

---

## 📂 项目结构

```
3d-web/
├── index.html              # 单页应用入口 (HTML + CSS + JS 全部内联, 875 行)
├── pan.stl                 # 示例 STL 模型 (27 KB)
└── js/
    ├── three.module.js     # Three.js r160 本地化副本 (1.2 MB, 不走 CDN)
    ├── controls/
    │   └── OrbitControls.js
    ├── loaders/
    │   ├── STLLoader.js
    │   ├── GLTFLoader.js
    │   ├── OBJLoader.js
    │   └── PLYLoader.js
    └── utils/
        └── BufferGeometryUtils.js
```

### 为什么本地化 Three.js?

`index.html` 用 [importmap](https://developer.mozilla.org/docs/Web/HTML/Element/script/type/importmap) 把 `three` / `three/addons/...` 这些 bare specifier 直接指到 `js/` 下的本地文件:

```html
<script type="importmap">
{
  "imports": {
    "three": "./js/three.module.js",
    "three/addons/": "./js/"
  }
}
</script>
```

好处:

- ✅ **离线可用** — 完全不依赖 CDN, 断网也能跑
- ✅ **加载稳定** — 没有 `unpkg.com` / `jsdelivr` 偶发抖动
- ✅ **可审查** — 第三方代码就在仓库里, 没有运行时供应链风险

代价: 仓库体积 ~1.4 MB (主要是 `three.module.js`), 远小于一张普通 PNG.

---

## 🎮 使用说明

### 加载模型

| 方式 | 操作 |
|---|---|
| **拖拽** | 把 `.stl` / `.glb` / `.obj` / `.ply` 文件直接拖进浏览器窗口 |
| **点击** | 点左上角 **「📁 打开文件」** 按钮, 系统选文件 |
| **URL** | 控制台执行 `window.__viewer.loadFromUrl('https://...model.glb')` |
| **Buffer** | 控制台 `window.__viewer.loadFromBuffer(arrayBuffer, 'foo.stl')` |

> 加载完模型后视图会自动取景 (zoom to fit), 不需要手动调.

### 视图控制

- **左键拖**: 旋转
- **右键拖**: 平移
- **滚轮**: 缩放
- **`G` 键**: 重新自动取景
- **`Esc` 键**: 重置视角

### 姿态精调 (右下角面板)

精确输入 X/Y/Z 数值, 实时应用平移 / 旋转 / 缩放, 适合对齐多模型或截图.

### 调试

打开 DevTools Console, 可访问全局句柄:

```js
window.__viewer.scene          // THREE.Scene
window.__viewer.camera         // THREE.PerspectiveCamera
window.__viewer.renderer       // THREE.WebGLRenderer
window.__viewer.controls       // OrbitControls 实例
window.__viewer.loadFromUrl(url)
window.__viewer.loadFromBuffer(buffer, 'filename.stl')
```

---

## 🛠 浏览器兼容性

需要支持:

- ES Modules (`<script type="module">`)
- importmap (Chrome 89+, Edge 89+, Safari 16.4+, Firefox 108+)
- WebGL 2 (Three.js r160 最低要求)

实测: Chrome / Edge / Safari 16+ / Firefox 108+ 全绿. 移动端可用但 `OrbitControls` 触屏体验一般.

---

## 📜 许可

MIT License — 详见 [LICENSE](LICENSE).

内含 Three.js r160, 也遵循 MIT.

---

## 🙏 致谢

- [Three.js](https://threejs.org/) — mrdoob & contributors
- [OrbitControls / STLLoader / GLTFLoader / OBJLoader / PLYLoader](https://github.com/mrdoob/three.js/tree/r160/examples/jsm) — Three.js 官方 addons
