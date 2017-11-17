# Smart-ExpressVpn

# 如何在使用ExpressVpn的情况下高速低延迟访问中国国内网站

ExpressVpn是我经朋友介绍所使用的一款VPN工具。虽然其价格相比于国内的大多数VPN都要高出不少，但其友好的GUI、快捷的使用前设置和高速安全的连接效果都令我眼前一亮。

但在使用过程中我也发现了一个问题，由于ExpressVPN使用的是其本身提供的DNS服务器，导致国内的很多常用域名像baidu.com qq.com等都不能被正确解析，从而导致无法访问。

# 手动设置DNS服务器，但未能成功

在与客服的沟通后，客服表示由于大清自有国情在，他们也对此没有好的方法，只是建议我可以尝试自己手动设置DNS服务器。于是我通过参考laoD同志博客里的一片文章[1],依次添加南大本身提供的、DNS+、114以及Google的8.8.8.8 DNS服务器。

初步尝试结果是令人沮丧的，没有任何变化。之后我冒出了搭建本地DNS服务器的想法，首先我选择修改本地hosts文件，向其中尝试性加入baidu.com的ip地址，并成功地在连接VPN的情况下以200ms的延迟ping通了baidu.com，并成功访问了其首页。

# 刷路由表实现流量分流

之后通过刷路由表的方式来让国内国外的流量分别走自己的通道，详参GitHub上的chnroutes项目[2]。再添加完静态路由后，我发现了此时ping baidu.com 会发生"General Failure"，流量被阻断。此时我怀疑是ExpressVPN的设置问题导致了这一现象。

通过观察APP的options选项卡我发现，ExpressVPN出于安全考虑禁止了一切不通过其通道的流量并且在VPN连接不畅时会自动断网。我猜想这可能是导致我发生错误的原因，在勾除掉这一选项后，一切正常。所有的域名解析，也表现正常。

# 结论

ExpressVPN出于安全考虑对流量进行了很大的限制，所以想要在连接VPN的情况下正常快速访问国内网站，首先需要在options选项卡中勾选掉"Enable Network Lock"选项，其次在网络连接中手动设置能够解析国内域名的DNS服务器地址，最终在不连接VPN的情况下刷路由表，最终可实现功能。

# 参考文献

[1] 2017公共DNS服务器评估报告——公共DNS推荐: https://laod.cn/hosts/2017-dns-pinggu.html 

[2] fivesheep/chnroutes: https://github.com/fivesheep/chnroutes 
