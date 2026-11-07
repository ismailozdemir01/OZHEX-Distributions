# OZHEX — Sıfırdan İleri Seviyeye Son Kullanıcı Eğitim Kitabı

**Belge türü:** Son kullanıcı programlama eğitimi ve uygulamalı kullanım kitabı  
**Ürün:** OZHEX 0.6.0 araç zinciri  
**Hedef kitle:** Programlamaya yeni başlayanlar ve OZHEX ile uygulama geliştirmek isteyen kullanıcılar  
**Platform kapsamı:** Yayımlanmış Linux x86-64 paketi ve Windows x64 üzerinde WSL2 + Ubuntu  
**Resmî indirme deposu:** https://github.com/ismailozdemir01/OZHEX-Distributions  
**Resmî Releases:** https://github.com/ismailozdemir01/OZHEX-Distributions/releases  
**Geliştirici kaynak deposu:** https://github.com/ismailozdemir01/

> **Bu kitap son kullanıcılar içindir.** Kaynak kod deposunu klonlamanız, derleyiciyi kaynak koddan üretmeniz veya dahili build betiklerini çalıştırmanız gerekmez. Kurulum paketleri ve kullanıcı belgeleri yalnızca OZHEX-Distributions deposundan alınmalıdır. Bu kitap compiler geliştirme, ABI/backend, self-host bootstrap veya dahili release üretim işlemlerini öğretmez.

---

## İçindekiler

1. Bu kitap nasıl kullanılır?
2. OZHEX'e giriş
3. Doğru paketi indirme
4. Kurulum ve ilk doğrulama
5. Geliştirme klasörü hazırlama
6. İlk program
7. Programın temel yapısı
8. Değişkenler ve sabitler
9. Veri türleri
10. Operatörler ve ifadeler
11. Koşullar
12. Döngüler
13. Fonksiyonlar
14. Yapılar ve enum
15. Diziler
16. String ve karakterler
17. Standart girdi/çıktı
18. Kaynak modüllerini kullanma
19. Dosya işlemleri
20. TCP ağ programlama
21. Paket yöneticisi
22. Derleme hedefleri ve platform seçimi
23. Hata ayıklama
24. Güvenli programlama ve yayın öncesi kontrol
25. Uygulamalı projeler
26. Hızlı komut başvurusu
27. Sık sorulan sorular
28. Terimler sözlüğü

---

# 1. Bu kitap nasıl kullanılır?

Kitap, OZHEX'i ilk kez açan kullanıcıdan gerçek uygulama geliştirmek isteyen ileri seviye kullanıcıya kadar aşamalı ilerler.

- **Başlangıç:** Kurulum, ilk program, değişken, tür, koşul ve döngü.
- **Orta seviye:** Fonksiyon, yapı, enum, dizi, string ve modüller.
- **İleri seviye kullanıcı:** Dosya ve TCP modülleri, paket yönetimi, platform sınırları, hata ayıklama ve uygulama tasarımı.

Örnekleri kendi çalışma klasörünüzde `.ozhex` uzantılı dosyalara kaydedin. Kaynak dosyalarını UTF-8 kodlamasıyla saklayın.

Bu kitapta “destekleniyor” ifadesi yalnızca belgelenmiş kullanıcı arayüzü veya standart kütüphane işlevleri için kullanılır. Bir hedefin derleyici yardımında listelenmesi, o hedef için yayımlanmış tam bir kullanıcı paketi bulunduğu anlamına gelmez.

# 2. OZHEX'e giriş

OZHEX, `.ozhex` uzantılı kaynak dosyalarını native programlara derleyen bir programlama dili ve araç zinciridir. Kullanıcı olarak temel döngünüz şöyledir:

1. Kaynak kodunu yazarsınız.
2. `ozhex` komutuyla derlersiniz.
3. Oluşan çalıştırılabilir dosyayı başlatırsınız.
4. Programınızı gerçek girdilerle çalıştırıp davranışını kontrol edersiniz.

