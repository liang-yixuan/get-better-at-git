# 2 - References

```
> rm -rf .git/hooks
> echo "hello world" >> README.md
> git add .
> gc -m "init"
[master (root-commit) b0c3fd2] init
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
> tree .git
.git
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
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
│   ├── b0
│   │   └── c3fd2da3fea3959bc5f4443c2563c6f7dd6109
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags
```

```
> gco -b "new-branch"
Switched to a new branch 'new-branch'
> tree .git
.git
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           ├── master
│           └── new-branch
├── objects
│   ├── 3b
│   │   └── 18e512dba79e4c8300dd08aeb37f8e728b8dad
│   ├── 43
│   │   └── b71c903ff52b9885bd36f3866324ef60e27b9b
│   ├── b0
│   │   └── c3fd2da3fea3959bc5f4443c2563c6f7dd6109
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   ├── master
    │   └── new-branch
    └── tags
```

The HEAD is pointing to the `new-branch`

```
> cat .git/HEAD
ref: refs/heads/new-branch
```
