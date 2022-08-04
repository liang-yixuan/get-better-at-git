# 1 - Git Object Model

### 🔨 Init

Right after we do `git init`, we will get the following from the `.git` folder (by typing `tree .git` in the terminal).

```
> tree .git
.git
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```

### 🔨 Git add

After we make some change and stage it in our `staging area`, we will see `an object` got create in our `.git` folder

```
> echo "hello world" >> README.md
> git add .
> tree .git
.git
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── objects
│   ├── 3b
│   │   └── 18e512dba79e4c8300dd08aeb37f8e728b8dad
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```

We can inspect:

1. The type of this object by using `git cat-file -t 3b18`: blob
2. The content of this object by using `git cat-file -p 3b18`: hello world
3. The size of this object by using `git cat-file -s 3b18`: 12  
   (hint: you can always do `git cat-file -h` to get help on what you can do with this command)

### 🔨 Git Commit

```
> gc -m "Initial commit"
[master (root-commit) b97cc01] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
> tree .git
.git
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── 3b
│   │   └── 18e512dba79e4c8300dd08aeb37f8e728b8dad
│   ├── 43
│   │   └── b71c903ff52b9885bd36f3866324ef60e27b9b
│   ├── b9
│   │   └── 7cc01a5408099753cee02cc9e3d12c5c05be32
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags
```

We can see the line `[master (root-commit) b97cc01] Initial commit` after we commited our change, things to point out:

1. It's a root commit, so it has no parent commit
2. The hash for this commit is b97cc01, which we can also find in our `.git/objects` folder
3. Our hash of our blob object remains the same

Let's do some inspection again

```
> git cat-file -t 3b18
blob

> git cat-file -p 3b18
hello world

> git cat-file -s 3b18
12
```

```
> git cat-file -t 43b7
tree

> git cat-file -p 43b7
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad	README.md

> git cat-file -s 43b7
37

> git ls-tree 43b7
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad	README.md
```

```
> git cat-file -t b97c
commit

> git cat-file -p b97c
tree 43b71c903ff52b9885bd36f3866324ef60e27b9b
author Yixuan Liang <yixuan.liang@rea-group.com> 1659614076 +1000
committer Yixuan Liang <yixuan.liang@rea-group.com> 1659614076 +1000

Initial commit

> git cat-file -s b97c
197
```

So you might have a different commit hash since the commit is done by different author and timestamp, but our `blob` hash and `tree` hash should be the same. (Order of these object does NOT indicate what types they are)

## Semi-Summary

A `commit` is pointed to a `tree`  
A `tree` is pointed to different `trees` and `blobs`  
A `blob` generally stores the contents of a file.

![](objects-example.png)

## But what is the difference between `blob`, `tree` and `commit`?

### 🔨 Add another file

```
> echo "This is another file." >> another.txt
> ga .
> tree .git
.git
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── 3b
│   │   └── 18e512dba79e4c8300dd08aeb37f8e728b8dad
│   ├── 43
│   │   └── b71c903ff52b9885bd36f3866324ef60e27b9b
│   ├── 9c
│   │   └── a9a8716bf7f5ab1ff449be0c97a04a7aae4262
│   ├── b9
│   │   └── 7cc01a5408099753cee02cc9e3d12c5c05be32
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags
```

Now I got a new object `9ca9`

```
> git cat-file -t 9ca9
blob
> git cat-file -p 9ca9
This is another file.
```

1️⃣ If I add the same file again, but this time in sub-folder, NOTHING CHANGES!
(note that if you echo other stuff in your `src/another.txt`, you will get another `blob` 2️⃣)

```
> mkdir src
> echo "This is another file." >> src/another.txt
> ga .
> tree .git
```

> Because blobs are just files in your repository and git won't repeat them if they are identical.

They have no information about where these file are.  
 **That's where `tree` comes in.**

### 🔨 Commit another file

Note that I have 2 repositories to compare:

1. "This is another file." in src/another.txt --> 2 blobs, 1 tree, 1 commit
2. "This is a file." in src/another.txt --> 3 blobs, 1 tree, 1 commit