OZHEX kaynak kodu Türkçe anahtar sözcükler içerir. Örneğin fonksiyon tanımlamak için `fonksiyon`, değer döndürmek için `dön`, koşul için `eğer` kullanılır.

# 3. Doğru paketi indirme

İndirme adresi:

**https://github.com/ismailozdemir01/OZHEX-Distributions/dist**

| Kullandığınız sistem | Kullanılacak paket | Çalışma ortamı |
|---|---|---|
| Windows 10/11 x64 | `OZHEX-WSL2-Setup-x64.exe` | WSL2 içindeki Ubuntu |
| Linux x86-64 | `ozhex-linux-x86_64.tar.gz` | Doğrudan Linux |
| Native Windows x64 | Yalnızca native paket yayımlanırsa | Doğrudan Windows |

**Windows kullanıcıları için:** WSL2 kurucusu, Windows'a native Windows compiler kurmaz. Ubuntu ortamına Linux x86-64 OZHEX araç zincirini kurar. `ozhex` komutları Ubuntu terminalinde çalıştırılır.

Releases sayfasında bulunmayan bir dosyanın yayımlandığını varsaymayın. Native Windows paketi yoksa WSL2 paketini kullanın veya native Windows desteğinin yayımlanmasını bekleyin.

# 4. Kurulum ve ilk doğrulama

Ayrıntılı, işletim sistemine özel kurulum adımları için bu depodaki [KURULUM.md](KURULUM.md) belgesini izleyin.

## 4.1 Windows + WSL2

Kurulumdan sonra Ubuntu terminalini açın:

```bash
command -v ozhex
ozhex --version
ozhex --help
ozhexpm --help
```

Komut bulunamazsa:

```bash
ls -lah ~/.local/bin/ozhex ~/.local/bin/ozhexpm
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
hash -r
command -v ozhex
```

İlk komut dosyaların gerçekten kurulu olup olmadığını gösterir. Dosyalar yoksa PATH ayarı tek başına çözüm değildir; kurulum adımlarını yeniden kontrol edin.

## 4.2 Linux x86-64

İndirdiğiniz arşiv ve checksum dosyasını aynı klasöre koyun. Önce checksum doğrulayın:

```bash
sha256sum -c ozhex-linux-x86_64.tar.gz.sha256
```

Sonuç `OK` değilse arşivi kurmayın. Doğruysa:

```bash
tar -xzf ozhex-linux-x86_64.tar.gz
cd OZHEX
bash installer/install.sh
export PATH="$HOME/.local/bin:$PATH"
ozhex --version
```

Kalıcı PATH ayarı için Bash kullanıyorsanız:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

# 5. Geliştirme klasörü hazırlama

Her uygulamayı ayrı klasörde tutun:

```bash
mkdir -p ~/ozhex-projects/ilk-projem
cd ~/ozhex-projects/ilk-projem
```

Kaynak dosyalarının uzantısı `.ozhex` olmalıdır. Örnek:

```text
ilk-projem/
├── main.ozhex
└── program
```

`main.ozhex` sizin kaynak dosyanızdır; `program` derleme sonucunda oluşan çalıştırılabilir dosyadır.

# 6. İlk program

`main.ozhex` dosyasını oluşturun:

```ozhex
fonksiyon ana() -> int {
    yaz("Merhaba, OZHEX!");
    dön 0;
}
```

Derleyin:

```bash
ozhex main.ozhex program
```

Çalıştırın:

```bash
./program
```

Beklenen çıktı:

```text
Merhaba, OZHEX!
```

Programın giriş fonksiyonu `ana` adını taşır. `yaz` standart çıktıya değer gönderir. `dön 0` başarılı sonlanma kodunu belirtir.

# 7. Programın temel yapısı

Bir OZHEX programı fonksiyonlardan ve bloklardan oluşur. Bloklar süslü parantezlerle sınırlandırılır.

