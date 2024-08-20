---
title: 4 - Template Bilgilerinin Yüklenmesi
type: docs
prev: docs/tenant-creation/data-migration
sidebar:
  open: true
---


Bu bölümde, Tenant ve anket (survey) bilgilerini içeren Excel Template bilgilerini sisteme yükleyeceğiz. İşlemler, ortamınıza göre farklılık gösterebilir:

- **Dev ve Demo Ortamları**: Postman üzerinden doğrudan yapabilirsiniz.
- **Production Ortamı**: Projeyi local ortamda çalıştırarak, Postman'den localdeki projeye istek atmanız gerekecek.

 **Not:** Herhangi bir ortamda anket yüklerken, işlem zaman aşımına uğrarsa, yükleme işlemi projeyi yerel ortamda çalıştırarak gerçekleştirilmelidir. Detaylar için [4.6 Projeyi Yerel Ortamda Çalıştırma](#46-projeyi-yerel-ortamda-%c3%a7al%c4%b1%c5%9ft%c4%b1rma) bölümüne bakabilirsiniz.



### Adımlar:

### 4.1 Postman Koleksiyonunu Hazırlama
   - İlk olarak,`Sense.postman_collection` `Tenant Excel dosyaları`  dosyalarını bilgisayarınıza indirin.
   - Ardından, Postman koleksiyonunu Postman uygulamasına aktarın.

### 4.2 Ortam ve Tenant Kodu Seçimi
   - Postman'de `Sense` üzerine gelin, `url` kısmından uygun ortamı seçin ve anketleri ekleyeceğiniz tenant kodunu girin.

   Örneğin:

   | Variable     |  Current Value |
   | -------------- |------------- | 
   | `url`          | `https://survey-api.dev.senswise.co`    |
   | `tenantCode`   | `TECH_TEST`    |

### 4.3 Anket Template Yüklenmesi

####  SCALES TEMPLATE YÜKLENMESİ

Skala bilgisi değişen tenantlar için diğer anketilerin tekrar yüklenmesi gerekmektedir.Bu aşamada Tenant ile ilgili Scales template yüklemiz gerekir. Postman Sense>TemplateUploads>SurveyOptionUpload  isteğine girin. 
   - **SurveyOptionUpload** işlemi için, tenant açma dosyalarındaki `Scales.xlsx` dosyasını Postman'deki `Body` alanına ekleyin ve `Send` butonuna tıklayın.

   Örneğin:

   | Key     |   Value |
   | -------------- |------------- | 
   | `ScaleFile`          | `Scales_2024_05_30.xlsx`    |

   > **Not:** Anketlerin ve bağımlılıklarının yüklenmesinin takibini kolaylaştırmak ve karışıklığı önlemek için nelerin yükleneceği liste halinde yazabilir ve yüklediklerinizi işaretleyebilirsiniz.

### 4.4 Survey Template ve Dependent Upload İşlemi
Bu aşamada anketimizi ve eğer anketin bağımlı soruları varsa anketin bağımlılığını yükleyeceğiz. Sense>TemplateUploads> SurveyUpload isteğine girin.
   - **SurveyUpload** açtıktan sonra `SurveyHeaders` alanına yükleyeceğiniz anketin `Header` dosyasını, `Surveys` alanına ise anketin `Template` dosyalarını ekleyin, ardından `Send` butonuna tıklayın. Geriye dönen Id değeri  anketin bağımlılığı varsa sonraki aşamada kullanılacaktır.

   Örneğin:

   | Key              | Value                            |
   |------------------|----------------------------------|
   | `SurveyHeaders`  | `CA_Header_2024_05_30.xlsx`      |
   | `Surveys`        | `CA_Template_2024_05_30.xlsx`    |

#### Bağımlı Soruların Yüklenmesi
   - `Send` işleminden sonra dönen `SurveyHeaderId` kodunu alın. Ardından, Sense>TemplateUploads> `surveyDependentQuestion` kısmına gelerek, `SurveyHeaderId` alanına bu kodu girin ve `QuestionFile` alanına anketin bağımlılığı olan dosyasını ekleyin, ardından `Send` butonuna tıklayın.

   > **Not:** Her anketin bağımlı soruları olmayabilir.

   Örneğin:

   | Key              | Value                            |
   |------------------|----------------------------------|
   | `QuestionFile`   | `CA_Dependant_2024_05_30.xlsx`   |
   | `SurveyHeaderId` | `408`                            | 

   ![Alt Text](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExYmtyb2FhODR2aHo3aWVlYWo2bHE4NDYyb2d5bGU5eXZ1OWRkOHIydSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/y5Envp1XPyI8DFM1Uy/giphy.gif)


   

### 4.5 Diğer Survey Template'lerinin ve Dependent Dosyalarının Yüklenmesi

Aşağıdaki listede belirtilen diğer anket şablonlarını ve bağımlı dosyalarını yüklemek için yukarıdaki adımları tekrarlayın:

- **SurveyUpload** kısmına, `SurveyHeaders` ve `Surveys` dosyalarını ekleyin ve `Send` butonuna tıklayın. Daha sonra, bağımlı sorular için `surveyDependentQuestion` kısmına, `SurveyHeaderId` kodunu ve ilgili `QuestionFile` dosyasını ekleyin.

| Survey          | SurveyHeader                 | SurveyTemplate                  | DependentQuestion              |
|-----------------|------------------------------|---------------------------------|--------------------------------|
| CA              | `CA_Header_2024_05_30.xlsx`  | `CA_Template_2024_05_30.xlsx`   | `CA_Dependant_2024_05_30.xlsx` |
| CA Dependent    | `CA_Dependant_2024_05_30.xlsx`| N/A                             | N/A                            |
| DEI             | `DEI_Header_2024_05_30.xlsx` | `DEI_Template_2024_05_30.xlsx`  | `DEI_Dependant_2024_05_30.xlsx`|
| DEI Dependent   | `DEI_Dependant_2024_05_30.xlsx`| N/A                             | N/A                            |
| ENPS            | `ENPS_Header_2024_05_30.xlsx`| `ENPS_Template_2024_05_30.xlsx` | `ENPS_Dependant_2024_05_30.xlsx`|
| ENPS Dependant  | `ENPS_Dependant_2024_05_30.xlsx`| N/A                             | N/A                            |
| OLX             | `OLX_Header_2024_05_30.xlsx` | `OLX_Template_2024_05_30.xlsx`  | `OLX_Dependant_2024_05_30.xlsx`|
| OLX Dependant   | `OLX_Dependant_2024_05_30.xlsx`| N/A                             | N/A                            |
| RTN             | `RTN_Header_2024_05_30.xlsx` | `RTN_Template_2024_05_30.xlsx`  | `RTN_Dependant_2024_05_30.xlsx`|
| RTN Dependant   | `RTN_Dependant_2024_05_30.xlsx`| N/A                             | N/A                            |
| LCA             | `LCA_Header_2024_05_30.xlsx` | `LCA_Template_2024_05_30.xlsx`  | N/A                            |
| OHC             | `OHC_Header_2024_05_30.xlsx` | `OHC_Template_2024_05_30.xlsx`  | N/A                            |
| SC              | `SC_Header_2024_05_30.xlsx`  | `SC_Template_2024_05_30.xlsx`   | N/A                            |
| UPN             | `UPN_Header_2024_05_30.xlsx` | `UPN_Template_2024_05_30.xlsx`  | N/A                            |
| WB              | `WB_Header_2024_05_30.xlsx`  | `WB_Template_2024_05_30.xlsx`   | N/A                            |

### 4.6 Projeyi Yerel Ortamda Çalıştırma

Anketleri yerel ortamda çalıştırılan projeyle yüklemek için `Sense.Survey.Api`, `Sense.API.Gateway` ve bazı Docker tabanlı bağımlılıkların çalışması gerekmektedir.


#### 1. Sense.API.Gateway Projesini Çalıştırma
- `Sense.API.Gateway` projesini doğrudan çalıştırabilirsiniz.

#### 2. Sense.Survey.Api Yapılandırması
- `Sense.Survey.Api` için sunucuda kullanılan Redis bağlantı dizesi ile yerel ortamda kullanılacak bağlantı adresi farklı olacaktır. Bu nedenle, yerel ortamda Redis bağlantısını kullanabilmek için `AppSettings.json` dosyasında gerekli güncellemeleri yapın. İlgili bilgiler için Mono Ortam Erişim Bilgilerine göz atabilirsiniz.


#### 3. Docker Servislerini Başlatma
- Diğer gerekli servisleri yerel ortamınızda başlatmak için Docker Desktop uygulamasını açtıktan sonra `docker-compose.yml` dosyasını kullanın.
- docker-compose.yml dosyasının bulunduğu dizinde aşağıdaki komutu terminalde çalıştırarak servisleri başlatabilirsiniz:

 ```   bash
    docker-compose -f docker-compose.yml up
 ```   

#### 4. Postman Üzerinden İstek Gönderme
- Projeler başarılı bir şekilde çalışmaya başladıktan sonra, Postman üzerinden isteğinizi Gateway’e gönderebilirsiniz.
- Bunun için Postman'de Sense üzerine geldikten sonra yapacağınız isteğin `Variables` alanında `current value` değerini sunucu yerine `localhost:4000` olarak değiştirin.


### 4.7 Template Bilgilerinin Güncellenmesi

- Bu aşamada, mevcut `Action`, `Labels` ve `Insight`   template bilgilerinde sonradan bir güncelleme yapılması durumunda, bu güncellenen template'lerin nasıl yükleneceği gösterilir.

> **Not:** Yeni bir tenant oluşturulduğunda bu template bilgilerini yüklemek zorunlu değildir.

####  ACTION TEMPLATE YÜKLENMESİ

Bazı anketler için Action Template bulunmaktadır. Bu template'i ilgili tenant'a yüklemek için, Postman'de bulunan `actionUpload` isteğine girin ve body alanında `ActionTemplateFile` anahtarına ilgili anketin Insight dosyasını yükleyin.

Örneğin:

| Key                   | Value                        |
|-----------------------|------------------------------|
| `ActionTemplateFile` | `SC_Action_Template_2024_05_30.xlsx`   |

####  LABELS TEMPLATE YÜKLENMESİ

Labels template dosyasında değişiklik olması durumunda yeni dosya yüklenmelidir. Labels template yüklemek için, Postman'de bulunan `uploadMultiLanguageKeyUpload` isteğine girin ve body alanında `MultilanguageUploadFile` anahtarına ilgili Labels dosyasını yükleyin.

Örneğin:

| Key                   | Value                        |
|-----------------------|------------------------------|
| `MultilanguageUploadFile` | `Labels-05-07-2024.xlsx`   |


####  INSIGHT  TEMPLATE YÜKLENMESİ

Insight template üzerinde bir değişiklik yapıldığında, yeni dosya yüklenmelidir. Bunun için Postman'de bulunan insightUpload isteğini açın ve body alanında, InsightTemplateFile anahtarına değişiklik yapılan Insight template dosyasını yükleyin.

Örneğin:

| Key                   | Value                        |
|-----------------------|------------------------------|
| `InsightTemplateFile` | `LCA_Insight_Template 231215.xlsx`   |



### 4.8 Son Kontroller
   - Tüm anketler için bu adımları tamamladıktan sonra, sisteme giriş yaparak anketlerin düzgün bir şekilde yüklendiğini kontrol edin.