# github使用记录
## ubuntu下的安装过程
### 参考文章
- ubuntu下的配置
```
https://blog.csdn.net/isunnyvinson/article/details/52598863
```
- ubuntu下的使用（库的导入和导出）
```
https://blog.csdn.net/feather_wch/article/details/50620311
```
命令为：
git clone git@github.com:FeatherHunter/IHome.git
2. 获取到源码之后，就可以进行开发了，代码开发完成就可以提交代码：
```
git add .    //往暂存区域添加已添加和修改的文件，不处理删除的文件
git status   //比较本地数据目录与暂存区域的变化
git commit -m "commit directions" //提到代码到本地数据目录，并添加提交说明
```
3. 有可能你和其他人改的是同一个文件，那么冲突的情况是在所难免的，那么在提交之后再获取一下代码，就会提示代码冲突的文件，我们需要做的就是处理这些冲突，并再次提交：
```
git pull     //更新代码
//根据提示修改冲突文件中的代码
git add .
git commit -m "commit directions"
```
4. 当你做完以上的步骤的时候，你需要做的是把本地数据目录的版本库的数据同步到GitHub服务器上去，这样你的同事才能够看到你作出的修改：


- 执行顺序

```
sxy@sxy-pc:~/git$ git config --global user.name "shengxiaoyi1993"
sxy@sxy-pc:~/git$ git config --global user.email "shengxiaoyi2014@icloud.com"
sxy@sxy-pc:~/git$ ls
repos
sxy@sxy-pc:~/git/repos/project$ git init
Reinitialized existing Git repository in /home/sxy/git/repos/project/.git/
sxy@sxy-pc:~/git/repos/project$ touch Readme
sxy@sxy-pc:~/git/repos/project$ ls
Readme  README.md
sxy@sxy-pc:~/git/repos/project$ git add Readme
sxy@sxy-pc:~/git/repos/project$ ls
Readme  README.md
sxy@sxy-pc:~/git/repos/project$ git commit -m 'add readme file'
[master 656da9b] add readme file
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 Readme
sxy@sxy-pc:~/git/repos/project$ git remote add origin https://github.com/shengxiaoyi1993/project.git  
fatal: remote origin already exists.
sxy@sxy-pc:~/git/repos/project$ git push origin master

Counting objects: 4, done.
Delta compression using up to 6 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 307 bytes | 307.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To github.com:shengxiaoyi1993/project.git
   603370e..656da9b  master -> master
sxy@sxy-pc:~/git/repos/project$
sxy@sxy-pc:~/git/repos/project$

```
- 上传本地项目

```
//设置key及上传本地项目
https://blog.csdn.net/pnan222/article/details/79740787
```

```
sxy@sxy-pc:~/git/repos$ cd opencv_test/
sxy@sxy-pc:~/git/repos/opencv_test$ git init
sxy@sxy-pc:~/git/repos/opencv_test$ git clone git@github.com:shengxiaoyi1993/opencv_test.git
sxy@sxy-pc:~/git/repos/opencv_test$ cd opencv_test/
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git init
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ touch Readme
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git add Readme
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git commit -m 'add readme file'
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git remote add origin https://github.com/shengxiaoyi1993/opencv_test.git
fatal: remote origin already exists.
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git remote rm origin
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git remote add origin https://github.com/shengxiaoyi1993/opencv_test.git
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git add -A
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git commit -m 'add all'
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git push origin master
//卡在writing objects
sxy@sxy-pc:~/git/repos/opencv_test/opencv_test$ git config --global http.postBuffer 524288000


```
- 删除项目中的某个文件
```
$ git rm -r --cached hkcamera_test/
$ git commit -m "删除hkcamera_test"
$ git push -u origin master

```
