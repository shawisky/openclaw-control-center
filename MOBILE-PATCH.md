# OpenClaw Control Center 移动端适配补丁

## 概述

为 `TianyiDataScience/openclaw-control-center` 项目添加手机竖屏支持。

## 需要修改的文件

### 1. src/ui/server.ts - HTML 头部添加 viewport

**位置**: 约第 7945-7950 行（`<!doctype html>` 后的 `<head>` 部分）

**修改前**:
```html
<head>
  <meta charset="utf-8" />
  <title>OpenClaw Control Center</title>
```

**修改后**:
```html
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=5, user-scalable=no" />
  <title>OpenClaw Control Center</title>
```

---

### 2. src/ui/server.ts - CSS 添加移动端断点

**位置**: 在现有 `@media (max-width: 740px)` 规则之后添加

**新增 CSS**:
```css
/* ===== 移动端优化 ===== */

/* 小平板横屏 / 大屏手机横屏 */
@media (max-width: 740px) {
  .section-title { font-size: 22px; }
  .overview-kpi-value { font-size: 28px; }
  .card { padding: 12px; }
}

/* 手机竖屏 - 主要优化断点 */
@media (max-width: 480px) {
  /* 基础布局 */
  .app-shell {
    grid-template-columns: 1fr !important;
    padding: 12px !important;
    gap: 12px !important;
  }
  
  .sidebar {
    position: fixed !important;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    transform: translateY(-100%);
    transition: transform 0.2s ease;
    max-height: 80vh;
    border-radius: 0 0 18px 18px;
  }
  
  .sidebar.open {
    transform: translateY(0);
  }
  
  /* 品牌栏 - 添加汉堡菜单 */
  .brand {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .brand h1 {
    font-size: 18px;
    flex: 1;
    text-align: center;
  }
  
  .menu-toggle {
    display: flex !important;
    padding: 8px;
    background: none;
    border: none;
    cursor: pointer;
  }
  
  /* 面板内容 */
  .panel {
    margin-top: 60px; /* 为固定 sidebar 留空间 */
  }
  
  /* 卡片和 KPI */
  .card {
    padding: 12px;
    border-radius: 12px;
  }
  
  .overview-kpi-grid {
    grid-template-columns: 1fr !important;
    gap: 10px;
  }
  
  .overview-kpi-value {
    font-size: 24px;
  }
  
  .overview-kpi-label {
    font-size: 12px;
  }
  
  /* 按钮触摸优化 */
  button,
  .btn,
  a.btn {
    min-height: 44px;
    min-width: 44px;
    padding: 12px 16px;
    font-size: 15px;
    touch-action: manipulation;
  }
  
  .icon-btn {
    min-height: 44px;
    min-width: 44px;
    padding: 12px;
  }
  
  .icon-btn svg {
    width: 20px;
    height: 20px;
  }
  
  /* 状态芯片 */
  .badge,
  .status-chip {
    min-height: 32px;
    padding: 6px 12px;
    font-size: 13px;
  }
  
  /* 表格优化 */
  table {
    display: block;
    overflow-x: auto;
    white-space: nowrap;
    -webkit-overflow-scrolling: touch;
  }
  
  .task-table,
  .session-table {
    min-width: 600px;
  }
  
  /* 导航链接 */
  .nav-links {
    flex-wrap: nowrap;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    padding-bottom: 8px;
  }
  
  .nav-link {
    flex-shrink: 0;
    padding: 10px 14px;
  }
  
  /* 文本大小 */
  .section-title {
    font-size: 20px;
  }
  
  h1 { font-size: 20px; }
  h2 { font-size: 18px; }
  h3 { font-size: 16px; }
  
  p, li, div {
    font-size: 15px;
    line-height: 1.5;
  }
  
  /* 隐藏非关键元素 */
  .meta {
    font-size: 11px;
  }
  
  .section-blurb {
    display: none;
  }
  
  /* 协作大厅 */
  .collaboration-summary-grid {
    grid-template-columns: 1fr !important;
  }
  
  .collaboration-avatar {
    width: 48px;
    max-width: 48px;
  }
  
  /* 任务面板 */
  .task-hub-board-grid {
    grid-template-columns: 1fr !important;
  }
  
  .task-card {
    padding: 12px;
  }
  
  /* 文件工作区 */
  .file-editor-textarea {
    min-height: 200px;
    font-size: 14px;
  }
  
  /* 头像网格 */
  .avatar-grid {
    grid-template-columns: repeat(3, 1fr) !important;
    gap: 8px;
  }
  
  .avatar-upload-list {
    grid-template-columns: repeat(3, 1fr) !important;
  }
  
  /* 底部操作栏 */
  .section-head-actions {
    flex-wrap: wrap;
    gap: 8px;
  }
  
  .section-head-actions .btn {
    flex: 1;
    min-width: 120px;
  }
}

/* 小屏手机 */
@media (max-width: 375px) {
  .app-shell {
    padding: 8px !important;
  }
  
  .card {
    padding: 10px;
  }
  
  .overview-kpi-value {
    font-size: 22px;
  }
  
  button,
  .btn,
  a.btn {
    padding: 10px 14px;
    font-size: 14px;
  }
  
  .avatar-grid {
    grid-template-columns: repeat(2, 1fr) !important;
  }
}

/* 触摸设备优化 */
@media (hover: none) and (pointer: coarse) {
  /* 增大悬停效果的热区 */
  .card:hover,
  .btn:hover,
  .nav-link:hover {
    transform: none; /* 禁用悬停位移 */
    box-shadow: var(--shadow-soft);
  }
  
  /* 激活状态 */
  .card:active,
  .btn:active,
  .nav-link:active {
    transform: scale(0.98);
    opacity: 0.9;
  }
  
  /* 禁用不必要的过渡 */
  * {
    transition-duration: 0.1s !important;
  }
}

/* 显示汉堡菜单按钮（仅在移动端） */
.menu-toggle {
  display: none;
}

@media (max-width: 480px) {
  .menu-toggle {
    display: flex;
  }
  
  .brand-actions {
    margin-right: 8px;
  }
}
```

