# How to Setup Your GitHub.io Blog

## 一、创建repo
repo的名字，必须保持格式\<username>.github.io，其中\<username>替换成你的github账户名

## 二、创建的repo clone到本地 输出hello world

```bash
git clone xxxx
cd xxx
echo "Hello World!" > index.html
git add index.html
git commit -m "Init commit"
git push origin main
```
- 打开网站：https://\<username\>.github.io，查看效果


## 三、选择合适的Jekyll主题
- github.io默认采用Jekyll作为建站工具。
- Jekyll的主题可以从[Jekyll Themes](https://jekyllthemes.io/)中选择。
- 将主题clone到本地，然后将主题的内容拷贝到你的 github\.io repo中即可。

## 四、编写发布博客
- 编写markdown文件，并放在_posts目录中，文件名需保持格式 YEAR-MONTH-DAY-title\.md。
- 提交到github\.io repo中，即可自动发布到网站上。

## 五、保证HTML图片路径正确解析
-  Jekyll 构建后图片路径的解析   ../md_imgs/image-1.png 这种相对路径在生成静态 HTML 时会根据 当前页面的最终 URL 路径 拼接图片路径，导致找不到图片。
- 所以markdown文档中不能使用相对路径，而是使用站点根路径的相对地址，类似：
    ```bash
    ![alt text](/md_imgs/image-1.png)
    ```
- 加上 / 表示从网站根目录开始查找。

    