---
name: starcraft-base-scene
overview: 制作一个星际争霸风格的纯前端展示页：2.5D 透视效果的基地地图场景，背景包含基地建筑和矿产，4辆采矿车在矿产与基地之间自动来回移动，使用 Canvas 绘制所有元素并实现动画循环。
design:
  architecture:
    framework: html
  styleKeywords:
    - Sci-Fi Military
    - Isometric 2.5D
    - Dark Industrial
    - StarCraft Terran
    - Glowing Minerals
    - Canvas Animation
  colorSystem:
    primary:
      - "#4A7AB5"
      - "#7B5EA7"
      - "#3CA0E0"
    background:
      - "#1A1A2E"
      - "#2D2D3F"
      - "#3B3B20"
    text:
      - "#E0E0E0"
    functional:
      - "#00FF88"
      - "#FF6B35"
      - "#FFDD44"
todos:
  - id: create-html-structure
    content: 创建 index.html 基础结构，Canvas 全屏适配
    status: completed
  - id: implement-isometric-engine
    content: 实现等距投影引擎：坐标转换、Tile 绘制、画家算法排序
    status: completed
    dependencies:
      - create-html-structure
  - id: draw-static-scene
    content: 绘制静态场景：地表纹理、泥土路径、指挥中心、矿簇
    status: completed
    dependencies:
      - implement-isometric-engine
  - id: implement-scv-animation
    content: 实现采矿车状态机和 Bezier 路径移动动画
    status: completed
    dependencies:
      - implement-isometric-engine
  - id: add-visual-effects
    content: 添加视觉效果：矿簇脉冲闪烁、采矿粒子、建筑光效
    status: completed
    dependencies:
      - draw-static-scene
      - implement-scv-animation
---

## Product Overview

一个纯前端展示页面，视觉风格模仿星际争霸游戏界面。页面展示一个基地地图场景，3-5 只采矿车在矿区与基地之间来回移动。

## Core Features

- 等距投影（Isometric）2.5D 透视渲染的基地地图场景
- 代码绘制所有场景元素：指挥中心基地、矿簇、采矿车、地表纹理
- 3-5 只采矿车沿预设路径在矿区与基地之间往返移动
- 采矿车到达矿区时暂停采矿，到达基地时卸载资源
- 深色科幻 Terran 风格配色，营造星际争霸氛围
- 全屏 Canvas 渲染，无外部依赖，纯 HTML/CSS/JS 单文件实现

## Tech Stack

- 纯 HTML5 + CSS3 + JavaScript（ES6+）
- HTML Canvas 2D 作为渲染引擎
- 等距投影（Isometric Projection）模拟 2.5D 透视效果
- requestAnimationFrame 驱动动画循环

## Implementation Approach

使用等距投影在 2D Canvas 上模拟 3D 透视效果。场景地图由基于网格的 Tile 系统构成，每个 Tile 使用菱形绘制。所有元素（基地、矿区、采矿车、地形）均通过 Canvas API 代码绘制，无需任何外部图片资源。

### 等距投影数学

- 屏幕坐标转换：`screenX = (mapX - mapY) * tileWidth / 2`，`screenY = (mapX + mapY) * tileHeight / 2`
- 菱形网格实现类似星际争霸的俯视 2.5D 视角

### 场景构成

1. **地表层**：深褐色/灰色土地纹理网格，不同类型 Tile（裸地、泥土路径）
2. **矿区**：地图左上区域，用蓝紫色晶体块表示矿簇，带闪烁光效
3. **指挥中心**：地图右下区域，大型多边形结构，灰色金属质感
4. **采矿车**：小型方形载具，沿路径移动，带轻微上下颠簸动画
5. **路径**：矿区到基地的弯曲泥土路径

### 采矿车动画状态机

```
Idle -> MovingToMineral -> Mining -> MovingToBase -> Unloading -> Idle
```

- 每辆车有随机初始状态偏移，避免同步
- 移动时沿 Bezier 曲线路径平滑移动
- 采矿时有小型动画效果（闪烁/粒子）

### 渲染顺序（画家算法）

背景网格 -> 路径 -> 基地阴影 -> 基地 -> 矿区阴影 -> 矿区 -> 采矿车（按 Y 坐标排序）-> 前景粒子效果

## Implementation Notes

- 采矿车移动使用预计算的 Bezier 路径点，而非逐帧计算，减少性能开销
- Canvas 尺寸跟随窗口 resize 事件动态调整
- 地形纹理使用 OffscreenCanvas 或离屏 Canvas 缓存静态部分，仅重绘动态元素
- 使用 requestAnimationFrame 确保流畅动画，帧率不设上限（依赖显示器刷新率）

## Directory Structure

```
/Users/admin/WorkBuddy/20260314090450/
└── index.html    # [NEW] 单文件实现，包含全部 HTML/CSS/JS
```

整个项目用单个 HTML 文件实现，内嵌 `<style>` 和 `<script>` 标签，零依赖，可直接浏览器打开。

## Key Code Structures

```
// 等距投影坐标转换
function toScreen(x, y, z) -> { screenX, screenY }

// 绘制等距菱形 Tile
function drawTile(ctx, mapX, mapY, color)

// 绘制基地建筑
function drawCommandCenter(ctx, mapX, mapY)

// 绘制矿区
function drawMineralPatch(ctx, mapX, mapY, time)

// 采矿车状态
enum SCVState { IDLE, MOVING_TO_MINERAL, MINING, MOVING_TO_BASE, UNLOADING }

// 采矿车对象
interface SCV {
  x, y,           // 当前位置
  state,           // SCVState
  pathProgress,    // 0~1 路径进度
  path,            // Bezier 路径点数组
  stateTimer,      // 状态计时器
  speed            // 移动速度
}

// 动画主循环
function gameLoop(timestamp)
```

## Design Style

星际争霸 Terran 科技风。深色科幻色调，金属质感建筑，蓝紫色矿簇发光，棕褐色荒地地表。整体模拟星际争霸游戏中的资源采集基地场景。

### 页面布局

全屏 Canvas 填满整个浏览器窗口，无滚动条，无任何 UI 元素覆盖。Canvas 自动适应窗口大小。

### 场景构图

- **地图中心偏右下**：Terran 指挥中心（Command Center），大型灰色金属建筑
- **地图左上区域**：矿簇区域（Mineral Field），3-5 组蓝紫色发光晶体
- **两者之间**：弯曲的泥土路径，采矿车沿路径移动
- **地面**：深褐/灰色荒地纹理，带随机噪点模拟地表
- **边缘**：自然过渡到深色/迷雾效果

### 视觉层次

1. 地表纹理（静态缓存）
2. 路径（静态缓存）
3. 建筑阴影
4. 基地建筑（多层叠放的等距方块组合）
5. 矿簇（发光晶体，带脉冲闪烁）
6. 采矿车（等距视角小方块，带轮子/机械臂细节）
7. 粒子效果（采矿火花、卸载粒子）

## Agent Extensions

### Skill

- **frontend-design**
- Purpose: 确保前端界面的设计质量，Canvas 绘制效果美观且有质感
- Expected outcome: 生成高质量的等距投影场景绘制代码，包括建筑金属质感、矿簇发光效果、地表纹理等视觉细节