# Connect to IPV6 network with public-ipv4-only VPS


## 起因

很多云服务供应商只提供了公网ipv4的虚拟机。但是因为一些原因, 我们需要用到ipv6的网络来进行访问。这里我们用到的是ipv4到ipv6的转换服务。

## 步骤

1. 在[HE.net](https://tunnelbroker.net)上注册免费的ipv4到ipv6的转换服务
    1.1. 创建通道(create tunnel)
    1.2. 根据提示的配置(example configurations), 在虚拟机上配置ipv6穿透
2. 等待服务就绪

## IPV6配置穿透服务时候的问题

1. He.net无法使用gmail和outlook注册
    - 解决方案:用163注册, 注册成功的时间: 2024-12-21
2. He.net无法创建ipv6穿透, 原因是"这个网络受到限制(This network is restricted)"
    - 可能的原因是He.net把ip地址丢进了黑名单
    - 解决方案: 创建tunnel时，先使用白名单的ipv4地址。再修改ip, 成功创建的时间: 2024-12-21
3. He.net的穿透服务如果一段时间没有活动，是会自动关闭的
    - 解决方案: 每10分钟ping一次ipv6网站.
    Sh: `crontab -l 2>/dev/null | echo "*/10 * * * * ping -c 2 ipv6.google.com" | crontab -`

