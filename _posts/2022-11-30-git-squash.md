## git squash and git rebase
when we commits several times for one task, we have lots of commit histories in git log.
```
$ git log --pretty=oneline
89082762f5d993d66b92c12095e19f15209711a8 (HEAD -> master, origin/master, origin/HEAD) post: 20220118
0aa5aac34e98f7cbb0bc2569d6d92de5c3d42aef post: 20230117 posting
ab96bd470cce640651791d1da83d44c4d86c39be post: 20220110 edit spring annotation 2
9f8dd0bacfff642a8539b19d5cf4aeb625a4f06d Merge branch 'master' of https://github.com/SOLokill/SOLokill.github.io
46cceb80544f75fbc757154848ce692963988e70 post: 20220109 circular reference
209d77ce0d5429b4bbb4138c5b35a09cd0088660 edit: rename posts
aa42625ac6ff1c2dfe51505e7d99286f504c4623 post: 20220106 circularReference
4e7a6ace36e952f0af776a5483f533114d7b27ee post: 20220105 spring annotation 3
6cfec0d9363ecc3d205ded8bc423f8ff2a3fa69e post: 20230103 Spring annotation 2
```
maybe like this..
then how to make clean and pretty?

```
$ git rebase -i HEAD~9
```
1. use this code for make clean the commit logs.
2. then, first commit is pick.
3. the other commits is squash.
4. :wq
5. git push -f
6. if 5. fails, repeat "rebase --continue" for commits that you want to squash.

