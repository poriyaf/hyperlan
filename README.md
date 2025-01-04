# نصب hyperlane
- اول به پریویت کی یک والت و یک rpc شخصی سازی شده شبکه بیس نیاز دارید برای ساختن ان به این ادرس مراجعه کنید https://dashboard.alchemy.com/apps/zfh4fwnitfwpd1l6/networks
- همچنین به یک دلار اتریوم برای پرداخت gas در شبکه بیس در همین والت نیاز دارید
- کد ها را تجمیع کردم تا نصب راحت تر باشد در این مدل نصب باید پریویت کی و rpc را خودتان وارد کنید
  ```
  #!/bin/bash

# به‌روزرسانی سیستم
echo "Updating system..."
sudo apt-get update && sudo apt-get upgrade -y

# نصب Docker
echo "Installing Docker..."
sudo apt-get install -y docker.io

# نصب Foundry
echo "Installing Foundry..."
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup

# نصب Node.js و nvm
echo "Installing Node.js and nvm..."
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
nvm install 20

# نصب Hyperlane CLI
echo "Installing Hyperlane CLI..."
npm install -g @hyperlane-xyz/cli

# دانلود Hyperlane Docker Image
echo "Downloading Hyperlane Docker Image..."
docker pull --platform linux/amd64 gcr.io/abacus-labs-dev/hyperlane-agent:agents-v1.0.0

# پیکربندی و اجرای نود Hyperlane
echo "Setting up and running Hyperlane node..."
mkdir -p ~/hyperlane_db_base && chmod -R 777 ~/hyperlane_db_base

docker run -d \
  -it \
  --name hyperlane \
  --mount type=bind,source=$HOME/hyperlane_db_base,target=/hyperlane_db_base \
  gcr.io/abacus-labs-dev/hyperlane-agent:agents-v1.0.0 \
  ./validator \
  --db /hyperlane_db_base \
  --originChainName base \
  --reorgPeriod 1 \
  --validator.id my_validator \
  --checkpointSyncer.type localStorage \
  --checkpointSyncer.folder base \
  --checkpointSyncer.path /hyperlane_db_base/base_checkpoints \
  --validator.key  your private key \
  --chains.base.signer.key  your private key \
  --chains.base.customRpcUrls  your rpc
# بررسی لاگ‌ها
echo "Displaying logs for Hyperlane node..."
docker logs -f hyperlane

  ```
- اگر نصب موفقیت آمیز و طبق عکس زیر لاگ ها را نشان بده میتوانید با ctrl+c پنجره را ببندید


![log hyperlane](https://github.com/user-attachments/assets/9e0b5670-4669-4741-bfb4-f4eb23ed4b90)

- ادرس والت را در سایت https://basescan.org/ چک کنید باید مثل عکس زیر باشه

![base hyper](https://github.com/user-attachments/assets/ca58e00c-7d1b-469b-ae0d-4c8e177cf434)
- نکته: این کد ها را سرویس chatgpt برام نوشته و شخصا 4بار نصب کردم اگه مشکلی بود از chatgpt بپرسید
