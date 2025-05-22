---
time: 2025-02-13
tags:
  - git
---

### 使用`git revert`撤销合并（保留历史记录）

`git revert`命令可以撤销一个合并操作，并生成一个新的提交记录来撤销这次合并。具体步骤如下：

1. **找到合并提交的哈希值**：使用`git log`命令查看提交历史，找到合并分支A的提交哈希值。
    
    bash复制
    
    ```bash
    git log
    ```
    
    假设合并提交的哈希值为`abcd1234`。
    
2. **撤销合并操作**：使用`git revert -m 1 <commit-hash>`命令来撤销合并操作。`-m 1`表示选择主分支的修改内容进行撤销。
    
    bash复制
    
    ```bash
    git revert -m 1 abcd1234
    ```
    
3. **提交撤销操作**：`git revert`会自动创建一个新的提交，撤销合并操作引入的更改。
    
4. **推送撤销提交到远程仓库**（如果需要）：将撤销提交推送到远程仓库，确保其他开发者也能看到撤销操作。
    
    bash复制
    
    ```bash
    git push origin <your-branch>
    ```
