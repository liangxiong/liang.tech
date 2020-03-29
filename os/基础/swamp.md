# swamp

## 配置虚拟内存：
- 创建：
mkdir swap
cd swap
sudo dd if=/dev/zero of=swapfile bs=1024 count=100000

- 把生成的文件转换成 Swap 文件
sudo mkswap swapfile

- 激活 Swap 文件。
sudo swapon swapfile

- 重启保存
vim /etc/fstab
swapfilepath swap swap defaults 0 0

- 卸载：
sudo swapoff swapfile
