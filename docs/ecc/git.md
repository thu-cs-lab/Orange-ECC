# Git 相关

## It's your first time here.

如果你无法回答以下三个问题，请先阅读 <https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/> 以及 <https://github.com/git/git/blob/master/Documentation/giteveryday.txt>。

- Commit 的时候提示 `*** Please tell me who you are.` 应该怎么办？
- pull 和 push 都是啥？
- .gitignore 是干啥的？

否则，请阅读本页面的 TOC，如果找不到完全对应你需要的回答，那么：

- 关于 Git 的使用技巧，请查看 <https://github.com/git-tips/tips>。链接内有中文版链接。
- 关于 Git 的行为细节，请查看 <https://git-scm.com/book/en/v2>。链接内有 PDF / EPUB 下载。有中文版，但是推荐阅读英文版。
- 关于 Git 的具体实现，请查看 <https://github.com/git/git/tree/master/Documentation>
- Git 源码：<https://github.com/git/git>

## 你盯着一个提交，感觉他很有问题，但是

### 你刚刚提交它

commit 有一个 --amend 选项

### 你想留下修改历史

See: [`git revert`](https://git-scm.com/docs/git-revert)

### 它在好久之前

See: [`git rebase`](https://git-scm.com/docs/git-rebase)

推荐的使用方法是在 vim / Emacs 作为默认编辑器 (`core.editor`) 时，使用 `git rebase -i [THE COMMIT HASH]`。

## 本地 CI？

See: [`hooks`](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)

最常用的是 `pre-commit` 和 `pre-push`。非零返回值会打断 `commit` 或者 `push`。

## 你想把代码同步到一个云端分支，但是你发现...

### 你忘了

你现在在分支 `jiegetql`，进行了数次 commit，但是忽然发现你其实一直在 master 上操作。

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

### 你懒了

你想直接用本地的 `dirty` 分支推到 remote `meow` 上的 `kawaii` 分支。

```bash
[dirty] > git push meow dirty:kawaii
```

See: [Refspec](https://git-scm.com/book/en/v2/Git-Internals-The-Refspec)

## 你想删除云端...

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

说真的，你都会用 Tag 了，为什么还在这里看这个文档？

## ”我[REDUCTED] 我 reset 错东西了！“

See: [`git reflog`](https://git-scm.com/docs/git-reflog)
