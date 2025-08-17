---
title: Synology DSM 7.2
weight: 20
---
<!--to translater: When translating elements like "buttons", don't just translate, please refer actual naming in their interface.-->
在 DSM 7.2 更新之後，Synology 已將 "Docker" 套件改名為 "Container Manager"，它採用新的介面，並且在其圖形介面內建 docker-compose，可讓您更容易地建立 Docker。
# 支援的機型以及需求

Container Manager 為部分低階的 ARM64 的機型帶來支援，例如 j 系列，如要獲取更多支援機型，請參閱 [Synology 網站](https://www.synology.com/zh-tw/dsm/packages/ContainerManager)。

# 1. 安裝 Container Manager (Docker)

開啟"套件中心"，搜尋並安裝 "Container Manager"。

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-7/images/dsm7_install_container_manager_though_package_center.png)

# 2. 建立資料夾

在您安裝完 "Container Manager" 之後，它會建立一個叫做 "docker" 的共享資料夾，讓我們把伺服器的資料放這。

打開您的 File Station，建立一個名叫 `rustdesk-server`(或您想要的名字)的資料夾，接著在其建立名為 `data` 的資料夾，如圖所示。

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-7/images/dsm7_create_required_folders.png)

# 3. 建立容器

打開您的 Container Manager，前往專案並點擊新增。

輸入您的專案名稱 `rustdesk-server` 然後變更來源從"上傳 compose.yml" 至 "建立 compose.yml"，接著複製下方內容到框框。

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-7/images/dsm7_creating_project_init.png?v2)

````yaml
services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    command: hbbs 
    volumes:
      - ./data:/root
    network_mode: host
    depends_on:
      - hbbr
    restart: always

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest # 如果您想安裝 Pro 版本，請將此更改為 rustdesk/rustdesk-server-pro:latest。
    command: hbbr
    volumes:
      - ./data:/root
    network_mode: host
    restart: always

# 因為使用 docker host mode
# 以防你忘記這些端口:
# 21114 TCP 用於網頁控制台，僅在 Pro 版本中可用
# 21115 TCP NAT 類型測試
# 21116 TCP TCP 打洞
# 21116 UDP 心跳/ID 伺服器
# 21117 TCP Relay/中繼
 ````

請略過 `網頁入口設定` 接著完成。

 ## 4. 檢查可否運作

打開您的 File Station 您應該可看到 `id_ed25519`、`id_ed25519.pub` 在您的 `docker/rustdesk-server/data` 資料夾。

公鑰看起來會像這個樣子:

![](/docs/en/self-host/rustdesk-server-oss/synology/dsm-7/images/dsm7_viewing_public_key_though_syno_text_editor.png)

看看[這裡](/docs/zh-tw/client)來設置您的客戶端，只有 `ID 伺服器` 以及 `Key` 是需要的，中繼伺服器不需設定，因為我們已經把它設置在 `hbbs` 了，hbbs 會自動提供這項資訊。

# 5. 在您的路由器設置 port forwarding (通訊埠轉發)

前往您的路由器的管理頁面，尋找任何有關於 `Port forwarding` 或是 `通訊埠轉發` 的設定，他應該在 `WAN`、`網際網路` 或是 `防火牆` 設置。

如果您還是無法找到設定，Google 搜尋 `{路由器廠牌} + port forwarding` 或 `{路由器型號} + port forwarding`

開啟這些需要的端口:
  * `21115` `TCP` NAT 類型測試
  * `21116` `TCP` TCP 打洞
  * `21116` `UDP` 心跳/ID 伺服器
  * `21117` `TCP` Relay/中繼
