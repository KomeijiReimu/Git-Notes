# push大文件

[glf](https://git-lfs.com/)


当推送超过100MB的文件的时候github会报错，因此需要到上面的网站下载GLF并安装，安装之后即可在原来的git基础上使用。

在add大文件之前，需要先

```bash
git lfs install
```

激活大文件服务
```bash
# 添加特定格式文件到大文件管理中去
git lfs track "*.psd"
# 添加所有格式
git lfs track "*"
```

添加想要推送的大文件后会生成一个`.gitattributes`文件，这是大文件服务管理文件类型的配置文件，要确保这个文件也在git管理中(被git所跟踪)
```bash
git add .gitattributes
```

或者像往常一样添加所有文件进暂存区
```
git add .
```

之后不需要任何特殊操作，正常commit和push即可


> note:glfs能免费管理的文件大小有限，只有1GB，多了要收费

![[Pasted image 20240902214746.png]]

# clone包含大文件的项目

如果使用glfs管理项目，那么当你直接用github主页的下载zip来下载仓库，你会发现所有的文件都是坏的。

其实不是坏的，只不过默认下载的是*大文件的指针*，你需要手动把大文件本体拉下来

```bash
# 进入仓库目录后操作
git lfs pull
```


速度很快(*在解决了速度慢的问题后*)

# 取消GLF的托管，使之变成正常仓库

由于空间有限，所以有些时候我们可能会涉及到一些让由GLF托管的仓库变为正常仓库的操作。

> 这里再次澄清：
> - GLF托管：大文件服务管理，无法直接下载或clone使用，需要用GLF拉下来或者转换
> - 正常仓库：可以直接下载或clone，不支持100MB以上的文件

把GLF变为正常仓库的操作步骤：
*此步骤可以保证有效，但是是否有其他有效的方法还尚未验证*

- **clone仓库**
*此时文件都是占位符*
- **使用 `git lfs fetch`**： 这个命令会从远程仓库下载所有的 LFS 文件，但不会自动替换本地的占位符文件。
- **使用 `git lfs checkout`**： 这一步会将下载的 LFS 文件替换为实际的文件。
- **取消对 LFS 文件的跟踪**： 取消对特定文件格式的 LFS 跟踪：
```bash
git lfs untrack "*.psd"
git lfs untrack "*" # 所有文件
```
- **更新 `.gitattributes` 文件**： 确保 `.gitattributes` 文件不再包含 LFS 的相关条目。或者直接删除此文件
- **提交更改并推送到远程**