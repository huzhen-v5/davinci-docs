# davinci可视化项目的用户操作文档工程

Davinci 是一个 DVaaS（Data Visualization as a Service）平台解决方案，面向业务人员/数据工程师/数据分析师/数据科学家，致力于提供一站式数据可视化解决方案。

Davinci源码地址：
[https://github.com/edp963/davinci](https://github.com/edp963/davinci)
<br>
Jekyll中文文档地址：
[http://jekyllcn.com/](http://jekyllcn.com/)
<br>
Minmal Mistakes源码地址：
[https://github.com/mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)

关于如何从davinci源码中的docs转换到本源码，可参考我的一篇博客：
[https://blog.csdn.net/huzhenv5/article/details/104196806](https://blog.csdn.net/huzhenv5/article/details/104196806)

## 安装Ruby开发环境
由于文档工程采用Jekyll + Minmal Mistakes，本机需要先安装Ruby开发环境，笔者尝试过安装Ruby2.7.0，结果在安装工程依赖过程中报错，提示Ruby版本需要大于等于2.3，小于2.7，所以笔者又安装了2.6.5的版本；因此笔者建议大家也安装2.6.5的Ruby版本，安装方法请看笔者的文章：[windows10上配置Ruby开发环境](https://blog.csdn.net/huzhenv5/article/details/104187433)

## 安装依赖
在根目录下运行命令：

```shell
bundle install
```

## 启动serve
在根目录下运行命令：

```shell
bundle exec jekyll serve
```

## 打包
在根目录下运行命令：

```shell
bundle exec jekyll build
```

## nginx部署
上步打包生成的`_site`文件夹里的内容就是静态页面的内容

nginx如何部署静态网页的方法网上很多，这里就不赘述了，但是需要注意两点：
* 在nginx部署的时候，根目录的名称不能随意命名，需要和`_config.yml`文件的`baseurl`字段保持一致，默认是`davinci`
* 在跳转页面的时候，跳转链接的url上不带.html的后缀，直接部署在nginx上跳转会提示404的报错，所以在nginx上要配置下，对访问链接进行重写，加上`.html`后缀：

```bash
location /davinci/ {
    root   html;
    index  index.html index.htm;
    if (!-e $request_filename){
	    rewrite ^(.*)$ /$1.html last;
	    break;
	}
}
```