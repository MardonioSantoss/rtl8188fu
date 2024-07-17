Driver RTL8188FU para kernel Linux 4.15.x ~ 6.7.x (Linux Mint, Ubuntu ou Debian Derivatives)


info: o suporte rtl8188fu será adicionado ao módulo rtl8xxxu do kernel Linux. https://patchwork.kernel.org/project/linux-wireless/patch/b14f299d-3248-98fe-eee1-ba50d2e76c74@gmail.com/


------------------

## Como instalar

`sudo apt-get install build-essential git dkms linux-headers-$(uname -r)`

`git clone https://github.com/kelebek333/rtl8188fu`

`sudo dkms install ./rtl8188fu`

`sudo cp ./rtl8188fu/firmware/rtl8188fufw.bin /lib/firmware/rtlwifi/`

------------------

## Configuração

#### Desative o gerenciamento de energia

Execute os seguintes comandos para desativar o gerenciamento de energia e problemas de conexão/reconexão.

`sudo mkdir -p /etc/modprobe.d/`

`sudo touch /etc/modprobe.d/rtl8188fu.conf`

`echo "options rtl8188fu rtw_power_mgnt=0 rtw_enusbss=0 rtw_ips_mode=0" | sudo tee /etc/modprobe.d/rtl8188fu.conf`


#### Desativar falsificação de endereço MAC

Execute os seguintes comandos para desabilitar a falsificação de endereço MAC (Nota: isso não é necessário em distribuições baseadas no Ubuntu. A falsificação de endereço MAC já está desabilitada na base do Ubuntu).

`sudo mkdir -p /etc/NetworkManager/conf.d/`

`sudo touch /etc/NetworkManager/conf.d/disable-random-mac.conf`

`echo -e "[device]\nwifi.scan-rand-mac-address=no" | sudo tee /etc/NetworkManager/conf.d/disable-random-mac.conf`

#### Blacklist (alias) para kernel 5.15 e 5.16 (não é necessário para kernel 5.17 e superior)

Se você estiver usando o kernel 5.15 e 5.16, você deve criar um arquivo de configuração com o seguinte comando para evitar conflito entre o módulo rtl8188fu e o módulo r8188eu integrado.

`echo 'alias usb:v0BDApF179d*dc*dsc*dp*icFFiscFFipFFin* rtl8188fu' | sudo tee /etc/modprobe.d/r8188eu-blacklist.conf`

#### Blacklist (alias) para kernel 6.2 e superior

Se você estiver usando o kernel 6.2 e superior, você deve criar um arquivo de configuração com o seguinte comando para evitar conflito entre o módulo rtl8188fu e o módulo rtl8xxxu integrado.

`echo 'alias usb:v0BDApF179d*dc*dsc*dp*icFFiscFFipFFin* rtl8188fu' | sudo tee /etc/modprobe.d/rtl8xxxu-blacklist.conf`

##### Então você deve atualizar o initramfs

Para initramfs

`sudo update-initramfs -u`

Para dracut

`sudo dracut -q --force`

##### Habilitar módulo rtl8188fu

Execute o seguinte comando para o kernel 6.1

`sudo modprobe rtl8188fu`

Execute os seguintes comandos para kernel 6.2 e superior

`sudo modprobe -r rtl8188fu`

`sudo modprobe rtl8188fu`

------------------

## Como desinstalar

`sudo dkms remove rtl8188fu/1.0 --all`

`sudo rm -f /lib/firmware/rtlwifi/rtl8188fufw.bin`

`sudo rm -f /etc/modprobe.d/rtl8188fu.conf`


------------------

## How to install from PPA repository

You can install rtl8188fu driver with following commands from PPA.

for xUbuntu 16.04-18.04-20.04-22.04-23.04-23.10 / Linux Mint 20.x-21.x

`sudo add-apt-repository ppa:kelebek333/kablosuz`

`sudo apt-get update`

`sudo apt install rtl8188fu-dkms`


You can purge packages with following commands

`sudo apt purge rtl8188fu-dkms`

------------------
