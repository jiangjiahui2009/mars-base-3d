# MEMORY.md - Long-term memory

## 项目记录

### StarCraft 基地场景 (2026-03-14 ~)
- 文件: `/Users/admin/WorkBuddy/20260314090450/index.html`
- 用户偏好: 追求游戏原版还原度，参考了 SC2 高清截图的比例
- 关键比例: CC 5×4 tiles, SCV ~1 tile, 8 簇矿(4大4小标准布局)
- 用户之前反馈过: 画面要居中、可拖动、矿车速度不要太快
- 当前版本: Three.js 3D 版本（2026-04-02 更新）
  - 使用 Three.js r128 + OrbitControls（可旋转/缩放/平移）
  - SCV 模型: 真实 GLTF 模型 scv_model.glb（来自下载文件夹 9cc8f205...，PBR 版）
    - 用 GLTFLoader 加载，自动缩放至 ~1 TILE，底部贴地
    - 有加载进度条 + 失败回退（几何体方块 SCV）
  - CC 和矿簇: 仍为手工几何体拼接
  - 灯光: 方向光阴影 + 矿区蓝色点光 + CC装卸台暖光
  - 粒子系统: 采矿飞溅 + 卸载粒子 + 环境粉尘
  - 地形: 程序化纹理(荒漠噪点/裂缝/碎石/车辙)
- 矿堆布局（2026-04-03 最新，用户手动选取坐标）:
  - 8个主矿堆使用用户通过坐标拾取器选取的固定坐标：
    (-17.6,5.7), (-17.7,8.3), (-17.4,10.2), (-17.2,11.7),
    (-15.2,13.7), (-14.3,14.9), (-13.1,15.5), (-12.0,15.6)
  - 不再使用极坐标弧形公式，直接硬编码 positions 数组
- CC 位置: (-2.7, 6.9)
- 水晶模型: 使用 crystal_model.glb（来自下载文件夹/水晶1/base_basic_pbr.glb）
  - 自发光 emissive 0x4488ff intensity 0.8 + 地面光晕
  - 注意：不要给每个矿堆加 PointLight，会导致性能卡顿
- 飞行器: aircraft_model.glb（来自下载文件夹/飞行器/base_basic_pbr.glb），位于 (8.5, 14.7)，朝向 CC
- 建筑模型: building2_model.glb，位于 (5.7, -4.5)
- UI 资源面板: "TOKEN" 替代 "MINERALS"，显示具体数字（约1亿级别上下千万波动）
- 矿区点击面板: 左侧弹出"AI大模型"列表，显示 arena.ai 排名前16的模型
  - 图标使用 Google faviconV2 服务（gstatic.com），最可靠
  - 踩坑：icons8.com 和 simpleicons.org 的图标URL有很多404，不建议使用
- 宇航员面板: 右侧弹出"Human Hub"，显示元技能和专技能列表
- CC面板: 底部弹出，4列布局（资源/生产物/能力与解锁/火星基地）
- 踩坑记录:
  - hoverY 变量在删除飞行器震动效果后残留引用 → 用 model.position.y 替代
  - crystal_model.glb 与 mineral_model.glb 文件名不一致导致404 → 统一为 crystal_model.glb
- 历史迭代: 等距Canvas → 俯视像素画Canvas → SC2比例Canvas → Three.js 3D