```ozhex
fonksiyon selamla() -> int {
    yaz("OZHEX programı başladı.");
    dön 0;
}

fonksiyon ana() -> int {
    selamla();
    dön 0;
}
```

Satır sonları ve noktalı virgüller örneklerdeki gibi kullanılmalıdır. Anahtar sözcüklerin yazımını değiştirmeyin.

# 8. Değişkenler ve sabitler

Değişken bildiriminde `değişken` kullanılır:

```ozhex
fonksiyon ana() -> int {
    değişken sayac: int = 10;
    sayac = sayac + 5;
    dön sayac;
}
```

Sabit bildiriminde `sabit` kullanılır:

```ozhex
fonksiyon ana() -> int {
    sabit LIMIT: int = 100;
    dön LIMIT;
}
```

Değişkeni kullanmadan önce uygun türde bir başlangıç değeri verin. Sabitleri değiştirilebilir durum bilgisi yerine değişmemesi gereken değerler için kullanın.

# 9. Veri türleri

OZHEX kaynaklarında bulunan temel tür ailesi:

| Tür | Kullanım |
|---|---|
| `int` | İşaretli tamsayı |
| `uint` | İşaretsiz tamsayı |
| `int8`, `int16`, `int32`, `int64` | Genişliği belirtilmiş işaretli tamsayı |
| `uint8`, `uint16`, `uint32`, `uint64` | Genişliği belirtilmiş işaretsiz tamsayı |
| `float` | Kayan noktalı sayı |
| `double` | Daha geniş kayan noktalı sayı |
| `bool` | Mantıksal değer |
| `char` | Karakter |
| `string` | Metin |
| `void` | Değer döndürmeyen fonksiyon türü |

Mantıksal değerler:

```ozhex
değişken etkin: bool = doğru;
değişken kapali: bool = yanlış;
```

Sayısal dönüşümlerde tür genişliğini ve sınırları dikkate alın. Tüm C/C++ dönüşüm davranışlarının veya tüm IEEE-754 sınırlarının OZHEX'te aynı olduğu varsayılmamalıdır.

# 10. Operatörler ve ifadeler

Belgelenmiş operatör grupları:

| Grup | Operatörler |
|---|---|
| Aritmetik | `+`, `-`, `*`, `/`, `%` |
| Karşılaştırma | `==`, `!=`, `<`, `<=`, `>`, `>=` |
| Mantıksal | `&&`, `||`, `!` |
| Bitsel | `&`, `|`, `^`, `~` |
| Kaydırma | `<<`, `>>` |
| Bileşik atama | `+=`, `-=`, `*=`, `/=`, `%=` ve bitsel karşılıkları |

Örnek:

```ozhex
fonksiyon ana() -> int {
    değişken a: int = 20;
    değişken b: int = 6;
    değişken toplam: int = a + b;
    değişken kalan: int = a % b;

    eğer toplam == 26 && kalan == 2 {
        dön 0;
    }
    dön 1;
}
```

Sıfıra bölme, taşma ve tür dönüşümü gibi sınır durumlarını uygulamanızda açıkça ele alın.

# 11. Koşullar

`eğer` ve `değilse` ile karar verilir:

```ozhex
fonksiyon ana() -> int {
    değişken puan: int = 75;

    eğer puan >= 50 {
        yaz("Başarılı");
    } değilse {
        yaz("Başarısız");
    }

    dön 0;
}
```

Birden fazla koşul mantıksal operatörlerle birleştirilebilir:

```ozhex
fonksiyon ana() -> int {
    değişken yas: int = 25;
    değişken onay: bool = doğru;

    eğer yas >= 18 && onay {
        yaz("İşlem kabul edildi");
    } değilse {
        yaz("İşlem reddedildi");
    }
    dön 0;
}
```

# 12. Döngüler

Döngüler tekrar eden işlemler için kullanılır. Depo örneklerinde `iken` koşullu döngü biçimi bulunur:

