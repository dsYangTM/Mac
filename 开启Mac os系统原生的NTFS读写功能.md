## 开启Mac os系统原生的NTFS读写功能

早期的Mac OS是可以通过修改mount_ntfs指令实现的。但是10.5以后的版本都不可以编译了，打开是乱码。只能说微软霸道。后来只能用破解版的Paragon NTFS for MAC，但是更新Mac os系统之后老版的Paragon破解版就不能用了。

后来找到了Mounty这个软件，免费里的精品，用了好一段时间，而且更新系统还能用，但是在使用时遇到的问题有两个，一是每次挂载硬盘都要手动点重载读写，如果在勿扰模式办公，非常影响使用；二是不稳定，文件经常权限异常，需要借助Terminal更改文件权限，否则即便是在Windows中打开U盘，部分文件也没有权限，本来想省事，反而更麻烦了

还有一种办法就是利用虚拟机来实现NTFS格式的读写操作，不过这办法非常麻烦，只有在不是特别紧急的时候用用。

说了这么多，接下来就是支持Mac os系统原生的操作方法了，让MAC的挂载配得上系统的逼格。其实最早在OSX 10.5的时候，OSX原生就支持直接写入NTFS盘的，后来由于微软的限制，把这个功能给屏蔽了，我们可以通过命令行手动打开这个选项。

### 插上NTFS格式的硬盘

打开“实用工具”-“终端“里面进行输入命令。

#### 输入：diskutil list

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615122904234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RzX1lhbmc=,size_16,color_FFFFFF,t_70)

可以看到，硬盘对应的设备路径是/dev/disk2，然后我们的磁盘是有名称的，这里有的会显示，有的不会显示，但是本地磁盘默认会放在/Volumes中

#### 然后输入：ls /Volumes/

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615123148644.png)

看到我的移动硬盘ds.Yang,，然后更新 /etc/fstab文件，

#### 输入：sudo nano /etc/fstab

命令输入之后会让你输入电脑的密码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615123218798.png)
输入电脑的密码之后会看到下面这个文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615123252808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RzX1lhbmc=,size_16,color_FFFFFF,t_70)

#### 然后在文件里输入：LABEL=ds.Yang none ntfs rw,auto,nobrowse

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615123314113.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RzX1lhbmc=,size_16,color_FFFFFF,t_70)

下面来依次解释一下，如果你的名字里面有空格键，就需要用\040的意思是代替空格键，比如西数的硬盘名字很统一，也带空格，可以这样写：My\040Passport，后面的ntfs rw表示把这个分区挂载为可读写的ntfs格式，最后nobrowse非常重要，因为这个代表了在finder里不显示这个分区，这个选项非常重要，如果不写入的话挂载是不会成功的。(为了保证能一次成功最好把你硬盘的名字重命名为没有空格或者特殊符号的)

#### 最后：写完这里以后不是按回车，按 Ctrl + X，会出现要不要保存的字样，请按 Y 然后再按回车，这个时候可以重启了。

重启后桌面没有硬盘弹出来了，那么它在哪呢？
点击访达![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618143229173.png)，在访达的侧边栏里。

### 经过多次的实验，在电脑开启状态下，直接插入移动硬盘是不会弹出的，finder侧边栏也没有移动硬盘的图标。这个时候就有两种解决办法 ：一、插入硬盘重启电脑就能在finder侧边栏显示了。二、是在桌面创建快捷方式，下面是创建快捷方式的命令

#### 输入：sudo ln -s /Volumes/ds.Yang/ ~/Desktop/ds.Yang

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615123406589.png)

这样电脑桌面就会有这个硬盘的快捷方式了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618142219505.png)
有个小问题，不管有没有插入移动硬盘，这个快捷方式会一直在桌面上，
对于有强迫症的我决不允许桌面有任何图标。

#### 隐藏桌面移动硬盘快捷方式，拖入访达侧边栏

将桌面的快捷方式拖入访达侧边栏

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618143800675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RzX1lhbmc=,size_16,color_FFFFFF,t_70)

如果直接在桌面上将快捷方式删掉是会连带finder侧边栏也一起删掉的，所以只能用terminal将桌面快捷方式移动到finder侧边栏里

#### 输入：cd ~/Desktop/

#### mv ds.Yang .dsYang

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618145626851.png)

这个时候finder侧边栏就是改完名的状态，桌面的快捷方式就没有了。这里要说一句，mv 改的名字前面一定要加 “.” 不然桌面快捷方式不会消失 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618145727299.png)

大功告成。如果有多个移动硬盘也是同样的方法。只需要在这个文件里添加一行即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618150148838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RzX1lhbmc=,size_16,color_FFFFFF,t_70)

希望这篇文章能对你有帮助！
