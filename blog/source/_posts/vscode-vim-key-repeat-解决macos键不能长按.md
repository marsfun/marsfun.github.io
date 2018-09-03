---
title: vscode vim key repeat(解决macos键不能长按)
date: 2018-08-12 13:30:12
categories: 
- vscode
- macos 
tag: [vscode vim]
---

### MacOS Vscode 中vim 无法长按键的问题
***
命令行执行以下命令

```
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false         # For VS Code
$ defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false # For VS Code Insider
$ defaults delete -g ApplePressAndHoldEnabled                                      # If necessary, reset global default

```
重启 vscode 