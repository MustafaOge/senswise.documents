---
title: 3 - Tenant Verilerinin Yeni Tenant'a Eklenmesi
type: docs
prev: docs/tenant-creation/assign-admin
next: docs/tenant-creation/template-setup

sidebar:
  open: true
---

Bu işlemler, iki farklı proje kullanılarak gerçekleştirilir:

- **3.1** için [SenseCorSeeds](https://github.com/Senswiseco/SenseCorSeeds) projesi kullanılır.
- **3.2** için **SenswiseNotificationUI** projesinin [newmdsystem](https://github.com/Senswiseco/SenswiseNotificationUI/tree/newmdsystem) branch'i kullanılır.

Bu projeleri çalıştırabilmek için öncelikle **bun** paket yöneticisini kurmanız gerekmektedir. **Bun**'u yüklemek için aşağıdaki bağlantıyı kullanabilirsiniz:

[https://bun.sh/docs/installation](https://bun.sh/docs/installation)

Ayrıca, **Visual Studio Code** için "Bun for Visual Studio Code" eklentisini de ekleyerek daha kolay bir geliştirme ortamı sağlayabilirsiniz.


### 3.1 Yeni Tenant için Parametrelerin Eklenmesi

1. Proje açıldıktan sonra, **.env.sample** dosyasını kopyalayarak **.env** dosyası oluşturun.
2. Hangi ortama bağlanacaksanız, o ortamın veri tabanı bağlantısını **.env** dosyasında etkinleştirin (yorum satırından çıkarın).
3. `seeds` klasörü altında, `parameter-group` içinde bulunan `data.ts` dosyasını açın.
4. `PREDEFINED_SCALE` objesi içindeki `output` alanlarını gözden geçirin:
   - Her bir `TENANT_CODE` alanına uygun tenant kodunu girin.
   - `value` değerlerinin `0` olduğundan emin olun.
   - Örnek olarak, aşağıdaki kodu kullanabilirsiniz:

     ```typescript
     output = overwriteWith(output, "TENANT_CODE", "Scale2", "0");
     ```

   - Bu işlem tenant kodları ve değerlerinin doğru bir şekilde ayarlandığını sağlar.

   > **Not:** Bu işlem bazen cache nedeniyle Scale durumunu `1` olarak bırakabilir, bu yüzden anket şablonları yüklenmeyebilir. Eğer böyle bir durumla karşılaşırsanız, cache'in temizlenmesini beklemeniz gerekebilir.

5. Terminalde, projedeki bağımlılıkları yüklemek için `bun install` komutunu çalıştırın.
6. Yeni tenant parametrelerini veritabanına yüklemek için `bun run index.ts` komutunu çalıştırın.
   - Yüklenen verileri **CORE.PARAMETER** tablosunda görebilirsiniz.

   > **Uyarı:** Bu işlem dev ve demo ortamlarında genellikle sorunsuz çalışırken, prod ortamında sorun çıkarsa birkaç kez denemeniz gerekebilir.


### 3.2 Yeni Tenant için E-posta Template Yüklenmesi

1. **SenswiseNotificationUI** projesinin **newmdsystem** branch'ine geçin.
2. Proje dosyalarında bulunan **NotificationDefinitions.xlsx** dosyasını açın ve şu adımları izleyin:
   - Yeni eklediğiniz tenant kodunu **TENANT** sütununun altına girin.
   - Excel dosyasındaki herhangi bir tabloyu seçin ve sağ tıklayarak "Taşı ve Kopyala" seçeneğiyle bir kopyasını oluşturun.
   - Oluşturduğunuz kopyanın ismini yeni tenant ismiyle değiştirin.
3. **.env** dosyasında yükleme yapacağınız veritabanı bağlantı ayarlarını yapılandırın. Bağlantı ayarlarında **NOTIFICATION?schema=DEF** olmasına dikkat edin.
4. Terminalde, eksik paketleri indirmek için `bun install` komutunu çalıştırın.
5. Son olarak, **NotificationDefinitions.xlsx** dosyasındaki değişiklikleri veritabanına yüklemek için `bun run .\scripts\executeExcel.ts` komutunu çalıştırın.

6. Yapılan değişiklikleri **GitHub** deposuna commit ile gönderin. Çünkü sonraki çalıştırılmalarda yeni eklenen tenant bilgilerinin excel dosyasında bulunması gerekir.
