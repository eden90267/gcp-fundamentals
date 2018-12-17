# Google Cloud Infrastructure

## Chap 01. Why GCP ？

1. 網路
  - GCP：10ms
  - AWS：42ms
2. 價格
  - CALCURATOR
  - 後進者所以一定要比較便宜
3. Mulitple Open Source Service
  - 可移轉其他雲端服務

## Chap 02. Getting Started

- 使用雲端專注商業邏輯
- 權限管理
  - Resource hierarchy levels
  - Resources (低) -> Projects -> Folders -> Org Node (高)
  - 有權限繼承
  - Folders
    - 權限控管的彈性位置
  - Project
    - 資源管理
    - 管權限
    - 管 Services / APIs
    - 綁一個付費方式
  - 多個權限時，越大則會覆蓋越小的
  - IAM
    - 誰可以做什麼，在什麼資源上
    - 三種身份
      - 個人帳號
      - Service account
      - Google Group
      - Cloud Identity or G Suite domain
        - 才有繼承概念
    - Services / Resources/ Verb
    - Roles
      - Primitive (最傳統)
        - Owner
        - Editor
        - Viewer
      - Predefined
        - Services / Resources/ Verb 設定
      - Custom
- Service Accounts
  - Service to Service (or APIs)
  - using keys
  - 可跨 project
- Cloud Identity
  - 組織管理用途
  - Microsoft AD 可用 Google Cloud Directory Sync 過去 
- Google Cloud Platform Console
- Goolge Cloud SDK
  - CLI tools
- 所有 GCP 操作都用 RESTful APIs
  - 可用 API Explorer
- Marketplace


## Chap 03. VM

- VPC
  - 控制拓僕跟組織的聯繫
  - networks are global
  - subnets 可直接內網溝通
  - Shared VPC
    - 企業專門組織做 network admin
    - Host Project 單純做 Network 管理
    - 可設定某個 service project 只能連哪個 region
  - VPC Peering
    - A/B 不同部門有網路管理員，但又需要互相溝通 (Peer-BA 設定)
    - IP range 不能衝突，要自己注意
- local SSD
  - 實體放置，關閉會遺失資料
  - 不然就選 SSD 方案
- Resize disks
  - 不能縮小只能調大
- 可 startup scripts
- 可選擇 Preemptible
  - 會 24 小時內被搶資源
  - 費用低
- CPU 高，網路 Gps 就越快
- 硬碟越大，I/O 就越快，無法指定
- Scale up (資料庫) / Scale out (小機器叢集)
- Load Balancing
  - 請求單一處理到世界各地服務
    - Proxy
      - 後端 Server 看到的 IP 是 Load Balancer
      - SSL 用
      - 有 Port 限制
    - Forward
      - 後端 Server 看到的 IP 是 Client
      - 沒有 Port 限制
      - internal 用途
- DNS
  - 8.8.8.8, 4.4.4.4 都是 Google 提供的 DNS 服務
- CDN
  - Cache 內容在世界各地
  - 圖片、靜態資源等網站
- interconnect options
  - 光纖網路直接接 Google
  - 混合雲用
  - VPN (最基本)
  - Direct Peering (直接機台放 Google 機房)
  - Carrier Peering (電信商放 Google 機房)
    - 四方電信、中華電信
    - 可用鋼網站查詢：[https://www.peeringdb.com/net/433](https://www.peeringdb.com/net/433)
  - Dedicated Interconnect (企業連接，SLA 保證)
  - Partner Interconnect (電信商連接，SLA 保證)

## Chap 04. Storage

- 產品很多，哪個適合自己？
- Cloud Storage
  - 物件儲存的產品
  - 每次上傳都是獨立物件
  - 內建加解密 (可自訂)
  - 可從不同來源上傳下載
  - create bucket is global unique name
  - Location
  - 可不走 IAM
  - 可作版本控制
  - 具生命週期管理規則
  - 整合很多 GCP 服務
- Cloud Bigtable
  - NoSQL
  - 大量資料讀寫 (IOT 產業)
  - Machine Learning
  - 數據分析用
- Cloud SQL
  - MySQL
  - PostgreSQL
  - read replica 可 external service
  - 資料量不可太多
  - connects 4000 超過不適合
  - 10TB 的資料量不適合
- Cloud Spanner (前面兩者不適合項目可避免)
- Cloud Datastore
  - 優異的 transaction 應用，撇除讀寫 I/O 損號
  - 應用程式用
- Big Query
  - 資料倉儲服務
- VM 平均 35 秒開啟
  - AWS 肯定超過 50 秒
- VM 若不同 Zone 要互 ping 要記得設定 Metadata
  - VmDnsSetting=ZonePreferred
- 可用 Database Private IP 連接，Google Search：Configuring GCP Private IP Connectivity

## Chap 05. Containers

- k8s 介於 IaaS 和 PaaS 之間
- container
  - 啟動速度快
  - 耗費資源低
- 為什麼 container ?
  - 一制性
    - 環境
  - app 與 os 完全切開
  - migration 到任何地方 (to cloud)
  - 應用程式打包的一種格式創新技術
- 不過 container 在高可用性與資源安排是一個問題
  - k8s 解決這個問題
- k8s
  - auto deploy
    - rolling updates
  - scaling
  - clusters
- k8s engine ?
  - run 在 GCP 好處？
    - local env 困難，GCP 幫你管理
    - 定期更新版本
  - Google Cloud Container Builder
    - CI Image build
  - Google Container Registry
    - Image 管理
    - 檔案控管