```ozhex
fonksiyon ana() -> int {
    değişken sayac: int = 0;

    iken sayac < 5 {
        yaz(sayac);
        sayac += 1;
    }

    dön 0;
}
```

Döngü koşulunun bir noktada yanlış olmasını sağlayın. Aksi durumda program sonlanmayan bir döngüye girebilir.

# 13. Fonksiyonlar

Fonksiyon, adlandırılmış ve tekrar kullanılabilen kod bloğudur:

```ozhex
fonksiyon topla(a: int, b: int) -> int {
    dön a + b;
}

fonksiyon ana() -> int {
    değişken sonuc: int = topla(20, 22);
    yaz(sonuc);
    dön 0;
}
```

Fonksiyon tasarlarken:
- Parametre türlerini açık yazın.
- Dönüş türünü doğru seçin.
- Bir fonksiyona tek ve anlaşılır bir sorumluluk verin.
- Girdilerin sınırlarını kontrol edin.
- Hata durumlarını çağıran koda açık biçimde yansıtın.

# 14. Yapılar ve enum

Yapılar birden fazla alanı tek bir veri modeli altında toplar:

```ozhex
yapı Nokta {
    x: int,
    y: int
}

fonksiyon ana() -> int {
    değişken p: Nokta = Nokta { x: 10, y: 20 };
    yaz(p.x);
    yaz(p.y);
    dön 0;
}
```

Enum, sınırlı seçenekleri adlandırır:

```ozhex
enum Durum {
    Bekliyor,
    Calisiyor,
    Tamamlandi
}
```

Yapı alanlarının sırasını ve türlerini değiştirirken programınızın bu veri modeline bağlı tüm bölümlerini gözden geçirin.

# 15. Diziler

Sabit uzunluklu dizi örneği:

```ozhex
fonksiyon ana() -> int {
    değişken sayilar: [int; 4] = [10, 20, 30, 40];
    yaz(sayilar[0]);
    yaz(sayilar[3]);
    dön 0;
}
```

Dizi indeksleri sıfırdan başlar. Dört elemanlı dizinin geçerli indeksleri 0, 1, 2 ve 3'tür. İndeksin sınır içinde kaldığını programcı olarak kontrol edin; otomatik sınır denetimi varsaymayın.

Çok boyutlu diziler ve bileşik türlerin bellekteki yerleşimi hakkında varsayım yapmadan önce kullandığınız sürümün örneklerini ve dil belgelerini kontrol edin.

# 16. String ve karakterler

String literal çift tırnakla yazılır:

```ozhex
fonksiyon ana() -> int {
    değişken mesaj: string = "OZHEX ile programlama";
    yaz(mesaj);
    dön 0;
}
```

UTF-8 metinlerde karakter sayısı ile bayt sayısı aynı olmak zorunda değildir. Çok baytlı karakterlerde byte indeksini karakter indeksi gibi kullanmayın. String işlemlerini yaparken kullandığınız API'nin uzunluk ve indeks tanımını dikkate alın.

# 17. Standart girdi/çıktı

Standart çıktı için belgelenmiş `yaz(...)` yerleşiği kullanılır:

```ozhex
fonksiyon ana() -> int {
    yaz("Toplam:");
    yaz(42);
    dön 0;
}
```

`yaz` için belgelenmiş destek, mevcut scalar ve string değerlerini kapsar. Burada gösterilmeyen bir biçimlendirme veya standart girdi API'sinin varlığını varsaymayın.

# 18. Kaynak modüllerini kullanma

Kaynak modülü içe aktarma yönergesi kendi satırında bulunur:

```ozhex
içe_aktar "net/socket.ozhex";
```

Import çözümleme sırası:
1. İçe aktaran dosyaya göreli yol.
2. Kurulu veya depo standart kütüphane kökü.
3. Proje paket yapılandırmasında tanımlanmış kurulu paket.

