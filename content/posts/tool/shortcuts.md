---
title: "快捷键备忘录"
date: 2020-03-21T14:06:58+08:00
lastmod: 2020-03-21T14:06:58+08:00
draft: false
author: "EdenZ"
authorLink: ""
description: "记录常用工具快捷键"
license: ""

tags: ["工具"]
categories: ["tool"]
hiddenFromHomePage: false

featuredImage: "/images/img/yellow.jpg"
featuredImagePreview: ""

toc: true
autoCollapseToc: true
math: false
lightgallery: true
linkToMarkdown: true
share:
  enable: true
comment: true
---

<!--more-->

快捷键是个好东西可惜脑子不好使老是容易忘。。。。以下快捷键是在Macs上的。
- ⌘（command）
- ⌥（option）
- ⇧（shift）
- ⇪（caps lock）
- ⌃（control）
- ↩（return）
- ⌅（enter）

## VSCode
- [中文快捷键表](https://www.52cik.com/vscode-keyboard-shortcuts/)

## Vim

### 移动
| 组合键                     | 描述 |
| -------------------------- | ---- |
| h,j,k,l | 左，下，上，右。  |
| w | 下一个词的词首。        |
| e | 下一个词的词尾。        |
| b | 上一个词的词首。        |
| <> | v 模式选中后进行缩进。 |

### 跳转
| 组合键                                          | 描述 |
| ----------------------------------------------- | ---- |
| % | 可以匹配{},"",(),[]之间跳转。                |
| H、M、L | 直接跳转到当前屏幕的顶部、中部、底部。 |
| #H | 跳转到当前屏的第 # 行。                     |
| #L | 跳转到当前屏的倒数第 # 行。                 |
| zt | 当前编辑行置为屏顶。                        |
| zz | 当前编辑行置为屏中。                        |
| zb | 当前编辑行置为屏底。                        |
| G | 直接跳转到文件的底部。                       |
| gg | 跳转到文件首。                              |
| () | 跳转到当前的行首、行尾。                    |
| {} | 向上、向下跳转到最近的空行。                |
| [{ | 跳转到目前区块开头。                        |
| ]} | 跳转到目前区块结尾。                        |
| 0 | 跳转到行首。                                 |
| $ | 跳转到行尾。                                 |
| 2$ | 跳转到下一行的行尾。                        |
| # | 跳转到该行的第 # 个位置。                    |
| #G | 15G,跳转到15行。                            |
| :# | 跳转到 # 行。                               |
| f'n' | 跳转到下一个"n"字母后。                   |
| ⌃+b | 向后翻一页。                            |
| ⌃+f | 向前翻一页。                            |
| ⌃+u | 向后翻半页。                            |
| ⌃+d | 向前翻半页。                            |
| ctry+e | 下滚一行。                              |

### 选择
| 组合键 | 描述         |
| ------ | ------------ |
| V      | 选择一行     |
| ^V     | 矩形选择     |
| v3w    | 选择三个字符 |

### 编辑
| 组合键                                                            | 描述 |
| ----------------------------------------------------------------- | ---- |
| i | 光标前插入。                                                   |
| I | 在当前行首插入。                                               |
| a | 光标后插入。                                                   |
| A | 当前行尾插入。                                                 |
| O | 在当前行之前插入新行。                                         |
| o | 在当前行之后插入新行。                                         |
| r | 替换光标所在处的字符。                                         |
| R | 替换光标所到之处的字符。                                       |
| cw | 更改光标所在处的字到字尾处。                                  |
| c#w | c3w 修改3个字符。                                            |
| C | 修改到行尾。                                                   |
| ci' | 修改配对标点符号中的文本内容。                               |
| di' | 删除配对标点符号中的文本内容。                               |
| yi' | 复制配对标点符号中的文本内容。                               |
| vi' | 选中配对标点符号中的文本内容。                               |
| s | 替换当前一个光标所处字符。                                     |
| #S | 删除 # 行，并以新文本代替。                                   |
| D | 删除到行尾。                                                   |
| X | 每按一次，删除光标所在位置的前面一个字符。                     |
| x | 每按一次，删除光标所在位置的后面一个字符。                     |
| #x | 删除光标所在位置后面 6 个字符。                               |
| d^ | 删至行首。                                                    |
| d$ | 删至行尾。                                                    |
| dd | (剪切)删除光标所                                              |
| dw | 删除一个单词/光标之后的单词剩余部分。                         |
| d4w | 删除4个word。                                                |
| #dd | 从光标所在行开始删除 # 行。                                  |
| daB | 删除 {} 及其内的内容。                                       |
| diB | 删除 {} 中的内容。                                           |
| n1,n2 d | 将 n1, n2 行之间的内容删除。                             |
| / | 输入关键字，发现不是要找的，直接在按 n，向后查找直到找到为止。 |
| ? | 输入关键字，发现不是要找的，直接在按 n，向前查找直到找到为止。 |
| * | 在当前页向后查找同一字。                                       |
| # | 在当前页向前查找同一字。                                       |
| yw | 将光标所在之处到字尾的字符复制到缓冲区中。                    |
| #yw | 复制 # 个字到缓冲区。                                        |
| Y | 相当于yy, 复制整行。                                           |
| #yy | 表示复制从光标所在的该行往下数 # 行文字。                    |
| p | 粘贴。所有与y相关的操作必用p来结合粘贴。                       |
| n1,n2 co n3 | 复制第 n1 行到第 n2 行之间的内容到第 n3 行后面。     |
| gUU | 将当前行的字母改为大写。                                     |
| guu | 将当前行的字母改为小写。                                     |
| gUw | 将当前光标下的单词改为大写。                                 |
| guw | 将当前光标下的单词改为小写。                                 |
| gg | 光标到文件第一个字符。                                        |
| gu | 把选择范围全部小写。                                          |
| G | 到文件结束。                                                   |
| ggguG | 整篇小写。                                                 |
| gggUG | 整篇大写。                                                 |
| J | 当前行和下一行合并成一行。                                     |
| n1,n2 m n3 | 将 n1 行到 n2 行之间的内容移至 n3 行下。              |

### 退出
| 组合键                                         | 描述 |
| ---------------------------------------------- | ---- |
| w filename | 保存正在编辑的文件 filename        |
| wq filename | 保存后退出正在编辑的文件 filename |
| q | 退出不保存。                                |

### 窗口操作
| 组合键                                 | 描述 |
| -------------------------------------- | ---- |
| ⌃+w p | 在两个分割窗口之间来回切换。 |
| ⌃+w j | 跳到下面的分割窗             |
| ⌃+w h | 跳到左边的分割窗。           |
| ⌃+w k | 跳到上面的分割窗。           |
| ⌃+w l | 跳到右边的分割窗。           |

## Mac
| 组合键               | 描述                                                                     |
| -------------------- | ------------------------------------------------------------------------ |
| ⌘+A            | Selects all items                                                        |
| ⌘+C            | Copies selected items                                                    |
| ⌘+D            | Duplicates the selected item(s)                                          |
| ⌘+E            | Ejects the selected volume                                               |
| ⌘+F            | Displays the Find dialog                                                 |
| ⌘+H            | Hides All Finder windows                                                 |
| ⌘+I            | Shows info for selected item or items                                    |
| ⌘+J            | Shows the view options for the active window                             |
| ⌘+K            | Displays the Connect to Server dialog                                    |
| ⌘+L            | Creates an alias for the selected item                                   |
| ⌘+M            | Minimizes the active window                                              |
| ⌘+N            | Opens a new Finder window                                                |
| ⌘+O            | Opens (or launches) the selected item                                    |
| ⌘+R            | Shows the original for selected alias                                    |
| ⌘+T            | Adds the selected item to the Sidebar                                    |
| ⌘+V            | Pastes items from the Clipboard                                          |
| ⌘+W            | Closes the active window                                                 |
| ⌘+X            | Cuts the selected items                                                  |
| ⌘+Z            | Undoes the last action (if possible)                                     |
| ⌘+,            | Displays Finder Preferences                                              |
| ⌘+1            | Shows the active window in icon mode                                     |
| ⌘+2            | Shows the active window in list mode                                     |
| ⌘+3            | Shows the active window in column mode                                   |
| ⌘+4            | Shows the active window in cover flow mode                               |
| ⌘+[            | Moves back to the previous Finder location                               |
| ⌘+]            | Moves forward to the next Finder location                                |
| ⌘+Del          | Moves selected items to the Trash                                        |
| ⌘+up-arrow     | Show enclosing folder                                                    |
| ⌘+ tab         | Cycles through windows                                                   |
| ⌘+?            | Displays the Mac OS X Help Viewer                                        |
| ⌘+Shift+A      | Takes you to your Applications folder                                    |
| ⌘+Shift+C      | Takes you to the top-level Computer location                             |
| ⌘+Shift+G      | Takes you to a folder that you specify                                   |
| ⌘+Shift+H      | Takes you to your Home folder                                            |
| ⌘+Shift+I      | Connects you to your iDisk                                               |
| ⌘+Shift+Q      | Logs you out                                                             |
| ⌘+Shift+N      | Creates a new untitled folder in the active window                       |
| ⌘+Shift+U      | Takes you to your Utilities folder                                       |
| ⌘+Shift+Del    | Deletes the contents of the Trash                                        |
| ⌘+Option+H     | Hides all windows except the Finder’s window(s)                          |
| ⌘+Option+N     | Creates a new Smart Folder                                               |
| ⌘+Option+T     | Hides the Finder window toolbar                                          |
| ⌘+Option+Space | Opens the Spotlight window                                               |
| ⌘+Space        | Opens the Spotlight menu                                                 |
| F8	Choose another    | desktop using Spaces                                                     |
| Control+up-arrow     | Displays the Mission Control screen                                      |
| Control+down-arrow   | Shows all open windows for the current application using Mission Control |
| F11                  | Hides all windows to display the Desktop using Mission Control           |
| F12                  | Displays your Dashboard widgets                                          |
| Space                | Quick Look                                                               |

## Bash/Zsh

### 窗口快捷键
| 组合键  | 描述       |
| ------- | ---------- |
| ⌘+n     | 新建窗口   |
| ⌘+⌥+num | 跳转窗口   |
| ⌘+t     | 新建tab    |
| ⌘+num   | 跳转tab    |
| ⌘+d     | 左右分屏   |
| ⌘+⇧+d   | 上下分屏   |
| ⌘+w     | 关闭当前页 |
| ⌘+⌅     | 最大最小化 |

### 编辑快捷键
| 组合键 | 描述                     |
| ------ | ------------------------ |
| ⌃ + u  | 清空当前行               |
| ⌃ + a  | 移动到行首               |
| ⌃ + e  | 移动到行尾               |
| ⌃ + f  | 向前移动                 |
| ⌃ + b  | 向后移动                 |
| ⌃ + p  | 上一条命令               |
| ⌃ + n  | 下一条命令               |
| ⌃ + r  | 搜索历史命令             |
| ⌃ + y  | 召回最近用命令删除的文字 |
| ⌃ + h  | 删除光标之前的字符       |
| ⌃ + d  | 删除光标所指的字符       |
| ⌃ + w  | 删除光标之前的单词       |
| ⌃ + k  | 删除从光标到行尾的内容   |
| ⌃ + t  | 交换光标和之前的字符     |

## 引用
- [zsh 快捷键让shell 命令效率飞起](https://juejin.im/post/5ce63ef1f265da1ba431c622)
- [Vim常用快捷键分类备忘单](https://quickapp.lovejade.cn/vim-common-shortcuts/)
