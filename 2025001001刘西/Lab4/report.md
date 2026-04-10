# Lab4：Dice Roller 实验报告

## 一、应用界面结构说明

本应用采用 Jetpack Compose 构建，界面结构如下：

- **MainActivity**：入口活动，负责设置 Compose 主题并调用主界面
- **DiceRollerApp()**：主 Composable 函数，包含 Column 容器和 DiceWithButtonAndImage 组件
- **DiceWithButtonAndImage()**：可组合函数，显示骰子图片和 Roll 按钮

界面采用 Column 垂直布局，骰子图片在上，按钮在下，水平和垂直居中排列。

## 二、Compose 状态保存骰子结果

使用 `remember { mutableStateOf(1) }` 创建可变状态：

```kotlin
var result by remember { mutableStateOf(1) }
```

当 `result` 的值发生变化时，Compose 会自动触发重组（Recomposition），界面会自动更新为新的图片。

## 三、根据点数切换图片资源

使用 `when` 表达式将点数映射到不同的图片资源：

```kotlin
val imageResource = when (result) {
    1 -> R.drawable.dice_1
    2 -> R.drawable.dice_2
    3 -> R.drawable.dice_3
    4 -> R.drawable.dice_4
    5 -> R.drawable.dice_5
    else -> R.drawable.dice_6
}
```

然后通过 `painterResource()` 加载图片并在 Image 组件中显示。

## 四、断点设置与观察

### 断点位置

1. **DiceRollerApp() 中的 result 赋值处**：`result = (1..6).random()`
2. **DiceWithButtonAndImage() 中的 imageResource 赋值处**

### 观察内容

在调试过程中，我观察到：
- 当点击 Roll 按钮时，`result` 的值会从 1 变为随机生成的 1-6 之间的整数
- `imageResource` 会根据新的 `result` 值选择对应的 drawable 资源
- 调试窗口中 `result$delegate` 变量显示当前骰子点数

## 五、调试工具使用体会

### Step Into
- 进入函数调用内部，查看具体实现细节
- 用于查看 `mutableStateOf` 的内部实现

### Step Over
- 逐行执行代码，不进入函数内部
- 适合快速浏览代码执行流程

### Step Out
- 执行完当前函数并返回上一级调用
- 用于退出嵌套的函数调用

## 六、遇到的问题与解决过程

1. **图片资源问题**：最初直接将老师仓库的 xml 文件复制到 drawable 目录
   - 解决：从 GitHub 获取原始 XML 内容，确保文件格式正确

2. **Compose 状态理解**：初期对状态驱动界面刷新机制不熟悉
   - 解决：通过调试观察 `result` 变量变化，理解了 Compose 的重组机制

## 七、结论

### 为什么按钮点击后图片能够自动刷新？

因为使用了 Compose 的状态管理机制：
- `mutableStateOf(1)` 创建一个响应式状态
- 当按钮点击触发 `onRollClick` 回调时，`result` 状态被更新
- Compose 检测到状态变化，自动触发重组，重新执行 DiceWithButtonAndImage 函数
- 由于 `imageResource` 依赖于 `result`，图片资源会相应更新

### 调试器中看到的变量值与界面结果是否一致？

是的，完全一致。调试器中观察到的 `result` 值（1-6）与界面上显示的骰子点数和图片是对应的。这验证了 Compose 状态驱动的界面更新机制是正确工作的。
