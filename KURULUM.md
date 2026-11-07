# OZHEX Kurulum Kılavuzu

Bu yönergeler yalnızca bu depoda yayımlanan dağıtım dosyalarını kullanır. Kaynak kod deposunu klonlamanız gerekmez.

## Windows x64 — WSL2 kurulumu

1. Releases bölümünden OZHEX-Setup-x64.exe dosyasını indirin.
2. Kurucuyu çalıştırın ve Windows yönetici iznini onaylayın.
3. Kurucu WSL2 bileşenlerini etkinleştirmek veya Ubuntu'yu indirmek için yeniden başlatma/ilk açılış isteyebilir. İstenen işlemi tamamlayıp kurucuyu yeniden çalıştırın.
4. Ubuntu ilk kez açıldığında Linux kullanıcı adı ve parolasını oluşturun.
5. OZHEX kurucusunu yeniden çalıştırın.
6. Ubuntu terminalinde şu komutu çalıştırın: ozhex --version

WSL2 kurucusu Windows üzerinde native compiler kurmaz; Linux x86-64 compiler'ı Ubuntu içinde çalıştırır.

## Linux x86-64 kurulumu

Gereksinimler:
- Linux x86-64
- Python 3.11 veya üzeri
- tar ve gzip

Önce yayımlanan checksum dosyasını doğrulayın:

    sha256sum -c ozhex-linux-x86_64.tar.gz.sha256

Arşivi açıp kurun:

    tar -xzf ozhex-linux-x86_64.tar.gz
    cd OZHEX
    bash installer/install.sh

Kurulumdan sonra:

    export PATH="$HOME/.local/bin:$PATH"
    ozhex --version

Kurucu compiler runtime, CLI araçları ve standart kütüphaneyi kullanıcı alanına yükler.

## Native Windows x64

OZHEX-Native-Setup-x64.exe yalnızca tam native Windows compiler, Windows runtime ABI ve Windows CLI paketi tamamlanıp yayımlandığında kullanılabilir. Bu dosya Releases bölümünde yoksa native Windows kurulumu henüz yayımlanmamıştır.

PE32+ biçiminde tekil Windows uygulaması üretilebilmesi, OZHEX compiler'ın Windows üzerinde native çalıştığını göstermez. WSL2 kurucusu ile native Windows kurucusu birbirinin yerine geçmez.

## Dosya doğrulama

Her release için yayımlanan SHA-256 değerini bağımsız olarak kontrol edin. Kaynağı ve checksum'ı aynı indirme kanalından almak, imza doğrulamasının yerini tutmaz.
