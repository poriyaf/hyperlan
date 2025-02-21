# نصب hyperlane
- اول به پریویت کی یک والت و یک rpc شخصی سازی شده شبکه بیس نیاز دارید برای ساختن ان به این ادرس مراجعه کنید https://dashboard.alchemy.com/apps/zfh4fwnitfwpd1l6/networks
- ![alchemy](https://github.com/user-attachments/assets/960323b1-77ad-4ac3-8b77-7459e51cd223)

- همچنین به یک دلار اتریوم برای پرداخت gas در شبکه بیس در همین والت نیاز دارید
- کد ها را تجمیع کردم تا نصب راحت تر باشد در این مدل نصب باید پریویت کی و rpc را خودتان وارد کنید


# به‌روزرسانی سیستم
  ```
 #!/bin/bash
echo "Updating system..."
sudo apt-get update && sudo apt-get upgrade -y
  ```
# نصب Docker
  ```
echo "Installing Docker..."
sudo apt-get install -y docker.io
  ```
# نصب Foundry
  ```
echo "Installing Foundry..."
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup
  ```
# نصب Node.js و nvm
  ```
echo "Installing Node.js and nvm..."
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
nvm install 20
  ```
# نصب Hyperlane CLI
  ```
echo "Installing Hyperlane CLI..."
npm install -g @hyperlane-xyz/cli
  ```
# دانلود Hyperlane Docker Image
  ```
echo "Downloading Hyperlane Docker Image..."
docker pull --platform linux/amd64 gcr.io/abacus-labs-dev/hyperlane-agent:agents-v1.0.0
  ```
# پیکربندی و اجرای نود Hyperlane
  ```
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
  --chains.base.customRpcUrls youtr rpc chain
  ```
# بررسی لاگ‌ها
  ```
echo "Displaying logs for Hyperlane node..."
docker logs -f hyperlane
  ```

- اگر نصب موفقیت آمیز و طبق عکس زیر لاگ ها را نشان بده میتوانید  پنجره را ببندید


![log hyperlane](https://github.com/user-attachments/assets/35da35d5-1c52-4dff-956d-d8600f94e4a8)


- ادرس والت را در سایت https://basescan.org/ چک کنید باید مثل عکس زیر باشه

![base hyper](https://github.com/user-attachments/assets/76c1a781-66f0-41c5-84bd-073d0670b36a)

 نکته: این کد ها را سرویس chatgpt برام نوشته و شخصا 4 تا نود نصب کردم اگه مشکلی بود از chatgpt بپرسید -
  
- 
برای مشاهده لاگ‌های زنده کانتینر (حتی پس از استارت مجدد)
 ```
docker logs -f hyperlane
 ```
- برای توقف کانتینر
 ```
 docker stop hyperlane
 ```
- برای استارت کانتینر
 ```
docker start hyperlane
 ```
