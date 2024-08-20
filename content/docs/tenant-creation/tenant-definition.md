---
title:  1 - Tenant Tanımlanması
type: docs
prev: docs/folder/
next: docs/tenant-creation/assign-admin

sidebar:
  open: true
---

Yeni bir tenant oluşturulurken, bu tenant’a ait bir kod belirlenir. Bu kod en fazla 50 karakter uzunluğunda olabilir.

Bu kod, veritabanındaki SENSWISE veritabanının COR şemasına ait TENANT tablosuna eklenir. Tenant'ı oluşturmak istediğiniz ortamın veritabanına bağlandıktan sonra aşağıdaki SQL sorgusuyla NEW_TENANT kodunu ekleyebilirsiniz:

```sql
INSERT INTO "COR"."TENANT" 
  ("CODE", "NAME", "DEFAULT_LANGUAGE", "CREATED_DATE", "MODIFIED_DATE", "IS_DELETED", "USER_TYPE_ID")
VALUES
  ('NEW_TENANT', 'New Tenant', 'tr-TR', '2024-05-21 00:00:00.000', NULL, false, 4);
```

> **Not:** Tenant kodunun **SCREAMING_SNAKE_CASE** formatında (yani büyük harflerle ve alt tire ile birleştirilmiş) olması standartlık sağlayacaktır, ancak bu zorunlu değildir.

**DEFAULT_LANGUAGE** ve **USER_TYPE_ID** alanlarının, yeni oluşturulacak tenant için uygun şekilde ayarlanması gerekir.

- **DEFAULT_LANGUAGE**: Tenant’ın varsayılan dilini belirtir.
- **USER_TYPE_ID**: Tenant’ın üyelik tipini belirler. Kullanılabilecek değerler şunlardır:
  1. FREE
  2. CLASSIC
  3. ANALYTICS
  4. PRO

Örneğin, **USER_TYPE_ID** değerini 4 olarak belirlersek, bu tenant PRO üyelik tipinde olacaktır.
    