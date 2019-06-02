### Cache Http Password
```shell
git config --global credential.helper wincred
```

### 场景1
>你之前在 newImage 分支上进行了一次提交，然后又基于它创建了 caption 分支，然后又提交了一次。
此时你想对的某个以前的提交记录进行一些小小的调整。比如设计师想修改一下 newImage 中图片的分辨率，尽管那个提交记录并不是最新的了

- 先用 git rebase -i 将提交重新排序，然后把我们想要修改的提交记录挪到最前
- 然后用 commit --amend 来进行一些小修改
- 接着再用 git rebase -i 来将他们调回原来的顺序
- 最后我们把 master 移到修改的最前端（用你自己喜欢的方法），就大功告成啦

--
- git checkout master
- git cherry-pick C2 #将C2提交复制到当前分支
- git commit --amend #调整修改
- git cherry-pick C3  

### 场景2
>建立标签

- git tag VERSION1 C1 #给C1提交打标签VERSION1
- 查找版本差异  
git describe 的​​语法是： git describe <ref>
<ref> 可以是任何能被 Git 识别成提交记录的引用，如果你没有指定的话，Git 会以你目前所检出的位置（HEAD）。它输出的结果是这样的：<tag>_<numCommits>_g<hash>tag 表示的是离 ref 最近的标签， numCommits 是表示这个 ref 与 tag 相差有多少个提交记录， hash 表示的是你所给定的 ref 所表示的提交记录哈希值的前几位。当 ref 提交记录上有某个标签时，则只输出标签名称  

### 场景3
git checkout master^
git checkout master~
git checkout master^2
git checkout master~2
之间的区别
