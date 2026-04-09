---
title: Swift 月考项目 - 学生成绩管理系统开发
date: 2026-03-02 21:35:13
tags:
  - Swift
  - 项目
  - 学生管理
  - 控制台应用
categories:
  - Swift
---

# Swift 月考项目 - 学生成绩管理系统开发

## 一、题目说明

本项目要求基于 Swift 5.0+ 开发一个带交互式菜单的学生成绩管理系统，综合运用 Swift 基础核心知识点（元组、可选型、运算符、字符串、循环、分支、函数等），通过控制台菜单交互实现学生信息的结构化存储、多维度查询、成绩统计、评优资格校验等功能，考察对 Swift 基础语法的综合应用能力与交互式程序开发思维。
<!-- more -->
**分组要求**：每组3-4人

## 二、核心要求

### 1. 数据结构定义（基础层）

#### 必选属性：
- 姓名（String）
- 学号（String，唯一标识）
- 年龄（Int）
- 语文成绩（Int）
- 数学成绩（Int）
- 英语成绩（Int）

#### 可选型属性：
- 是否缺考（Bool?）
- 评优备注（String?）

#### 数据结构定义：

```swift
typealias StudentInfo = (
    name: String,          // 姓名
    studentID: String,     // 学号（唯一）
    age: Int,              // 年龄
    chinese: Int,          // 语文成绩
    math: Int,             // 数学成绩
    english: Int,          // 英语成绩
    isAbsent: Bool?,       // 是否缺考（可选型）
    awardRemark: String?   // 评优备注（可选型）
)
```

#### 场景示例：

```swift
// 场景1：全科目及格、平均分≥80（有评优资格）
let student1: StudentInfo = ("张三", "2023001", 18, 92, 88, 90, false, "成绩优异")

// 场景2：有科目不及格（无评优资格）
let student2: StudentInfo = ("李四", "2023002", 17, 85, 58, 78, false, "数学需补考")

// 场景3：存在缺考（无评优资格）
let student3: StudentInfo = ("王五", "2023003", 18, 0, 95, 89, true, "语文缺考")

// 场景4：可选属性全为 nil（无评优资格）
let student4: StudentInfo = ("赵六", "2022004", 19, 75, 79, 82, nil, nil)
```

### 2. 交互式菜单设计（交互层）

#### 菜单展示效果：

```
==================== 学生成绩管理系统 ====================
请选择操作（输入数字序号）：
1. 查询所有学生完整信息
2. 按学号查询单个学生信息
3. 统计指定科目成绩（语文/数学/英语）
4. 筛选有评优资格的学生
5. 筛选单科成绩≥90分的学生
6. 查看系统数据汇总（总人数、平均分等）
0. 退出系统
=========================================================
```

#### 输入校验要求：
- 输入非数字/不在 0-6 范围内 → 提示「输入错误，请重新选择」
- 重新显示菜单，不退出程序

#### 循环逻辑要求：
- 执行完非「0」功能后，打印「按回车键返回主菜单...」
- 接收回车输入后，重新显示菜单

### 3. 核心功能实现（功能层）

#### 功能1：查询所有学生完整信息
- 遍历学生数组，格式化打印每个学生的信息卡片
- 可选属性 nil 替换为「无」，平均分保留 1 位小数
- 排版清晰（用分隔符区分不同学生）

#### 功能2：按学号查询单个学生信息
- 接收用户输入的学号，遍历数组匹配对应学生
- 找到：打印完整信息卡片；未找到：提示「未查询到学号为 XXX 的学生」

#### 功能3：统计指定科目成绩
- 接收用户输入的科目名称（「语文」/「数学」/「英语」），校验输入合法性
- 统计该科目的：最高分、最低分、平均分（保留 1 位小数）、及格人数（≥60分）

#### 功能4：筛选有评优资格的学生
- 评优条件：所有科目及格（≥60分）且平均分≥80分且无缺考
- 打印符合条件的学生信息，按平均分从高到低排序

#### 功能5：筛选单科成绩≥90分的学生
- 接收用户输入的科目名称，校验输入合法性
- 打印该科目成绩≥90分的学生信息，按成绩从高到低排序

#### 功能6：查看系统数据汇总
- 统计总学生人数
- 统计各科目平均分（保留 1 位小数）
- 统计有评优资格的学生人数
- 统计缺考学生人数

## 三、项目实现思路

### 1. 数据存储
- 使用数组存储学生信息：`var students: [StudentInfo] = []`
- 初始化时添加示例数据，方便测试

### 2. 菜单循环
- 使用 `while true` 循环实现菜单的持续显示
- 根据用户输入执行对应功能
- 输入 0 时退出循环

### 3. 功能实现
- **查询功能**：使用 `for-in` 循环遍历学生数组
- **统计功能**：使用 `reduce` 方法计算总和，使用 `max()` 和 `min()` 方法获取最值
- **筛选功能**：使用 `filter` 方法筛选符合条件的学生
- **排序功能**：使用 `sorted` 方法按指定条件排序

