---
layout: post
title:  "Welcome to Jekyll!"
date:   2018-12-20 17:02:04 +0800
categories: jekyll update
tags: [jekyll]
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

## 本地开发常用命令

本地开发 Jekyll 站点的常用命令：

### 基础命令
- `bundle exec jekyll serve` - 构建站点并启动本地服务器（通常在 http://localhost:4000）
- `bundle exec jekyll serve --watch` - 文件变更时自动重建（`jekyll serve` 默认行为）
- `bundle exec jekyll serve --drafts` - 包含草稿文章预览
- `bundle exec jekyll build` - 构建站点但不启动服务器（输出到 `_site` 目录）

### 调试命令
- `bundle exec jekyll serve --config _config.yml,_config_dev.yml` - 使用额外配置文件
- `bundle exec jekyll serve --incremental` - 只构建变更文件（构建快但不稳定）
- `jekyll doctor` - 检查站点配置问题
- `bundle exec jekyll build --trace` - 构建失败时输出详细错误

### 常用选项
- `--host 0.0.0.0` - 允许网络设备访问
- `--port 4001` - 指定端口（替换 4001 为所需端口）
- `--detach` - 后台运行

### 问题排查
- 依赖问题：先运行 `bundle install`
- 文件变更不更新：重启服务器（`Ctrl+C` 再 `bundle exec jekyll serve`）
- 查看生成文件：检查 `_site` 目录
- 配置问题：用 `jekyll doctor` 检查

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