---

### 3. src/ui/server.ts - 添加移动端 JavaScript

**位置**: 在 `<script>` 标签中（现有 theme 和 restore 脚本之后）

**新增 JavaScript**:
```javascript
/* Mobile Menu Toggle */
(() => {
  const menuToggle = document.querySelector('.menu-toggle');
  const sidebar = document.querySelector('.sidebar');
  
  if (!menuToggle || !sidebar) return;
  
  menuToggle.addEventListener('click', () => {
    sidebar.classList.toggle('open');
    menuToggle.setAttribute('aria-expanded', sidebar.classList.contains('open'));
  });
  
  // 点击外部关闭菜单
  document.addEventListener('click', (e) => {
    if (!sidebar.contains(e.target) && !menuToggle.contains(e.target)) {
      sidebar.classList.remove('open');
      menuToggle.setAttribute('aria-expanded', 'false');
    }
  });
})();

/* 触摸优化 - 防止双击缩放 */
document.addEventListener('dblclick', (e) => {
  e.preventDefault();
}, { passive: false });

/* 滚动优化 */
if ('ontouchstart' in window) {
  document.body.classList.add('touch-device');
}
```

---

### 4. src/ui/server.ts - 汉堡菜单图标

**位置**: 在 `.brand` div 内，`<h1>` 之前

**新增 HTML**:
```html
<button class="menu-toggle" type="button" aria-label="Toggle menu" aria-expanded="false">
  <svg viewBox="0 0 24 24" fill="none" width="24" height="24">
    <path d="M3 6h18M3 12h18M3 18h18" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
  </svg>
</button>
```

---

## 测试清单

- [ ] iPhone SE (375x667)
- [ ] iPhone 12/13/14 (390x844)
- [ ] iPhone 12/13/14 Pro Max (428x926)
- [ ] Android 小屏 (360x640)
- [ ] Android 标准 (393x873)
- [ ] iPad 竖屏 (768x1024)
- [ ] iPad 横屏 (1024x768)

---

## 预期效果

| 设备 | 修改前 | 修改后 |
|------|--------|--------|
| iPhone 竖屏 | ❌ 需要缩放，无法操作 | ✅ 完整可用 |
| Android 竖屏 | ❌ 需要缩放，无法操作 | ✅ 完整可用 |
| iPad 竖屏 | ⚠️ 拥挤，部分功能受限 | ✅ 良好 |
| iPad 横屏 | ✅ 可用 | ✅ 良好 |
| 桌面 | ✅ 完美 | ✅ 完美 |

---

## 注意事项

1. **表格数据**: 复杂表格仍需要横向滚动，这是可接受的
2. **多列布局**: 在手机上会变为单列，信息密度降低但可读
3. **侧边栏**: 改为顶部抽屉式，节省空间
4. **按钮尺寸**: 增大到 44px 以上，符合 WCAG 标准

---

## 回滚方案

如果出现问题，只需删除：
1. viewport meta 标签
2. 新增的 `@media` 规则
3. 新增的 JavaScript
4. 汉堡菜单 HTML

恢复原有代码即可。
