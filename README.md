# OZHEX Binary Distributions

Bu depo yalnızca OZHEX'in son kullanıcı dağıtım dosyaları ve kurulum belgeleri içindir. Derleyici kaynak kodu bu depoya eklenmez.

## Dağıtım dosyaları

GitHub Releases bölümünde yayımlandığında aşağıdaki dosyalar bulunur:

| Dosya | Platform | Açıklama |
|---|---|---|
| OZHEX-WSL2-Setup-x64.exe | Windows x64 + WSL2 | WSL2/Ubuntu kurulumunu hazırlar ve Linux x86-64 OZHEX araç zincirini kurar. |
| ozhex-linux-x86_64.tar.gz | Linux x86-64 | Linux native compiler, CLI araçları ve standart kütüphane. |
| OZHEX-Native-Setup-x64.exe | Windows x64 | WSL2 gerektirmeyen native Windows kurulumu. Yalnızca Windows compiler/runtime tamamlandığında yayımlanır. |

Henüz yayımlanmamış veya doğrulanmamış bir dosya hazır kabul edilmemelidir. Native Windows kurucusu, Windows runtime ABI tamamlanmadan dağıtıma çıkarılmaz.

## Kurulum

Ayrıntılı yönergeler için [KURULUM.md](KURULUM.md) dosyasına bakın.

## Kaynak kodu

OZHEX geliştirme deposu ayrı tutulur: https://github.com/ismailozdemir01/OZHEX

Son kullanıcıların OZHEX'i kurmak için kaynak depoyu klonlaması gerekmez.