Import sistemi kaynak dosyalarını bağımlılık sırasına göre genişletir ve aynı modülü yinelenen biçimde eklememeye çalışır. Döngüsel import, üst dizine kaçış, sembolik bağlantı ve kaynak boyutu/modül sayısı sınırları reddedilir.

**Modül sistemi sınırı:** Mevcut katman düz kaynak ekleme modelidir. Namespace, alias, görünürlük denetimi veya modül başına sembol izolasyonu sağladığı varsayılmamalıdır.

# 19. Dosya işlemleri

Linux x86-64 standart kütüphanesinde `fs/file.ozhex` dosya modülü bulunur:

```ozhex
içe_aktar "fs/file.ozhex";
```

Belgelenen işlev ailesi:
- Dosyayı no-follow davranışıyla açma.
- Okuma ve yazma.
- Sınırlandırılmış read-all işlemi.
- Dosya boyutu ve stat bilgisi.
- Yolun varlığını ve türünü kontrol etme.
- Silme, dizin oluşturma ve yeniden adlandırma.
- fsync ve aynı dosya sistemi üzerinde atomik dosya değiştirme.

Bu API Linux x86-64'e özgüdür. Hatalar negatif Linux errno değerleri olarak döndürülür. Ebeveyn dizinlerdeki sembolik bağlantı geçişi sandbox edilmez. Güvenilmeyen yolları uygulamanızın denetlediği bir dizin içinde tutun.

# 20. TCP ağ programlama

Linux x86-64 için `net/socket.ozhex` modülü temel TCP işlemleri sunar:

```ozhex
içe_aktar "net/socket.ozhex";
```

Belgelenen kapsam:
- TCP socket oluşturma ve kapatma.
- Loopback adresine bind/listen/accept.
- IPv4 literal adrese connect.
- Geçici port numarasını öğrenme.
- Okuma, yazma ve tüm veriyi yazmayı deneyen write-all işlemi.
- Socket zaman aşımı ve SO_REUSEADDR.

Bu modül düşük seviyeli ve bloklayan socket işlemleridir. DNS çözümleme, TLS, asenkron I/O ve platformlar arası socket backend'i sağladığı varsayılmamalıdır. Gerçek ağ servislerini internete açmadan önce erişim denetimi, hata yönetimi ve kaynak sınırları ekleyin.

# 21. Paket yöneticisi

OZHEX paket yöneticisinin komutu `ozhexpm` şeklindedir. Komutları görmek için:

```bash
ozhexpm --help
```

Mevcut komut aileleri:

| Komut | Açıklama |
|---|---|
| `yeni` / `new` | Paket projesi oluşturur |
| `doğrula` / `dogrula` / `validate` | `ozhex.toml` dosyasını doğrular |
| `kilitle` / `lock` | Bağımlılıkları çözümler ve lock dosyası yazar |
| `kur` / `install` | Doğrulanmış bağımlılıkları kurar |
| `agac` / `tree` | Kilitlenmiş bağımlılık ağacını gösterir |
| `paketle` / `pack` | Deterministik kaynak arşivi oluşturur |
| `yayinla` / `publish` | Değiştirilemez paket sürümü yayımlar |

Alt komut parametreleri sürüme göre yardım çıktısından öğrenilmelidir:

```bash
ozhexpm yeni --help
ozhexpm doğrula --help
ozhexpm kilitle --help
ozhexpm kur --help
ozhexpm agac --help
ozhexpm paketle --help
ozhexpm yayinla --help
```

Yayımlama işlemi registry erişimi ve yetki gerektirebilir. Token veya erişim bilgilerinizi kaynak dosyalarına eklemeyin.

# 22. Derleme hedefleri ve platform seçimi

Kurulu compiler yardımında aşağıdaki hedef adları listelenir:

- `x86_64-linux`
- `x86_64-windows`
- `x86_64-macos`

Genel derleme biçimi:

```bash
ozhex [--target <hedef>] <girdi.ozhex> <çıktı>
```

