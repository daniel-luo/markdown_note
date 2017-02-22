命令：git stash
1.使用git stash保存当前的工作现场，那么就可以切换到其他分支进行工作，或者在当前分支上完成其他紧急的工作，比如修订一个bug测试提交。
2.如果一个使用了一个git stash，切换到一个分支，且在该分支上的工作未完成也需要保存它的工作现场。再使用git stash。那么stash 队列中就有了两个工作现场。
3.可以使用git stash list。查看stash队列。
4.如果在一个分支上想要恢复某一个工作现场怎么办：先用git stash list查看stash队列。确定要恢复哪个工作现场到当前分支。然后用git stash pop stash@{num}。num 就是你要恢复的工作现场的编号。
5.如果想要清空stash队列则使用git stash clear。
6.同时注意使用git stash pop命令是恢复stash队列中的stash@{0}即最上层的那个工作现场。而且使用pop命令恢复的工作现场，其对应的stash 在队列中删除。使用git stash apply stash@{num}方法除了不在stash队列删除外其他和git stash pop 完全一样。