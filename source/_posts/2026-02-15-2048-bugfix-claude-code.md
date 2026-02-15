---
title: 🔧 2048 游戏 Bug 修复记录：6 个坑与解决方案
date: 2026-02-15 14:30:01
tags:
  - 2048
  - Bug修复
  - JavaScript
  - 代码审查
categories:
  - 技术笔记
---

## 🔍 背景

使用 Claude Code 对 2048 游戏进行了一次系统性代码审查，结果发现了 **6 个 bug**——从严重的逻辑错误到细微的体验问题。记录一下这些坑以及修复方案。

---

## 🚨 严重 Bug

### 1. localStorage 类型陷阱

**问题代码：**
```javascript
this.bestScore = localStorage.getItem('best2048') || 0;
```

**坑在哪？**  
`localStorage.getItem()` 返回的是**字符串**。当最高分是 `"100"` 时，与新分数 `50` 比较：
```javascript
"100" > 50  // true，没问题
"20" > 100  // false，也没问题
// 但字符串比较时：
"2" > "10"  // true！因为 "2" > "1"
```

**修复：**
```javascript
this.bestScore = parseInt(localStorage.getItem('best2048')) || 0;
```

---

### 2. 调用不存在的方法

**问题：**  
`slide()` 方法里调用了 `this.checkWin()`，但这个方法**根本不存在**。

**后果：**  
玩家好不容易达到 2048，游戏直接报错崩溃 💥

**修复：**
```javascript
checkWin() {
    for (let r = 0; r < this.size; r++) {
        for (let c = 0; c < this.size; c++) {
            if (this.grid[r][c] === 2048) return true;
        }
    }
    return false;
}
```

---

## ⚠️ 中等 Bug

### 3. 新方块动画失效

**问题：**  
CSS 里定义了 `.tile.new` 的弹出动画，但从未被应用。

**原因：**  
`addRandomTile()` 返回了新方块位置，但 `render()` 没有使用这个数据。

**修复：**
```javascript
// render() 方法中
const isNewTile = this.newTileInfo && 
    this.newTileInfo.r === r && 
    this.newTileInfo.c === c;
tile.className = `tile tile-${value}${isNewTile ? ' new' : ''}`;

// 动画只播放一次
this.newTileInfo = null;
```

---

### 4. 移动端触摸体验差

**问题：**
- 滑动时页面跟着滚动
- 偶尔触摸事件异常

**修复：**
```javascript
// 阻止默认滚动行为
gameContainer.addEventListener('touchmove', (e) => {
    e.preventDefault();
}, { passive: false });

// 添加边界检查
if (startX === undefined || startY === undefined) return;

// 重置触摸状态
startX = undefined;
startY = undefined;
```

---

## 📝 轻微问题

### 5. 状态重复初始化

`this.hasWon = false` 在 `init()` 里写了两遍 😅

### 6. 胜利无提示

达到 2048 后没有任何反馈，玩家不知道自己赢了。

**修复：**
```javascript
if (this.checkWin() && !this.hasWon) {
    this.hasWon = true;
    setTimeout(() => {
        this.showOverlay('你赢了! 🎉');
    }, 300);
}
```

---

## 📊 修复统计

| 级别 | 数量 | 说明 |
|------|------|------|
| 🔴 严重 | 2 | 类型错误、缺失方法 |
| 🟡 中等 | 2 | 动画失效、触摸问题 |
| 🟢 轻微 | 2 | 重复代码、缺失提示 |

**代码变更：** `main.js` +50 行 / -6 行

---

## 🎮 体验地址

**在线试玩：** [https://2048-game-sigma-wheat.vercel.app](https://2048-game-sigma-wheat.vercel.app)

**源码：** [https://github.com/occcat/2048-game](https://github.com/occcat/2048-game)

---

## 💡 经验

1. **永远不要相信 localStorage** —— 返回的都是字符串
2. **调用前检查** —— 未定义的方法只有调用时才报错
3. **移动端要特判** —— 触摸事件需要 `preventDefault()`
4. **系统性审查有价值** —— 自己写的时候容易忽略 obvious 的问题

现在游戏可以正常玩了，去试试能不能达到 2048 吧！🎯
