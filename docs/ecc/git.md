# Git 相关

!!! question "本文是否适合你"
    请尝试回答以下三个问题

    - Commit 的时候提示 `*** Please tell me who you are.` 应该怎么办？
    - pull 和 push 都是啥？
    - .gitignore 是干啥的？

    如果你无法回答，请先阅读 <https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/> 以及 <https://github.com/git/git/blob/master/Documentation/giteveryday.txt>。这将会使你入门 Git 的基本使用。

!!! info "如果在 TOC 找不到完全对应你问题的解答"

    - 关于 Git 的使用技巧，请查看 <https://github.com/git-tips/tips>。链接内有中文版链接。
    - 关于 Git 的行为细节，请查看 <https://git-scm.com/book/en/v2>。链接内有 PDF / EPUB 下载。有中文版，但是推荐阅读英文版。
    - 关于 Git 的更多细节，请查看 <https://github.com/b1f6c1c4/learn-git-the-super-hard-way>。只有中文版。
    - 关于 Git 的具体实现，请查看 <https://github.com/git/git/tree/master/Documentation>
    - Git 源码：<https://github.com/git/git>

## 修改一个提交的内容

这可能包括修改的文件、Commit 信息、签名等等。

### 这是最近的一个提交，不保留

commit 有一个 --amend 选项，相当于撤销 HEAD 对应的 commit，然后重新 commit。

```bash
> git add files/to/change
> git commit --amend
# You can change commit message too!
```

### 这不是最近的一个提交，不保留这一提交

git 拥有一个子命令 `rebase` 用于修改较深的历史。常用于从历史中删除不小心添加进去的秘钥、修复本地分支和 Upstream 的分叉（将本地分支“移植”到 Upstream 指向的最新 Commit 上，因此得名 `rebase`），以及，删除某一个特定的历史中的提交。

See: [`git rebase`](https://git-scm.com/docs/git-rebase)

推荐的使用方法是在 vim / Emacs 作为默认编辑器 (`core.editor`) 时，使用 `git rebase -i [THE COMMIT HASH]`。

### 您想保留之前的提交

See: [`git revert`](https://git-scm.com/docs/git-revert)

这会产生一个 "Reverted: xxx" 的新提交，内容是给定提交的逆。

## 在 push 或者 commit 之前进行特定检查，类似 CI

See: [`hooks`](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)

最常用的是 `pre-commit` 和 `pre-push`。非零返回值会打断 `commit` 或者 `push`。

## 将本地 A 分支推至 remote 的 B 分支

### 错误地在本地的 A 分支上进行了提交

你希望在分支 `jiegetql` 进行数次 commit，但是忽然发现你其实一直在 master 上操作。

```bash
# 首先，将本地的 jiegetql 分支同步到和本地 master 的相同位置
[  master] > git checkout jiegetql
[jiegetql] > git reset --hard master
# Alternately: git merge --ff-only master

# 接下来，将 master 强制安排到和云端的 master 同样地地方
[jiegetql] > git checkout master
[  master] > git fetch --all
[  master] > git reset --hard origin/master
```

### A 上的提交是正确的，只是想推到另一个名字的分支上

你想直接用本地的 `dirty` 分支推到 remote `meow` 上的 `kawaii` 分支。

```bash
[dirty] > git push meow dirty:kawaii
```

See: [Refspec](https://git-scm.com/book/en/v2/Git-Internals-The-Refspec)

## 需要删除云端的分支 / Tag

### 分支

你想删除 remote `meow` 上面的 `kawaii` 分支。

```bash
> git push meow :kawaii
# Alternately: git push --delete meow kawaii
```

如果你很感兴趣 `:kawaii` 是什么意思，请**仔细**阅读 Git Book 的 [Refspec](https://git-scm.com/book/en/v2/Git-Internals-The-Refspec) 一节

### Tag

你想删除 remote `meow` 上面的 `kawaii` Tag。

```bash
> git push meow :refs/tags/kawaii
```

## 错误的分支删除 / reset，需要找回之前的 commit hash

只要没有执行过 `git gc`，git 不会主动清理对象，即使没有任何的指针指向它。`git reflog` 会打印出最近 git 操作所改变的各种 Hash，可以据此加上 `git reset --hard` 或者 `git checkout + git branch` 恢复之前的 commit。

See: [`git reflog`](https://git-scm.com/docs/git-reflog)
