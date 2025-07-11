### Thông tin

-   Chuẩn bị: 0.5$ ETH trên Base mainnet
-   Tạo URL RPC tạo: https://alchemy.com
-   OKOK

### Cấu hình tối thiểu

-   CPU 2 Core
-   Ram 2 GB
-   Ổ cứng SSD 20 GB
-   Hệ điều hành linux Ubuntu 22.04 trở lên
    **Note**: Có thể mua gói basic nhất 5$/tháng tại https://contabo.com/en/vps/.

### Code chạy

1. Update

```
sudo apt-get update && sudo apt-get upgrade -y
```

2. Cài Docker

```
sudo apt-get install docker.io -y
```

3. Cài đặt Nodejs

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

4. Cài đặt nvm

```
nvm install 20
```

5. Cài đặt Foundry

```
curl -L https://foundry.paradigm.xyz | bash
```

```
source /root/.bashrc
```

```
foundryup
```

6. Cài đặt Hyperlane Client

```
npm install -g @hyperlane-xyz/cli
```

7. Clone source Hyperlane từ github repo

```
docker pull --platform linux/amd64 gcr.io/abacus-labs-dev/hyperlane-agent:agents-v1.0.0
```

8. Tạo thư mục node database

```
mkdir -p /root/hyperlane_db_base && chmod -R 777 /root/hyperlane_db_base
```

9. cấu hình Hyperlane validator

```
docker run -d \
  -it \
  --name hyperlane \
  --mount type=bind,source=/root/hyperlane_db_base,target=/hyperlane_db_base \
  gcr.io/abacus-labs-dev/hyperlane-agent:agents-v1.0.0 \
  ./validator \
  --db /hyperlane_db_base \
  --originChainName base \
  --reorgPeriod 1 \
  --validator.id <Name> \
  --checkpointSyncer.type localStorage \
  --checkpointSyncer.folder base \
  --checkpointSyncer.path /hyperlane_db_base/base_checkpoints \
  --validator.key <PRIVATE_KEY> \
  --chains.base.signer.key <PRIVATE_KEY> \
  --chains.base.customRpcUrls <RPC_URL>
```

(\*\*\*) NOTE:

-   Thay < NAME > bằng Tên của các bạn (chỗ này là validator name)
-   Thay <PRIVATE_KEY> bằng Private key ví của các bạn
-   Chỗ Privatekey anh em có thể import ví đã tạo sẵn hoặc có thể tạo ví mới.
-   Tạo ví mới bằng dòng code sau:

```
cast wallet new
```

-   Sau khi tạo ví xong thì coppy Private key import vào metamask để sử dụng

10. Check log

```
docker logs -f hyperlane
```
