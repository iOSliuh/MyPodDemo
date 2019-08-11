一、创建私有索引库<br>
1.1 在托管平台创建一个项目（有私有权限则创建的是私有索引库，否则则如普通git）,如<br>

https://github.com/iOSliuh/liuhSpecs.git<br>
其中liuhSpecs为索引库名。<br>

1.2 添加刚才在托管平台创建的索引库到本地repo仓库中<br>

pod repo add liuhSpecs https://github.com/iOSliuh/liuhSpecs.git。<br>

二、创建Cocoapods私有库<br>
可通过<br>
pod lib create privateLibraryDemo<br>
命令依提示创建(会生成所需索引描述文件.podspec)，也可按照普通创建工程项目(如静态库、动态库)创建，另外单独再创建索引文件.podspec。<br>

三、提交Cocoapods私有库代码<br>
3.1、本地、关联远程及master提交<br>
     git remote add origin https://github.com/iOSliuh/privateLibraryDemo.git<br>
     git push -u origin master<br>
     
3.2、创建tag并编写描述文件<br>
    git tag -a 1.0.0 -m "version 1.0.0"<br>
    git push --tags<br>
    
3.3、关于tag操作<br>
     git  tag  查询所有tag<br>
     git  tag -d. 1.0.0  删除本地tag. 1.0.0<br>
     git  push origin. --delete 1.0.0  删除远程tag 1.0.0<br>
     
四、提交spec至索引库<br>
    先使用如下两行命令分别进行验证检查本地和远程，可忽略警告。<br>
    pod lib lint --allow-warnings<br>
    pod spec lint --allow-warnings<br>
    
  若验证失败或私有库中引用了第三方库 ，可在--allow-warnings前添加--verbose --use-libraries。<br>
    
  验证通过即可执行提交<br>
  pod repo push liuhSpecs privateLibraryDemo.podspec。<br>
    
  podspec描述文件的编辑至关重要，可参考官方指导<br>
  https://guides.cocoapods.org/syntax/podspec.html<br>
    
五、顺利完成上述过程后即可在项目工程中通过pod 'privateLibraryDemo'->·1.0.0·引用私有库。<br>

notes：<br>
1、私有库的Deployment Targets 版本号(如创建的私有库为静态库SDK)应该不高于引用工程的版本。<br>
2、如创建的私有库为静态库SDK，为避免私有库中引入的第三方库(如AFNetworking)在主工程中也有引入而引起冲突，可以在描述文件中添加spec.dependency = 'AFNetworking'。
  
  
