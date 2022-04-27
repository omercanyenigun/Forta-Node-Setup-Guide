# Forta-Node-Setup-Guide
**Forta, Merkezi Olmayan Web Stres Testini Duyurdu!** 
- **Güçlendirme, yüksek bant genişliğine sahip merkezi olmayan ağı stres testine tabi tutacak, dünyanın dört bir yanından katılımı teşvik edecek ve Forta topluluğunun dünyanın ilk gerçek zamanlı merkezi olmayan izleme ağının başlatılmasına hazırlanmasına yardımcı olacaktır.**

**Katılım Şartları:**
- **Resmi Discordlarına katılın**
- **Formu doldurun**
- **KYC'yi TokenSoft üzerinden yapın. (KYC için mail gelmesi gerek)**

**Ödüller**
- **1. hafta - 250 000 FORT** 
- **2. hafta - 250 000** 
- **FORT toplam arz 1.000.000.000'dır, 2 haftalık bir node için ödül toplam miktarın sadece %0.05'idir + genel listeden sadece 200 doğrulayıcı seçilecektir, bu yüzden kendi kararınızı verin.**

**Sistem Gereksinimleri**

- **4 CPU 16 RAM 100+ SSD Ubuntu 20.04**

**Kurulum**

- **Güncellemeler ve Docker Kurulumu**
```
apt update
```
```
apt upgrade
```
```
apt install ca-certificates curl gnupg lsb-release git htop
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
apt-get update
```
```
apt-get install docker-ce docker-ce-cli containerd.io
```
```
docker version
```
```
nano /etc/docker/daemon.json
```


- **deamon.json sayfası açıldığında aşağıdaki kodu girin**

```

{ 
   "default-address-pools": [ 
        { 
            "base":"172.17.0.0/12", 
            "size":16 
        }, 
        { 
            "base":"192.168.0.0/16" , 
            "size":20 
        }, 
        { 
            "base":"10.99.0.0/16", 
            "size":24 
        } 
    ] 
}
```
- **Kodu girdikten sonra CTRL-X Yapıp Sonra Y enter yaparak dosyayı kaydedin.**

**Yeniden başlatma**
```
systemctl restart docker
```

- **Biraz bekledikten sonra (30sn) şu kodları girin**

```
sudo curl https://dist.forta.network/pgp.public -o /usr/share/keyrings/forta-keyring.asc -s
```
```
echo 'deb [signed-by=/usr/share/keyrings/forta-keyring.asc] https://dist.forta.network/repositories/apt stable main' | sudo tee -a /etc/apt/sources.list.d/forta.list
```
```
apt-get update
```
```
apt-get install forta
```

- **En önemli yer! <your_passphrase> yerine bir şifre belirleyin ve bunu unutmayın.**
```
forta init --passphrase <your_passphrase>
```
- **Bu koddan sonra size bir cüzdan adresi verecek çıktı şöyle gözükmelidir.**

```
Scanner address: 0x43919032A43E0Ed2Dda502756D8D9D7324836920

Successfully initialized at /root/.forta

- Please make sure that all of the values in config.yml are set correctly.
- Please fund your scanner address with some MATIC.
- Please enable it for the chain ID in your config by doing 'forta register --owner-address <your_owner_wallet_address>'.
```

- **Scanner address yazan yerde sizde çıkan adrese Tokensofta kayıtlı metamask adresinizden 0.1 Matic gönderin**

**Alchemy**

- **https://www.alchemy.com/  adresinden bir profil oluşturun.**
- **Create APP yerinden Polygon Mainnet seçerek devam edin.** 
- **Anasayfaya gelerek View Detals kısmından View Key e basarak oradaki HTTP Polygon Mainnet linkini kopyalayın.**

**Şu koddan devam edin**
```
nano /root/.forta/config.yml
```
- **config dosyasının içindekileri CTRL-K ile silin**  

```
chainId: 137

scan:
  jsonRpc:
    url: <buraya alchemyden kopyaladığınız linki girin>

trace:
  enabled: <buraya alchemyden kopyaladığınız linki girin>
```
  
  - **Kodu girdikten sonra CTRL-X Yapıp Sonra Y enter yaparak dosyayı kaydedin.**
  

- **Aşağıdaki kodu girdiğinizde**
```
forta register --owner-address <metamask adresiniz> --passphrase <şifreniz>
```
- **Şu çıktıyı almalısınız.**
```
Sending a transaction to register your scan node to chain 1...
Successfully sent the transaction!

Please ensure that https://polygonscan.com/tx/0xc61929753a2d25f53c7c18d731d205e1f47e767206da3fe28266e528fa10041f succeeds before you do 'forta run'. This can take a while depending on the network load.
```
```
systemctl enable forta
```
```
nano /lib/systemd/system/forta.service
```
- **Dosyanın içindekileri CTRL-K ile silin <şifreniz> olan yeri değiştirin.**

```
[Unit]
Description=Forta
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
Environment="FORTA_DIR=/root/.forta/"
Environment="FORTA_PASSPHRASE=<şifreniz>"
Restart=on-failure
RestartSec=15s

ExecStart=/usr/bin/forta run

[Install]
WantedBy=multi-user.target
```
 - **Yukarıdaki kodu yapıştırdıktan sonra CTRL-X Yapıp Sonra Y enter yaparak dosyayı kaydedin.**

**Node Görüntüleme**

```
systemctl daemon-reload
```
```
systemctl restart forta
```
```
systemctl status forta
```

- **Bundan sonra https://dashboard.alchemyapi.io/ buradan nodeunuzu kontrol edebilirsiniz.**

**Son Olarak Size gönderilecek olan formu doldurun.**
- **1) metamask adresiniz**
- **2)Node adresiniz (tarayıcı adresi)**
- **3)Tam adınız.**

**End**

- **https://t.me/testnetrun**

- **https://www.youtube.com/c/TechTakip**

- **https://testnet.run/**

- **https://stake.testnet.run/**

- **https://twitter.com/testnetrun**









