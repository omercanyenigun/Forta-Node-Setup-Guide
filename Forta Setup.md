# Forta-Node-Setup-Guide
**Forta, Merkezi Olmayan Web Stres Testini Duyurdu!** 
- **Güçlendirme, yüksek bant genişliğine sahip merkezi olmayan ağı stres testine tabi tutacak, dünyanın dört bir yanından katılımı teşvik edecek ve Forta topluluğunun dünyanın ilk gerçek zamanlı merkezi olmayan izleme ağının başlatılmasına hazırlanmasına yardımcı olacaktır.**

**Katılım Şartları:**
- **Resmi Discordlarına katılın**
- **Formu doldurun**
- **KYC'yi TokenSoft üzerinden yapın. (KYC için mail gelmesi gerek)**

**Ödüller**
- **1 hafta - 250 000 FORT** 
- **2 hafta - 250 000** 
- **FORT toplam arz 1.000.000.000'dır, 2 haftalık bir node için ödül toplam miktarın sadece %0.05'idir + genel listeden sadece 200 doğrulayıcı seçilecektir, bu yüzden kendi kararınızı verin.**

**Sistem Gereksinimleri**

- **4 CPU 16 RAM 100+ SSD Ubuntu 20.04**

**Kurulum**
- **Screen Sayfa Açma**
```
screen -S Anasayfa
```
- **Güncellemeler ve Docker Kurulumu**
```
sudo apt-get update
```
```
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```
```
sudo apt-get update
```
```
apt-cache madison docker-ce
```
**Son Komuttan sonra Packages yazan sıralı çıktılar almış olmanız gerek.**

**Her şey doğruysa, Docker'ı kurduk. Şimdi, nodeun kendisini Forta'dan yüklemeye geçelim:**

```
cd /etc/docker
```
```
touch daemon.json
```
```
nano daemon.json
```

- **Nano deamon.json sayfası açıldığında aşağıdaki kodu girin**

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
echo 'deb [signed-by=/usr/share/keyrings/forta-keyring.asc] https://dist.forta.network/repositories/apt stable main' | sudo tee -a /etc/apt/sources.list.d/forta.list
sudo apt-get update
sudo apt-get install forta
```
```
sudo curl https://dist.forta.network/artifacts/forta -o /usr/local/bin/forta
```
```
sudo chmod 755 /usr/local/bin/forta
```
- **En önemli yer! <your_passphrase> yerine bir şifre belirleyin ve bunu unutmayın.**
```
forta init --passphrase <your_passphrase>
```
- **Bu koddan sonra size bir cüzdan adresi verecek çıktı şöye gözükmelidir.**

```
Scanner address: 0x43919032A43E0Ed2Dda502756D8D9D7324836920

Successfully initialized at /root/.forta

- Please make sure that all of the values in config.yml are set correctly.
- Please fund your scanner address with some MATIC.
- Please enable it for the chain ID in your config by doing 'forta register --owner-address <your_owner_wallet_address>'.
```

- **Scanner address yazan yerde sizde çıkan adrese Tokensofta kayıtlı metamask adresinizden 0.1 Matic gönderin**

**Forta Klasörüne Girme**
```
cd $HOME/.forta
```
yada 
```
cd ~/.forta
```
**Alchemy**

- **https://www.alchemy.com/  adresinden bir profil oluşturun.**
- **Create APP yerinden ETH Mainnet seçerek devam edin.** 
- **Anasayfaya gelerek View Detals kısmından View Key e basarak oradaki HTTP ETH Mainnet linkini kopyalayın.**

**Şu koddan devam edin**
```
nanoconfig.yml
```
- **nanoconfig dosyasının içindeki**  
```
# The scan settings are used to retrieve the transactions that are analyzed
scan:
  jsonRpc:
    url: <required>

# The trace endpoint must support trace_block (such as alchemy)
trace:
  jsonRpc:
    url: <required>

```
- **Bu yerdeki URL yerine Alchemy den kopyalamış olduğunuz linki yapıştırın. Her iki url: yazan yere**
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
**Node başlatma kodu**
```
forta run --passphrase <şifreniz>
```

- **Kodu girdikten sonra şu çıktıyı almalısınız**
```
INFO[2022-04-27T14:08:13Z] start-up check successful                    
INFO[2022-04-27T14:08:13Z] replacing updater                             supervisor="disco.forta.network/bafybeihngcxtggkfuzr3f3wj27barxb3ssse3l4ibpo3b3d5tcwwadrdke@sha256:a3a0c9942927be0a42a7d9fba0dc5799f582595fd00943219dd5261518a9d00f" updater="disco.forta.network/bafybeihngcxtggkfuzr3f3wj27barxb3ssse3l4ibpo3b3d5tcwwadrdke@sha256:a3a0c9942927be0a42a7d9fba0dc5799f582595fd00943219dd5261518a9d00f"
INFO[2022-04-27T14:08:13Z] ensuring local image                          image="disco.forta.network/bafybeihngcxtggkfuzr3f3wj27barxb3ssse3l4ibpo3b3d5tcwwadrdke@sha256:a3a0c9942927be0a42a7d9fba0dc5799f582595fd00943219dd5261518a9d00f" name=updater
```

- **Bundan sonra https://dashboard.alchemyapi.io/ buradan nodeunuzu kontrol edebilirsiniz.**

**Son Olarak Size gönderilecek olan formu doldurun.**
Size gönderilecek olan formu doldurun.**
- **1) metamask adresiniz**
- **2)Node adresiniz (tarayıcı adresi)**
- **3)Tam adınız.**

**End**

- **https://t.me/testnetrun**

- **https://www.youtube.com/c/TechTakip**

- **https://testnet.run/**

- **https://stake.testnet.run/**

- **https://twitter.com/testnetrun**









