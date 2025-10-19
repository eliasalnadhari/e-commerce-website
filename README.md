متجر إلكتروني كامل (PHP + MySQL) بواجهتين:

واجهة المستخدم (Storefront): تصفّح المنتجات، عرض سريع (Quick View)، بحث وفلاتر، Wishlist، Cart، Checkout، تتبّع الطلبات، صفحة تواصل.

لوحة تحكّم الإدمن (Admin Panel): إدارة المنتجات، الطلبات، المستخدمين، الرسائل، ولوحة مؤشرات (Dashboard).

التقنية والهيكل

Backend: PHP (PDO)

Database: MySQL (ملف سكريبت جاهز: shop_db.sql)

Auth & Sessions: جلسات PHP للمستخدم والإدمن

Mail: مدمج PHPMailer عبر Composer (composer.json)

reCAPTCHA: تحقق روبوت في تسجيل دخول المستخدم (user_login.php)

Front-end: HTML/CSS/JS (ملفات css/style.css و js/script.js)

المسار الأساسي للمشروع: 111/11/

أهم الصفحات (Storefront)

home.php الرئيسية وعروض المنتجات.

shop.php كل المنتجات + تصنيفات.

category.php تصفية حسب الفئات.

search_page.php البحث.

quick_view.php عرض سريع للمنتج.

wishlist.php قائمة الرغبات.

cart.php السلة.

checkout.php إكمال الطلب.

orders.php عرض الطلبات الخاصة بالمستخدم.

contact.php تواصل وإرسال رسالة.

user_register.php و user_login.php تسجيل/دخول.

reset.php و active.php استعادة كلمة السر وتفعيل الحساب عبر كود أمان.

لوحة الإدمن (Admin)

المجلد: admin/

dashboard.php لوحة المؤشرات.

products.php إدارة المنتجات (إضافة/تعديل/حذف) + update_product.php.

placed_orders.php إدارة الطلبات.

users_accounts.php إدارة المستخدمين.

messages.php قراءة رسائل نموذج التواصل.

admin_login.php, register_admin.php, update_profile.php.

قاعدة البيانات (الجداول الأساسية)

(من shop_db.sql)

admins — حسابات الإدمن.

users — حسابات المستخدمين (فيها حقل Activated + security_code للتفعيل).

products — المنتجات (الاسم، الوصف، السعر، 3 صور).

cart — عناصر السلة لكل مستخدم.

wishlist — قائمة الرغبات.

orders — الطلبات (بيانات الشحن/الدفع والحالة).

messages — رسائل تواصل المستخدمين.

الإيميل وإعادة التفعيل

PHPMailer مفعّل عبر Composer (composer.json يطلب phpmailer/phpmailer:^6.1).

ملف mail.php مهيأ لـ SMTP (Gmail)، بس محتاج تكميل/تحديث إعدادات: Host, Username, Password, From… الخ.

التفعيل: active.php يحدّث Activated=true بناءً على security_code (يتولّد بـ md5(date("h:i:s"))).

الأمان (بصراحة وباختصار)

كلمات المرور في جدول users تحفظ كنص عادي (Plaintext). لازم تتحوّل لـ hash (مثلاً password_hash() و password_verify()).

reCAPTCHA secret موجود صريح في user_login.php — بدّله من Google reCAPTCHA الخاص بك وخزّنه كمتغير بيئي.

DB creds في components/connect.php (حاليًا root بدون كلمة مرور) — غيّرها وخلّها من البيئة.

ارفع الصور بفلترة صارمة للامتدادات والحجم، وتوليد أسماء عشوائية.

حدّث عناوين الـ SMTP في mail.php لاستخدام App Password لـ Gmail أو خدمة بريد مخصّصة.

كيف تشغّله محليًا (Local)

أنشئ قاعدة بيانات shop_db في MySQL.

استورد 111/11/shop_db.sql.

عدّل الاتصال في 111/11/components/connect.php (host/db/user/pass).

Composer install داخل مجلد 111/11/ لتثبيت PHPMailer:

(لو مش متاح Composer عندك، نزّل مجلد vendor يدوياً أو غيّر mail.php لاستخدام نسخة PHPMailer مضمنة).

حدّث إعدادات mail.php (SMTP) وuser_login.php (مفاتيح reCAPTCHA).

ضع المشروع تحت localhost (XAMPP/WAMP)، وافتح http://localhost/111/11/home.php.

ادخل لوحة الإدارة: http://localhost/111/11/admin/admin_login.php (أنشئ إدمن من register_admin.php لو لزم).

تدفّق المستخدم (User Journey)

يتصفح المنتجات → يضيف للسلة أو الـ Wishlist → عرض سريع/تفاصيل → Checkout → إنشاء طلب → يتابع حالة الطلب من صفحة orders.php.

تواصل: المستخدم يرسل رسالة من contact.php، تظهر للإدمن في admin/messages.php.

تفعيل الحساب/الاستعادة: عبر روابط الكود active.php وعمليات reset.php.
