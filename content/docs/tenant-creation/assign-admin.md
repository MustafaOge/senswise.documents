---
title: 2 - Yeni Tenant'a Admin Atanması
type: docs
prev: docs/tenant-creation/tenant-definition
next: docs/tenant-creation/data-migration

sidebar:
  open: true
---




Yeni oluşturulan tenant için bir admin kullanıcı atamak gerekmektedir. Bunu yapmak için aşağıdaki adımları izleyebilirsiniz:

#### 2.1 Kullanıcı Oluşturma:
   - İlgili sunucunun kayıt olma adresinden (örneğin, [https://dev.senswise.co/sign-up](https://dev.senswise.co/sign-up)) bir **FREE_USER** hesabı oluşturun.
   - Bu kullanıcının daha sonra admin olarak atanabilmesi için gerekli işlemleri yapacağız.

#### 2.2 Kullanıcı Bilgilerinin Güncellenmesi:
   - Örneğin, admin olacak kullanıcının e-posta adresi `new_tenant@senswise.co` olsun.
   - **SENSWISE** veritabanındaki **USER** şemasının **USER** tablosunda bu kullanıcıyı bulmak için aşağıdaki sorguyu kullanabilirsiniz:

     ```sql
     SELECT * FROM "USER"."USER" u WHERE u."EMAIL" LIKE '%new_tenant@senswise.co%'
     ```

   - Bulduğunuz kullanıcının `ID` numarasını alın. Örneğin, `ID` numarası `12345` olsun.
   - Eğer bu kullanıcının aynı zamanda bir lider olması gerekiyorsa, **IS_MANAGER** alanını da `true` olarak güncelleyin.

#### 2.3 Kullanıcı Yetkilerinin Tanımlanması:

- Kullanıcının `ID` değerini **USER** şemasındaki **USER_ROLE** tablosunda sorgulayarak, ilgili kaydın **ROLE_ID** değerini `4` (**ENDUSER**) yerine `2` (**ADMIN**) olarak güncelleyiniz.

- Admin kullanıcısının durumunu güncellemek için aşağıdaki sorguyu kullanın:

     ```sql
     UPDATE "USER"."USER_ROLE"
     SET "ROLE_ID"=2
     WHERE "USER_ID"=12345;
     ```
-    Burada **ROLE_ID** ile yetkiler belirlenir:
     - `1` - SUPERADMIN
     - `2` - ADMIN
     - `3` - MANAGER
     - `4` - ENDUSER

   - Yeni tenant için sadece `2` (ADMIN) veya hem `2` hem de `3` (MANAGER) atanabilir. **SUPERADMIN** yetkisi sadece SENSWISE çalışanları için kullanılmalıdır, müşteriler bu yetkiye sahip olmamalıdır.

#### 2.4 Tenant Ataması ve Kullanıcı Tipinin Güncellenmesi:
   - Admin kullanıcısını yeni tenant'a atamak ve kullanıcı tipini güncellemek için aşağıdaki sorguyu kullanabilirsiniz:

     ```sql
     UPDATE "USER"."USER_TENANT"
     SET "TENANT_CODE"='NEW_TENANT', "USER_TYPE_ID"=4
     WHERE "USER_ID"=12345;
     ```

   - Burada `USER_TYPE_ID` `4` olarak ayarlandığı için kullanıcı **PRO** üye olacaktır.

#### 2.5 Sonuç:
   - Yeni tenant için bir admin kullanıcısı oluşturulmuş oldu. Bu kullanıcı artık sisteme admin olarak giriş yapabilir, ancak diğer veriler eklenene kadar sınırlı yetkilerle çalışacaktır. 😊
