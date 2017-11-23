# github github常见操作和常见错误

Quick setup — if you’ve done this kind of thing before
 Set up in Desktop	or
HTTPS

	 https://github.com/littlediv/DownloadProgress.git

 SSH

	git@github.com:littlediv/DownloadProgress.git

We recommend every repository include a README, LICENSE, and .gitignore.
…or create a new repository on the command line

	echo "# DownloadProgress" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin git@github.com:littlediv/DownloadProgress.git
	git push -u origin master

…or push an existing repository from the command line

	git remote add origin git@github.com:littlediv/DownloadProgress.git
	git push -u origin master


…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

Import code

## 错误提示：fatal: remote origin already exists

如果输入$ git remote add origin git@github.com:djqiang（github帐号名）/gitdemo（项目名）.git 

提示出错信息：fatal: remote origin already exists.
    
解决办法如下：
    
1、先输入$ git remote rm origin
    
2、再输入$ git remote add origin git@github.com:djqiang/gitdemo.git 就不会报错了！
   
 3、如果输入$ git remote rm origin 还是报错的话，error: Could not remove config section 'remote.origin'. 我们需要修改gitconfig文件的内容
    
4、找到你的github的安装路径，我的是C:\Users\ASUS\AppData\Local\GitHub\PortableGit_ca477551eeb4aea0e4ae9fcd3358bd96720bb5c8\etc
    
5、找到一个名为gitconfig的文件，打开它把里面的[remote "origin"]那一行删掉就好了

## github提示Permission denied (publickey)，如何才能解决？

	$ git push -u origin master
	Permission denied (publickey).
	fatal: Could not read from remote repository.

1、创建SSH密钥

1）打开终端，输入命令 ssh-keygen -t rsa -C "66******33@163.com" 然后按回车键，双引号里的邮箱换成自己的；

2）按回车保存到默认位置，再稍等出来提示输入密码短语，输完按回车要输两遍；它用来加密私钥，也就是以后使用私钥的时候要输这个密码；

3）稍等出来提示成功，密钥存放在自己主文件夹的.ssh文件夹中

4）打开文件管理器，显示隐藏文件后，可以看到这个文件夹中有两个文件，一个私钥一个公钥，把这个文件夹备份一下.id_rsa 是密钥 ，id_rsa.pub是公钥。
打开公钥文件，把里面的内容全部选中以后复制一下，等会要用到；

5）接下来登录 github，在右上角自己的用户名旁边找到扳手图标设置账户，在设置页面右边找到 SSH  Keys，点击进入；
6）点击ADD SSH key

7）在 Title 里输一个名称，下面的 Key 里一会粘贴自己的公钥；

8）到刚才的.ssh文件夹中，双击打开自己的公钥文件 id_rsa.pub，复制里面的所有内容，然后粘贴到刚才的密钥导入框中，然后点下边的“Add Key”导入密钥；


## 访问受限

	$ ssh -T git@github.com
	Permission denied (publickey).

生成的 秘钥 文件位置不正确 /c/Users/mac/.ssh/id_rsa 

	$ ssh -T -v git@github.com
	OpenSSH_7.1p2, OpenSSL 1.0.2d 9 Jul 2015
	debug1: Reading configuration data /etc/ssh/ssh_config
	debug1: Connecting to github.com [192.30.255.112] port 22.
	debug1: Connection established.
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_rsa type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_rsa-cert type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_dsa type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_dsa-cert type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_ecdsa type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_ecdsa-cert type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_ed25519 type -1
	debug1: key_load_public: No such file or directory
	debug1: identity file /c/Users/mac/.ssh/id_ed25519-cert type -1
	debug1: Enabling compatibility mode for protocol 2.0
	debug1: Local version string SSH-2.0-OpenSSH_7.1
	debug1: Remote protocol version 2.0, remote software version libssh_0.7.0
	debug1: no match: libssh_0.7.0
	debug1: Authenticating to github.com:22 as 'git'
	debug1: SSH2_MSG_KEXINIT sent
	debug1: SSH2_MSG_KEXINIT received
	debug1: kex: server->client chacha20-poly1305@openssh.com <implicit> none
	debug1: kex: client->server chacha20-poly1305@openssh.com <implicit> none
	debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
	debug1: Server host key: ssh-rsa SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8
	debug1: Host 'github.com' is known and matches the RSA host key.
	debug1: Found key in /c/Users/mac/.ssh/known_hosts:2
	debug1: SSH2_MSG_NEWKEYS sent
	debug1: expecting SSH2_MSG_NEWKEYS
	debug1: SSH2_MSG_NEWKEYS received
	debug1: SSH2_MSG_SERVICE_REQUEST sent
	debug1: SSH2_MSG_SERVICE_ACCEPT received
	debug1: Authentications that can continue: publickey
	debug1: Next authentication method: publickey
	debug1: Trying private key: /c/Users/mac/.ssh/id_rsa
	debug1: Trying private key: /c/Users/mac/.ssh/id_dsa
	debug1: Trying private key: /c/Users/mac/.ssh/id_ecdsa
	debug1: Trying private key: /c/Users/mac/.ssh/id_ed25519
	debug1: No more authentication methods to try.
	Permission denied (publickey).



## 参考信息

[github常见操作和常见错误！错误提示：fatal: remote origin already exists.](http://blog.csdn.net/dengjianqiang2011/article/details/9260435)

[github提示Permission denied (publickey)，如何才能解决？](https://www.zhihu.com/question/21402411)

[创建SSH密钥](http://blog.csdn.net/makenothing/article/details/8450417)