Linux x86-64 varsayılan hedeftir. Diğer hedeflerin CLI'da görünmesi, o hedefler için tam runtime, standart kütüphane, native compiler dağıtımı veya üretim desteği bulunduğunu kanıtlamaz. Son kullanıcı olarak yalnızca Releases sayfasında yayımlanmış paketleri ve o paket için belgelenen hedefleri esas alın.

# 23. Hata ayıklama

## `ozhex: command not found`

```bash
ls -lah ~/.local/bin/ozhex
export PATH="$HOME/.local/bin:$PATH"
hash -r
command -v ozhex
```

Dosya yoksa kurulumun doğru Ubuntu kullanıcısına ve dağıtımına yapıldığını kontrol edin.

## `ozhexpm --version` alt komut hatası

Mevcut paket yöneticisi arayüzü `--version` seçeneği yerine alt komut bekler. Yardım için:

```bash
ozhexpm --help
```

## Kaynak dosyası bulunamıyor

```bash
pwd
ls -la
```

Dosya adını, uzantıyı ve büyük/küçük harfleri kontrol edin.

## Program çalıştırılamıyor

- Derleme çıktısının bulunduğu klasörde olduğunuzu kontrol edin.
- Linux'ta gerekiyorsa çalıştırma iznini kontrol edin: `chmod +x ./program`.
- Hedef dosya biçiminin çalıştırdığınız işletim sistemiyle eşleştiğini doğrulayın.
- WSL2 altında Linux çıktısını Ubuntu terminalinden çalıştırın.

# 24. Güvenli programlama ve yayın öncesi kontrol

- Yalnızca resmî Releases sayfasından paket indirin.
- Yayımlanmış checksum değerini doğrulayın.
- Güvenmediğiniz kaynak kodlarını derlemeyin.
- Kullanıcı girdilerini doğrulayın.
- Dizi indekslerini sınırlandırın.
- Dosya yollarında güvenilmeyen girdileri doğrudan kullanmayın.
- Ağ servislerinde zaman aşımı ve hata durumlarını ele alın.
- Gizli anahtarları kaynak koduna veya paket arşivine koymayın.
- Derleme başarısını uygulamanın doğruluğuyla karıştırmayın.
- Programınızı hedeflediğiniz gerçek ortamda çalıştırıp doğrulayın.

Checksum, indirilen dosyanın bütünlüğünü kontrol etmeye yarar; tek başına yayıncı kimliğini doğrulayan dijital imza değildir.

# 25. Uygulamalı projeler

Aşağıdaki çalışmalar, kitapta anlatılan özellikleri küçük uygulamalara dönüştürmek için hazırlanmıştır. Her proje için kaynak dosyasını oluşturun, derleyin ve programı çalıştırın.

## Proje 1 — Merhaba uygulaması

```ozhex
fonksiyon ana() -> int {
    yaz("İlk OZHEX uygulamam");
    dön 0;
}
```

## Proje 2 — Hesaplama

```ozhex
fonksiyon carp(a: int, b: int) -> int {
    dön a * b;
}

fonksiyon ana() -> int {
    değişken sonuc: int = carp(7, 6);
    yaz(sonuc);
    dön 0;
}
```

## Proje 3 — Koşul denetimi

```ozhex
fonksiyon ana() -> int {
    değişken sicaklik: int = 28;

    eğer sicaklik > 25 {
        yaz("Sıcak");
    } değilse {
        yaz("Normal");
    }

    dön 0;
}
```

## Proje 4 — Dizi toplamı

```ozhex
fonksiyon ana() -> int {
    değişken degerler: [int; 5] = [3, 5, 7, 9, 11];
    değişken toplam: int = 0;
    değişken i: int = 0;

    iken i < 5 {
        toplam += degerler[i];
        i += 1;
    }

    yaz(toplam);
    dön 0;
}
```

## Proje 5 — Veri modeli