In repository 1️⃣

```
> gc -m "add files"
[master 23b8c75] add files
 2 files changed, 2 insertions(+)
 create mode 100644 another.txt
 create mode 100644 src/another.txt
> tree .git
.git
├── COMMIT_EDITMSG
├── HEAD
├── ORIG_HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── 23
│   │   └── b8c7598f13272ccd05b2d546cf0605cbd10bae
│   ├── 3b
│   │   └── 18e512dba79e4c8300dd08aeb37f8e728b8dad
│   ├── 43
│   │   └── b71c903ff52b9885bd36f3866324ef60e27b9b
│   ├── 9c
│   │   └── a9a8716bf7f5ab1ff449be0c97a04a7aae4262
│   ├── b9
│   │   └── 7cc01a5408099753cee02cc9e3d12c5c05be32
│   ├── e0
│   │   └── 4292d0122f5c100183391aaf07a0860c516043
│   ├── eb
│   │   └── 029c83f4172e63cb15f207f8652b73dfb3b6ff
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags

18 directories, 30 files
```

we get 3 new objects after commiting: 1 `commit object` and 2 `trees`.

```
> git cat-file -t e042
tree
> git cat-file -p e042
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad	README.md
100644 blob 9ca9a8716bf7f5ab1ff449be0c97a04a7aae4262	another.txt
040000 tree eb029c83f4172e63cb15f207f8652b73dfb3b6ff	src
```

```
> git cat-file -t eb02
tree
> git cat-file -p eb02
100644 blob 9ca9a8716bf7f5ab1ff449be0c97a04a7aae4262	another.txt
```

> Trees are folders in a commit.

Q1: What about our old tree?
A1: It remains unchange and only contains 1 blob, which is the state of our first commit.

```
> git cat-file -p 43b7
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad	README.md
```

Q2: Different commits create different trees? even if they have the same stuff?
A2: YES! Since the there will always be files chnage in every commit. For folders that involve the change (i.e., src): the hash of the tree will get updated. For folders that are related to the changed folder (i.e., ./): it will point to different trees, so it's hash will also get updated.

```
> echo "Will this add a new tree that points to the same thing as e042?" >> src/question.txt
> ga .
> gc -m "ask question about tree"
[master 5aae3fa] ask question about tree
 1 file changed, 1 insertion(+)
 create mode 100644 src/question.txt
```

```
> git cat-file -p 9247
Will this add a new tree that points to the same thing as e042?
> git cat-file -p f1f8
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad	README.md
100644 blob 9ca9a8716bf7f5ab1ff449be0c97a04a7aae4262	another.txt
040000 tree 5f85b1a83afa2121895a913ce04682731ceec885	src
> git cat-file -p 5f85
100644 blob 9ca9a8716bf7f5ab1ff449be0c97a04a7aae4262	another.txt
100644 blob 9247b82e946b9ace17e8aaa7e7b27b8b823f2242	question.txt
```

### Commit

- points to a tree (the newest and highest level folder)
- contains "parent" if it's not initial commit
- has other infomation about the commit (author, committer, date ...)

```
> git cat-file -t 23b8
commit
tree e04292d0122f5c100183391aaf07a0860c516043
parent b97cc01a5408099753cee02cc9e3d12c5c05be32
author Yixuan Liang <yixuan.liang@rea-group.com> 1659617208 +1000
committer Yixuan Liang <yixuan.liang@rea-group.com> 1659617208 +1000

add files
```

```
> git cat-file -p 5aae
tree f1f80736a9cb1b9a52e834e24b9f46f841f91c6c
parent 23b8c7598f13272ccd05b2d546cf0605cbd10bae
author Yixuan Liang <yixuan.liang@rea-group.com> 1659618530 +1000
committer Yixuan Liang <yixuan.liang@rea-group.com> 1659618530 +1000

ask question about tree
```

# Reference

http://shafiul.github.io/gitbook/1_the_git_object_model.html  
https://github.com/nnja/advanced-git/blob/master/presentation/slides.pdf
