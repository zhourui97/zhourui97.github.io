# google search的替代
1. duckduckgo
2. aol

# google ip
给两个测试google ip的方法

1. 最简单的测试方法就是在Chrome中以https方式访问该IP地址，如果返回信息中提示该服务器为google.com或带国家地区后缀如google.com.hk的服务器，则此IP确认100%可用。如果返回信息是其他Goolge服务器，则不建议使用。
2. 使用wget

	wget --content-disposition --no-check-certificate --tries=1 -T 10 https://ip

香港：
218.253.0.80-218.253.0.90
218.189.25.166-218.189.25.187

韩国：
121.78.74.80-121.78.74.88

俄罗斯：
178.45.251.84-178.45.251.123


台湾：
210.61.221.148-210.61.221.187
61.219.131.84-61.219.131.251
202.39.143.84-202.39.143.123
203.66.124.148-203.66.124.251
203.211.0.20-203.211.0.59
60.199.175.18-60.199.175.187

日本：
218.176.242.20 -218.176.242.251

新加坡：
203.116.165.148-203.116.165.251
203.117.34.148 – 203.117.34.187

美国：
74.125.128.*

北京：
203.208.46.*

仅限ip访问：
- http://203.208.46.145/
- http://203.116.165.225/
- http://www.googlestable.com/


- http://www.kookle.co.nr/ 也有数千个 GGC IP 地址，还嫌少的可以去看看。

# 解除youtube限制
解除地区限制方法一：goagent的GAE代理方式可以通过修改配置文件的方法解除部分限制，简单原理就是让Youtube判断你访问视频使用的ip为中国ip，而不是默认的GAE出口ip。这项设置在新版本配置文件中本身就有，只不过默认没有开启，将该行前面的注释符;编辑掉即可开启，参考如下。

	https?://www.youtube.com/watch = google_hk

方法二：选择没有限制的出口ip做代理。可以自己找个无限制ip的php或者PaaS空间.

参阅：[解除youtube限制](http://www.faith.ga/1976.html)