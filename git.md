### git的三块区域

1、工作区 working directory 可以看成就是很普通的项目文件夹  在这个项目文件夹里对你的项目里面的文件进行任意的修改 新建 删除   

2、暂存区 暂存已经修改的文件 最后统一提交到git仓库中 tracked（文件没有add到暂存区就是untracked，add到暂存区了就是tracked，只能将tracked的文件同步到仓库 也就是说文件要同步到仓库的前提是文件必须是在暂存区中

3、git仓库 git repository 最终确定项目的一个版本 同步到仓库 （可以理解一个仓库就是一个项目） 并且对他人可见





##### gits status 查看文件的状态  它可以查看到仓库下的文件信息 哪些是

##### git add [文件]  将文件从工作区加入到暂存区

##### git rm [文件]  将仓库的文件删除后需要用git rm更新一下暂存区 然后再用git commit同步到仓库

##### git commit -m "description" 将文件从暂存区提交到git仓库 并增加描述

##### git push 将本地仓库的内容同步到远程仓库



##### git config --local 为local级别的git进行配置

##### git config --global 为global级别的git进行配置 

##### git config --system 为system级别的git进行配置



##### git init 将当前文件夹（一般是项目目录嘻嘻嘻嘻）创建为git仓库