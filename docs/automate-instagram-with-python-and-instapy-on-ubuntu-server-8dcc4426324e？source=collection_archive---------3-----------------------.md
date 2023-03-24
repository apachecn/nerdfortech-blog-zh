# 在 Ubuntu 服务器上使用 Python 和 Instapy 自动化 Instagram

> 原文：<https://medium.com/nerd-for-tech/automate-instagram-with-python-and-instapy-on-ubuntu-server-8dcc4426324e?source=collection_archive---------3----------------------->

在本文中，我们将讨论如何使用 Python 和 Instapy 来自动化您的 Instagram。继之前关于用 python 自动化 twitter 的[教程之后，我们将对 Instagram 的可用库做类似的事情。](https://blog.tati.digital/2021/02/18/digital-marketing-with-twitter-and-python-for-django-web-app/)

![](img/78ef34b43c8ce8bc96cb2cdefbceb355.png)

我们将在本教程中介绍的内容:

*   设置开发环境——专门针对 ubuntu
*   安装 instapy
*   创建 instagram 会话
*   安排 python 脚本在系统文件上运行

# 设置开发环境—使用 python 和 Instapy 实现 Instagram 自动化

我们将在 Ubuntu 虚拟机上工作，您可以从这里获得:

[获取数字海洋水滴](https://m.do.co/c/7d9a2c75356d)，并使用此视频进行设置:[初始虚拟服务器设置](https://www.youtube.com/watch?v=Zyl23djnsVg)。

然后你可以按照以下步骤操作:注意，如果你得到一个装有数字海洋的虚拟机，你应该选择最新版本的 Ubuntu，它会预装 python 3.8 (Ubuntu 20.04)。

我们将遵循的步骤也可从官方的 [Instapy 文档网站](https://instapy.org/)获得。

从下面的命令运行它——安装 python3 支持库、pip3 等。

```
sudo apt-get update
sudo apt-get -y install unzip python3-pip python3-dev build-essential libssl-dev libffi-dev xvfb
```

安装虚拟环境

```
pip3 install virtualenv
```

安装 Chrome 和 Firefox——Instapy 在后台使用的工具，你需要这些工具才能让 insta py 工作。使用 wget 下载 chrome:

```
cd ~
wget "https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb"
```

安装 chrome

```
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

如果出现错误，只需运行以下命令:

```
apt-get -f install
```

删除下载的文件:

```
sudo rm google-chrome-stable_current_amd64.deb
```

安装 firefox 和驱动程序

```
sudo apt-get install firefox 
sudo apt install firefox-geckodriver
```

导航到您想要工作的文件夹，并创建虚拟环境

```
mkdir instagram && cd instagram
virtualenv instagramenv
source instagramenv/bin/activatepip install instapy
```

成功运行以上所有命令后，您将在虚拟环境中安装 instapy，您可以开始使用。

# 创建 Instapy 会话—运行一些代码

创建一个名为 config.py 的文件，您可以在其中输入您的 instagram 用户名和密码:

# `config.py`

`insta_username = 'instagram-username'
insta_password = 'instagram-password'`

在同一个目录中，创建以下文件— main.py

```
from instapy import InstaPy, smart_run
import configdef wordPressSession():
    comments = [
            'Learn more from https://skolo.online',
            'Learn Wordpress https://skolo.online',
            'Learn WooCommerce from https://skolo.online',
            'Yes! Amazing stuff',
            'Just love wordpress',
            'Check us out https://skolo.online',
            'Check out Youtube https://rb.gy/n7tfbn',
            'Free Learning Videos https://rb.gy/n7tfbn',
            'Online Learning https://rb.gy/n7tfbn',
            'WooCommerce Series https://rb.gy/hgc8um',
            'Learn Wordpress https://rb.gy/hgc8um',
            'Learn WooCommerce https://rb.gy/hgc8um'
            ] session = InstaPy(username=config.insta_username,
                      password=config.insta_password,
                      headless_browser=True) try:
        with smart_run(session):
            session.like_by_tags(["wordpress", 'woocommerce'], amount=10)
            session.set_do_comment(True, percentage=50)
            session.set_comments(comments)
    except:
        import traceback
        print(traceback.format_exc())
        pass wordPressSession()
```

上面的代码会让你登录到你的 instagram 个人资料，搜索你提供的标签:“wordpress”和“woocommerce”。然后，它会喜欢带有这些标签的许多状态，甚至会用您在列表中提供的评论对 50%的状态进行评论。

通过在命令行中输入以下命令来运行该文件:

```
python main.py
```

# 自动化整个过程

我们在虚拟服务器上运行这些代码的原因是，我们希望能够按计划连续运行这些代码。Python 为此提供了一个[调度库](https://schedule.readthedocs.io/en/stable/):

仍然在虚拟环境中:

```
pip install schedule
```

然后编辑您的代码，如下所示:

```
from instapy import InstaPy, smart_run
import config
import schedule
import time
import randomdef wordPressSession():
    comments = [
            'Learn more from https://skolo.online',
            'Learn Wordpress https://skolo.online',
            'Learn WooCommerce from https://skolo.online',
            'Yes! Amazing stuff',
            'Just love wordpress',
            'Check us out https://skolo.online',
            'Check out Youtube https://rb.gy/n7tfbn',
            'Free Learning Videos https://rb.gy/n7tfbn',
            'Online Learning https://rb.gy/n7tfbn',
            'WooCommerce Series https://rb.gy/hgc8um',
            'Learn Wordpress https://rb.gy/hgc8um',
            'Learn WooCommerce https://rb.gy/hgc8um'
            ] session = InstaPy(username=config.insta_username,
                      password=config.insta_password,
                      headless_browser=True) try:
        with smart_run(session):
            session.like_by_tags(["wordpress", 'woocommerce'], amount=10)
            session.set_do_comment(True, percentage=50)
            session.set_comments(comments)
    except:
        import traceback
        print(traceback.format_exc())
        passschedule.every().day.at("08:00").do(wordPressSession)while True:
    schedule.run_pending()
    time.sleep(1)
```

上面的代码将在每天 08:00 运行名为:wordPressSession()的函数。

创建名为 Instagram 的系统文件:

```
sudo nano /etc/systemd/system/instagram.service
```

在文件中，粘贴以下内容:

```
[Unit]
Description=Instagram Bot Skolo Online Service
After=multi-user.target[Service]
Type=simple
Environment="PATH=/home/username/instagram/instagramenv/bin"
ExecStart=/home/username/instagram/instagramenv/bin/python /home/username/instagram/main.py [Install]
WantedBy=multi-user.target
```

上面的代码需要根据您的使用情况进行修改:

**环境**:应该是你的 env 文件/bin 的绝对路径

**Exec Start** :应该是你的 python 在 env 里面的绝对路径和你正在运行的文件位置。有点像: ***python main.py*** —但是有绝对路径。

本次运行后:

```
sudo systemctl start instagram
sudo systemctl enable instagram
sudo systemctl status instagram
```

当检查系统文件的状态时，在最后一个命令之后，您应该会看到一个漂亮的绿灯——如果一切正常的话。如果没有，请检查错误消息并进行相应的修复—错误的通常原因是系统文件中的绝对路径错误。

# 完整视频教程—使用 python 和 instapy 实现 instagram 自动化:

下面是完整的 youtube 视频教程。

使用 Python 和 Instapy 自动化 Instagram