### 4. 输入处理
- 使用 `readLine()` 读取用户输入
- 对输入进行类型转换和合法性校验
- 处理输入错误的情况

## 四、代码示例

### 主程序结构

```swift
// 定义学生信息类型
typealias StudentInfo = (
    name: String,
    studentID: String,
    age: Int,
    chinese: Int,
    math: Int,
    english: Int,
    isAbsent: Bool?,
    awardRemark: String?
)

// 初始化学生数据
var students: [StudentInfo] = [
    ("张三", "2023001", 18, 92, 88, 90, false, "成绩优异"),
    ("李四", "2023002", 17, 85, 58, 78, false, "数学需补考"),
    ("王五", "2023003", 18, 0, 95, 89, true, "语文缺考"),
    ("赵六", "2022004", 19, 75, 79, 82, nil, nil)
]

// 主菜单循环
while true {
    // 显示菜单
    print("==================== 学生成绩管理系统 ====================")
    print("请选择操作（输入数字序号）：")
    print("1. 查询所有学生完整信息")
    print("2. 按学号查询单个学生信息")
    print("3. 统计指定科目成绩（语文/数学/英语）")
    print("4. 筛选有评优资格的学生")
    print("5. 筛选单科成绩≥90分的学生")
    print("6. 查看系统数据汇总（总人数、平均分等）")
    print("0. 退出系统")
    print("=========================================================")
    
    // 读取用户输入
    guard let input = readLine(), let choice = Int(input) else {
        print("输入错误，请重新选择")
        continue
    }
    
    // 根据选择执行对应功能
    switch choice {
    case 0:
        print("退出系统")
        exit(0)
    case 1:
        // 查询所有学生完整信息
        showAllStudents()
    case 2:
        // 按学号查询单个学生信息
        searchStudentByID()
    case 3:
        // 统计指定科目成绩
        statisticsBySubject()
    case 4:
        // 筛选有评优资格的学生
        filterAwardStudents()
    case 5:
        // 筛选单科成绩≥90分的学生
        filterHighScoreStudents()
    case 6:
        // 查看系统数据汇总
        showSystemSummary()
    default:
        print("输入错误，请重新选择")
    }
    
    // 按回车键返回主菜单
    print("按回车键返回主菜单...")
    _ = readLine()
}
```

### 功能函数示例

```swift
// 显示所有学生信息
func showAllStudents() {
    for student in students {
        print("\n-------------------- 学生信息 --------------------")
        print("姓名：\(student.name)")
        print("学号：\(student.studentID)")
        print("年龄：\(student.age)")
        print("语文：\(student.chinese)")
        print("数学：\(student.math)")
        print("英语：\(student.english)")
        print("是否缺考：\(student.isAbsent ?? false ? "是" : "否")")
        print("评优备注：\(student.awardRemark ?? "无")")
        
        let average = Double(student.chinese + student.math + student.english) / 3.0
        print("平均分：\(String(format: "%.1f", average))")
        print("--------------------------------------------------")
    }
}

// 按学号查询学生
func searchStudentByID() {
    print("请输入学号：")
    guard let inputID = readLine() else {
        print("输入错误")
        return
    }
    
    if let student = students.first(where: { $0.studentID == inputID }) {
        print("\n-------------------- 学生信息 --------------------")
        print("姓名：\(student.name)")
        print("学号：\(student.studentID)")
        print("年龄：\(student.age)")
        print("语文：\(student.chinese)")
        print("数学：\(student.math)")
        print("英语：\(student.english)")
        print("是否缺考：\(student.isAbsent ?? false ? "是" : "否")")
        print("评优备注：\(student.awardRemark ?? "无")")
        
        let average = Double(student.chinese + student.math + student.english) / 3.0
        print("平均分：\(String(format: "%.1f", average))")
        print("--------------------------------------------------")
    } else {
        print("未查询到学号为 \(inputID) 的学生")
    }
}
```

## 五、项目要求

1. **代码规范**：代码结构清晰，命名规范，添加必要的注释
2. **功能完整**：实现所有要求的功能，处理边界情况
3. **交互友好**：菜单界面美观，用户输入有提示，错误处理完善
4. **代码质量**：逻辑清晰，避免冗余代码，使用 Swift 语言特性

## 六、总结

本项目是一个综合性的 Swift 基础练习，通过实现学生成绩管理系统，巩固 Swift 的核心知识点，包括：

- 元组的使用
- 可选型的处理
- 控制流（循环、分支）
- 函数的定义和调用
- 数组的操作（遍历、筛选、排序）
- 字符串处理
- 输入输出操作

通过完成这个项目，不仅可以熟悉 Swift 的基本语法，还能培养交互式程序开发的思维，为后续的 iOS 应用开发打下基础。
