# OpenCTI Project

---

## مقدمة وفهم المشروع

OpenCTI (Open Cyber Threat Intelligence) هو نظام مفتوح المصدر لإدارة وتبادل معلومات التهديدات السيبرانية.  
يهدف المشروع إلى توحيد البيانات من مصادر متعددة، مع توفير أدوات لتحليلها ومشاركتها بطريقة منظمة وآمنة.

---

## هيكلة المجلدات والملفات
├── nawafOpenCTI
│ └── opencti
│ ├── docker-compose.dev.yml
│ ├── docker-compose.opensearch.yml
│ ├── docker-compose.yml
│ ├── rabbitmq.conf
│ ├── README.md
│ └── renovate.json



### شرح الهيكلة:

- **nawafOpenCTI/opencti/**  
  مجلد المشروع الرئيسي يحتوي على ملفات التهيئة والوثائق الخاصة بـ OpenCTI.

---

## شرح الملفات الرئيسية في المجلد

### 1. `docker-compose.yml`  
ملف تكوين الخدمات الأساسية لتشغيل OpenCTI باستخدام Docker. يحتوي على إعدادات الحاويات:  
- OpenCTI platform  
- قاعدة بيانات PostgreSQL  
- RabbitMQ (نظام الرسائل)  
- Elasticsearch أو OpenSearch (محرك البحث)

---

### 2. `docker-compose.dev.yml`  
نسخة مخصصة لبيئة التطوير، تضيف أدوات مثل إعادة التحميل التلقائي للمطورين لتسهيل العمل.

---

### 3. `docker-compose.opensearch.yml`  
تكوين بديل يستخدم OpenSearch بدلاً من Elasticsearch كمحرك بحث وتحليل.

---

### 4. `rabbitmq.conf`  
ملف إعدادات خاص بـ RabbitMQ، يتحكم في الرسائل بين الخدمات المختلفة، مثل الكونيكتورز و OpenCTI.

---

### 5. `README.md`  
ملف التوثيق الرئيسي للمشروع يحتوي شرحًا كاملاً عن المشروع، خطوات التثبيت والاستخدام.

---

### 6. `renovate.json`  
إعدادات أداة Renovate لتحديث التبعيات البرمجية تلقائيًا.

---

## الكونيكتورز (Connectors) المستخدمة وفوائدها

1. **AlienVault OTX (Open Threat Exchange)**  
   - **النوع:** External Import  
   - **الفائدة:** يجلب حزم التهديدات (Pulses) وعناوين IP خبيثة، بصمات ملفات، عائلات البرمجيات الخبيثة من منصة AlienVault.  
   - **المصدر:** [otx.alienvault.com](https://otx.alienvault.com)  
   - **مطلوب API Key:** نعم  
   - **تردد التحديث:** كل 30 دقيقة

2. **MalwareBazaar**  
   - **النوع:** External Import  
   - **الفائدة:** يستورد عينات البرمجيات الخبيثة مع بيانات مثل البصمات، نوع الملف، التواريخ، والعائلات.  
   - **المصدر:** [bazaar.abuse.ch](https://bazaar.abuse.ch)  
   - **مطلوب API Key:** نعم  
   - **تردد التحديث:** كل 5 دقائق

3. **URLhaus**  
   - **النوع:** External Import  
   - **الفائدة:** يجمع عناوين URL الخبيثة المستخدمة لنشر البرمجيات الضارة.  
   - **المصدر:** [urlhaus.abuse.ch](https://urlhaus.abuse.ch)  
   - **مطلوب API Key:** لا  
   - **تردد التحديث:** كل ساعة

4. **CrowdSec**  
   - **النوع:** Internal Enrichment  
   - **الفائدة:** يثري عناوين IP بمعلومات السمعة والتهديدات وتقنيات MITRE ATT&CK والثغرات المرتبطة.  
   - **المصدر:** [crowdsec.net](https://crowdsec.net)  
   - **مطلوب API Key:** نعم  
   - **تردد التحديث:** فوري عند إضافة IP جديد

5. **CVE (Common Vulnerabilities and Exposures)**  
   - **النوع:** External Import  
   - **الفائدة:** يجلب الثغرات الأمنية من قاعدة بيانات NVD مع تفاصيل مثل رقم CVE، شدة الثغرة، الروابط، والتقييمات.  
   - **المصدر:** [nvd.nist.gov](https://nvd.nist.gov)  
   - **مطلوب API Key:** لا  
   - **تردد التحديث:** كل 24 ساعة

6. **MITRE ATT&CK**  
   - **النوع:** External Import  
   - **الفائدة:** يستورد بيانات إطار عمل MITRE ATT&CK حول تقنيات وتكتيكات الهجمات السيبرانية.  
   - **المصدر:** [attack.mitre.org](https://attack.mitre.org)  
   - **مطلوب API Key:** لا  
   - **تردد التحديث:** كل 7 أيام

7. **OpenCTI Datasets**  
   - **النوع:** External Import  
   - **الفائدة:** يجلب مجموعات بيانات مثل قطاعات السوق، المواقع الجغرافية، والشركات.  
   - **المصدر:** GitHub - OpenCTI-Platform/datasets  
   - **مطلوب API Key:** لا  
   - **تردد التحديث:** كل 7 أيام

8. **Internal Export Connectors**  
   - **النوع:** Internal Export  
   - **الفائدة:** تصدير بيانات التهديدات إلى صيغ مثل STIX 2.1، CSV، TXT للمشاركة أو التحليل الخارجي.

9. **Internal Import Connectors**  
   - **النوع:** Internal Import  
   - **الفائدة:** استيراد معلومات التهديدات من ملفات رفع يدوية بصيغ متعددة (STIX, YARA, تقارير...).

10. **Worker**  
    - **النوع:** خدمة داخلية  
    - **الفائدة:** تعالج الرسائل من RabbitMQ وتدير تنفيذ المهام والكونيكتورز.

---

## جدول ملخص سريع للكونيكتورز

| الكونيكتور               | النوع           | الفائدة الرئيسية                  | API Key مطلوب؟ |
|-------------------------|-----------------|---------------------------------|----------------|
| AlienVault OTX          | External Import | استيراد تهديدات وحزم APT         | نعم            |
| MalwareBazaar           | External Import | عينات برمجيات خبيثة              | نعم            |
| URLhaus                 | External Import | عناوين URL خبيثة                 | لا             |
| CrowdSec                | Internal Enrichment | إثراء عناوين IP بالسمعة والتقنيات | نعم            |
| CVE                     | External Import | ثغرات أمنية                      | لا             |
| MITRE ATT&CK            | External Import | تقنيات وتكتيكات الهجوم          | لا             |
| OpenCTI Datasets        | External Import | بيانات القطاعات والمواقع         | لا             |
| Internal Export Connectors | Internal Export | تصدير البيانات                  | لا             |
| Internal Import Connectors | Internal Import | استيراد بيانات من ملفات         | لا             |
| Worker                  | خدمة داخلية    | معالجة المهام والتكامل           | لا             |

---

#### أوامر تشغيل الكونتينرز والكونيكتورز باستخدام Docker

| الأمر                             | الوصف                                                   |
|-----------------------------------|---------------------------------------------------------|
| `docker-compose up -d`             | تشغيل جميع الخدمات والكونتينرز في الخلفية (Detached mode).  |
| `docker-compose ps`                | عرض حالة الكونتينرز العاملة حالياً مع تفاصيلها.           |
| `docker-compose logs -f`           | متابعة سجلات جميع الكونتينرز في الوقت الفعلي.              |
| `docker-compose logs -f <container>` | متابعة سجلات كونتينر محدد بالاسم.                      |
| `docker-compose stop`              | إيقاف تشغيل الكونتينرز بدون حذفها (توقف مؤقت).             |
| `docker-compose down`              | إيقاف وإزالة جميع الكونتينرز، الشبكات، والحجوم المرتبطة.    |
| `docker-compose restart`           | إعادة تشغيل جميع الكونتينرز.                              |
| `docker-compose exec <container> bash` | الدخول إلى داخل كونتينر محدد للوصول إلى الصدفة (Shell).  |
| `docker-compose build`             | بناء أو إعادة بناء الصور (images) المحددة في ملف التكوين.  |

---

### ملاحظات

- استخدم `docker-compose logs -f` لمراقبة نشاط الكونتينرز والكونيكتورز خصوصاً عند تشغيلهم لأول مرة.
- عند الحاجة لتشغيل كونتينر معين أو التحقق من حالته يمكن استخدام أوامر `docker-compose ps` و`docker-compose exec`.
- تأكد من تحديث ملفات التكوين (مثل docker-compose.yml) قبل إعادة البناء أو التشغيل.

---

هيكلة المجلدات والملفات
nawafOpenCTI/
└── opencti/
    ├── docker-compose.dev.yml       # تكوين بيئة التطوير (Dev environment)
    ├── docker-compose.opensearch.yml # تكوين محرك البحث OpenSearch
    ├── docker-compose.yml           # تكوين الخدمات الأساسية (Database, RabbitMQ, Workers)
    ├── rabbitmq.conf                # إعدادات خدمة RabbitMQ
    ├── README.md                     # ملف التوثيق
    └── renovate.json                 # إعدادات تحديث التبعيات (Dependency updates)
شرح الملفات
docker-compose.dev.yml:
يستخدم لتشغيل OpenCTI في بيئة التطوير مع إعدادات مخصصة لتسهيل الاختبار والتطوير.
docker-compose.opensearch.yml:
يحتوي على تكوينات OpenSearch، محرك البحث المسؤول عن تخزين واسترجاع البيانات بكفاءة.
docker-compose.yml:
الملف الرئيسي لتشغيل الخدمات الأساسية مثل:
قاعدة البيانات (PostgreSQL)
وسيط الرسائل (RabbitMQ)
خدمة Worker لمعالجة الكونتيكتورز
OpenCTI API و Frontend
rabbitmq.conf:
يحتوي على إعدادات RabbitMQ مثل المستخدمين، الصلاحيات، وعدد الطوابير queues.
README.md:
الملف الذي يوثق المشروع، يشرح الكونتيكتورز، الأوامر، الهيكلة، وفوائد كل خدمة.
renovate.json:
ملف إعدادات أداة Renovate لتحديث التبعيات تلقائياً في المشروع.