```ozhex
yapı Kullanici {
    id: int,
    puan: int
}

fonksiyon ana() -> int {
    değişken kayit: Kullanici = Kullanici { id: 1, puan: 90 };
    yaz(kayit.id);
    yaz(kayit.puan);
    dön 0;
}
```

## Proje 6 — Modül kullanan uygulama

Linux x86-64 üzerinde dosya veya socket modülünü içe aktaran bir program oluşturun. Modülün belgelenmiş API'sini ve platform sınırlamalarını incelemeden işlev adı uydurmayın.

# 26. Hızlı komut başvurusu

| İşlem | Komut |
|---|---|
| Sürümü göster | `ozhex --version` |
| Derleyici yardımı | `ozhex --help` |
| Kaynak derle | `ozhex main.ozhex program` |
| Programı çalıştır | `./program` |
| Paket yöneticisi yardımı | `ozhexpm --help` |
| Paket projesi yardımı | `ozhexpm yeni --help` |
| Bağımlılık doğrulama yardımı | `ozhexpm doğrula --help` |
| Kurulum yardımı | `ozhexpm kur --help` |

# 27. Sık sorulan sorular

### Kaynak kod deposunu indirmem gerekiyor mu?

Hayır. Son kullanıcı paketlerini [OZHEX-Distributions Releases](https://github.com/ismailozdemir01/OZHEX-Distributions/dist) sayfasından indirin.

### Windows'ta hangi terminali kullanmalıyım?

WSL2 dağıtımıyla kurulum yaptıysanız Ubuntu terminalini kullanın. WSL2 paketi native Windows compiler değildir.

### `ozhexpm --version` neden çalışmıyor?

Mevcut CLI alt komut bekler. Komut listesi için `ozhexpm --help` kullanın.

### macOS, ARM64 veya native Windows desteği hazır mı?

CLI'da hedef adının bulunması, yayımlanmış son kullanıcı paketi bulunduğu anlamına gelmez. Destek durumunu Releases sayfasından kontrol edin.

### Derleme başarılı olunca program tamamlanmış sayılır mı?

Hayır. Derleme yalnızca kaynak kodunun derlenebildiğini gösterir. Uygulamanın beklenen işi yaptığını gerçek kullanım senaryolarında kontrol etmeniz gerekir.

# 28. Terimler sözlüğü

| Terim | Açıklama |
|---|---|
| Kaynak kod | Programcının yazdığı `.ozhex` dosyası |
| Derleyici | Kaynak kodu hedef program biçimine dönüştüren araç |
| Native program | İşletim sisteminin doğrudan çalıştırdığı hedef dosya |
| WSL2 | Windows üzerinde Linux ortamı çalıştırma altyapısı |
| Ubuntu | Bu dağıtımda Windows kullanıcıları için kullanılan Linux ortamı |
| Standart kütüphane | Dil ile birlikte sunulan modül ve runtime API'leri |
| Modül | Başka kaynak dosyaya içe aktarılan OZHEX kaynak birimi |
| Paket | Dağıtılabilir OZHEX proje arşivi |
| Registry | Paketlerin yayımlandığı ve alındığı servis |
| Checksum | Dosya bütünlüğünü denetlemek için kullanılan özet değer |

---

## Resmî bağlantılar

- **İndirme ve sürümler:** https://github.com/ismailozdemir01/OZHEX-Distributions/dist
- **Dağıtım deposu:** https://github.com/ismailozdemir01/OZHEX-Distributions
- **Kurulum adımları:** [KURULUM.md](KURULUM.md)
- **Son kullanıcı kılavuzu:** [KULLANICI-KILAVUZU.md](KULLANICI-KILAVUZU.md)
- **Geliştirici/yönetici kaynak deposu:** https://github.com/ismailozdemir01/

**Doküman kapsamı:** Son kullanıcı eğitimi. Compiler geliştirme, kaynak koddan release üretme, backend/ABI, self-host bootstrap ve dahili proje yönetimi bu kitabın kapsamı dışındadır.
