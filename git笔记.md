
##fork别人的项目后同步原分支

	1.查看所有远程库(remote repo)的远程url
	git remote -v;
	
	2.添加源分支url
	git remote add upstream 这里替换为源项目url;
	
	3.查看所有远程库(remote repo)的远程url
	git remote -v;
	
	4.从源分支获取最新的代码
	git fetch upstream;
	
	5.切换到主分支
	git checkout master;
	
	6.合并本地分支和源分支
	git merge upstream/master;

	7.push到fork分支
	git push;