# 折腾域名

一直以为一个域名只能绑定一个服务，无论有多少个子域名，结果发现，原来可以绑定多个GitHub Page。

原来的域名[shaojin.me](https://shaojin.me)是在[Namesilo](https://https://www.namesilo.com/)购买，使用默认DNS服务。

之前，开设了两个GitHub Page，一个用于Gridea，一个用Hugo搭建。因为就买了一个域名，就只绑定到了Gridea。为了自定义Hugo的域名，单独申请了一个免费域名。突然想起来，是否可以为[shaojin.me](https://shaojin.me)创建一个子域名用来绑定另一个Page服务，顺便将域名解析服务转移到Cloudflare。

注册完Cloudflare后，输入站点域名，Cloudflare会自动进行扫描，然后给出两个DNS 服务器名称，直接到[Namesilo](https://https://www.namesilo.com/)域名管理后台进行更改，然后等待生效就可以。

生效后，注意关闭Cloudflare的CDN服务，否则站点显示总存在问题，关闭后就正常了。

然后，添加CNAME`blog.shaojin.me`这个子域名到Gridea关联的仓库，添加CNAME`shaojin.me`到Hugo的仓库。

完美!


