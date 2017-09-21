# git 分支相关




### git rebase

`git rebase master topic`

`git rebase --onto test release^ dev` 把本来基于 release^1 的 dev 分支变基到 test 分支上，用于将一条分支上的开发线整个移动到完全不同的分支  
A -> B -> C -> D -> E  test  
A -> B -> W -> X -> Y -> Z  release (release^1 = Y)    
A -> B -> W -> X -> Y -> P -> Q  dev

变基之后 dev 分支变成
A -> B -> C -> D -> E -> P -> Q

`git rebase --preserve-merges master dev` 如果需要被变基的分支出现了环型结构，变基过程中可能会丢失掉某些合并点，是因为 git 在变基过程中会将该环形结构解析成一个线性序列，如果你不想 git 这么做，而是要保留合并点的话，可以使用 `--preserve-merges` 参数
