# How to Setup Your GitHub.io Blog

* 创建repo
repo的名字，必须保持格式\<username>.github.io，其中\<username>替换成你的github账户名

* 创建的repo clone到本地
    ```bash
    git clone xxxx
    cd xxx
    echo "Hello World!" > index.html
    git add index.html
    git commit -m "Init commit"
    git push origin main
    ```
* 打开网站：https://\<username\>.github.io，查看效果


* 选择合适的Jekyll主题
github.io默认采用Jekyll作为建站工具。
Jekyll的主题可以从[Jekyll Themes](https://jekyllthemes.io/)中选择。
将主题clone到本地，然后将主题的内容拷贝到你的 github\.io repo中即可。

* 编写发布博客
编写markdown文件，并放在_posts目录中，文件名需保持格式 YEAR-MONTH-DAY-title\.md。
提交到github\.io repo中，即可自动发布到网站上。