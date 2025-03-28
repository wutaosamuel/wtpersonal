# Homeassitant 部署


## Homeassitant docker 安装

docker-compose.yml

```yml
version: "3"

services:
  homeassitant:
    container_name: homeassistant
    image: homeassistant/home-assistant
    restart: unless-stopped
    network_mode: host
    privileged: true
    # environment:
    # - https_proxy=http://172.16.8.168:8123
    volumes:
      - /root/homeassistant/config:/config
      - /etc/localtime:/etc/localtime:ro
```

创建并运行 docker

```bash
docker-compose up -d
```

## HACS 安装 (Integrations of the community store/社区插件)

如果 hacs 没有安装, 可以手动安装

```bash
# 进入docker容器
docker exec -it homeassistant bash

# 运行hacs安装脚本
wget -O - https://get.hacs.xyz | bash -

# 退出容器
exit
```

完成之后, 重启 homeassistant docker 或者重启服务(在配置中的服务控制)。
如果连接 hacs.xyz 太慢, 可以在 docker compose 文件中的 environment 添加 https 代理.

更多的信息, 请参考[官方指南](hacs.xyz/docs/configuration/basic)

## 插件/Integrations

因为网络问题, 大多数插件我都是手动安装的.

### 主题/Themes

这里使用了 lovelace-ios-dark-mode-theme 主题作为参考

```bash
# 下载主题
git clone https://github.com/basnijholt/lovelace-ios-dark-mode-theme.git PATH/TO/HOMESSISTANT/config/themes
```

添加下面两行到 PATH/TO/HOMESSISTANT/config/configuration.yaml

```yml
frontend:
  themes: !include_dir_merge_named themes
```

最后, 重启 homeassistant 服务以及在用户的主题下更改主题

### HomeKit

1. 直接下载 home assistant 官方 HomeKit 插件
2. 在通知中, 用苹果设备配对二维码或者 8 个数字
3. 完成 HomeKit 配置

### 小米/XiaoMi MIoT (Pending)

```bash
git clone https://github.com/ha0y/xiaomi_miot_raw.git
cp -dr ./xiaomi_miot_raw/custom_components/xiaomi_miot_raw PATH/TO/HOMESSISTANT/config/custom_components
# restart homeassistant docker or restart server in Server Controls in Configuration
docker restart homeassistant
```

最后, 设置小米的账户

## 设置 IOS 的家庭&快捷方式/Home&Shortcut

下面几点是我在设置 IOS 所遇到的问题

- 家庭 app 只能在家庭局域网中连接
  - 还没找到方案
- 家庭/快捷指令 app 无法设置只开或者只关的动作
- 解决只开或者只关的动作后, 家庭 app 中的按钮状态(亮或者暗)与实际智能设备状态不一致. 因为只要单向动作只需要运行, 不需要'关'运行.

### 只开或者只关的动作

因为默认的家庭 app 开关可以开和关, 但是没有只开和只关功能. 这只能通过 homeassistant 的`脚本`功能来实现. 这需要在`配置/脚本`中配置.
这个功能也可以预设智能设备的值, 比如设置灯光亮度值等.

### IOS 的家庭&快捷方式/ome&Shortcut

所有脚本都设置完成后, 家庭 app 中的配件可能不会显示脚本. 需要重新配置 HomeKit 插件配置, `配置中先移除再添加脚本元素`.

`在快捷方式app中, 这种单向动作最号需要触发2次`, 因为第二次动作可以把开关图标状态变暗.

