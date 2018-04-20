#<center>通过git上传文件到GitHub</center>#
- 1打开git
- 2将GitHub与git绑定

```
git config --global user.name "github用户名"
git config --global user.email "github注册的邮箱"
```
- 3设置sshkey
```
sshkey的作用，我们可以使用rsa来生成公钥和秘钥，并且将公钥配置到GitHub上边，这样子从GitHub发回的加密的数据就可以在本地进行解密了
**生成秘钥的方式**
ssh-keygen -t rsa -C ""
生成之后打开，公钥配置到GitHub上边
```
- 4上传本地项目到GitHub
```
如果已经将GitHub上的仓库clone到本地的话，就不需要执行git remote add origin yourgit.git
之后执行git push -u origin master
```

<font color=red size=6>notice:</font>
> 在上传本地文件之前，需要将本地文件加到git仓库中，我们通过git add .(代表的是全部)加入文件，通过git commit -m "注释"进行提交。之后在上传
