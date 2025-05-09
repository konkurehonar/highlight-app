Font-optimization

بازنویسی و دسته‌بندی: بهینه‌سازی فونت با ابزار پایتون
این مستند در بخش technical/font-optimization.md قرار می‌گیرد.
markdown

Hide
# بهینه‌سازی فونت با حذف کاراکترهای اضافی

این مستند نحوه بهینه‌سازی فایل‌های فونت با استفاده از پایتون و کتابخانه FontTools را توضیح می‌دهد. هدف از این فرآیند، کاهش حجم فونت‌ها با حذف کاراکترهای غیرضروری و بهبود زمان بارگذاری صفحات وب است.

## مزایای بهینه‌سازی فونت

- **کاهش حجم فایل**: حجم فونت‌ها را می‌توان تا 70-90% کاهش داد
- **بارگذاری سریع‌تر**: زمان بارگذاری صفحات وب بهبود می‌یابد
- **مصرف کمتر پهنای باند**: ترافیک شبکه کاهش می‌یابد
- **تجربه کاربری بهتر**: تأخیر کمتر در نمایش متن به کاربران

## پیش‌نیازها

قبل از اجرای اسکریپت، باید موارد زیر را نصب کنید:

```bash
pip install fonttools brotli zopfli
کد بهینه‌سازی فونت
اسکریپت زیر با استفاده از کتابخانه FontTools، یک زیرمجموعه از فونت را با حفظ کاراکترهای فارسی و لاتین مورد نیاز ایجاد می‌کند:
python

Hide
from fontTools.subset import Subsetter, load_font, save_font

# لیست یونیکدهای مورد نیاز (فارسی + لاتین)
unicodes = [
    # فارسی
    0x0600, 0x06FF,    # حروف پایه فارسی/عربی
    0x0750, 0x077F,    # حروف گسترش‌یافته عربی
    0xFB50, 0xFDFF,    # نمایش پیوسته حروف عربی
    0x06F0, 0x06F9,    # اعداد فارسی
    
    # لاتین
    0x0020, 0x007E,    # حروف و اعداد لاتین و علامت‌ها
]

# بارگیری فونت اصلی
font = load_font("Vazirmatn-Regular.ttf")

# زیرمجموعه‌سازی
subsetter = Subsetter()
subsetter.populate(unicodes=unicodes)
subsetter.subset(font)

# ذخیره فونت بهینه‌شده
save_font(font, "Vazirmatn-Regular-subset.ttf")
نحوه استفاده
فایل پایتون را با نام font_optimizer.py ذخیره کنید
فونت مورد نظر را در همان مسیر قرار دهید
در صورت نیاز، نام فایل فونت ورودی و خروجی را در کد تغییر دهید
اسکریپت را با دستور زیر اجرا کنید:
bash

Hide
python font_optimizer.py
فایل فونت بهینه‌شده با پسوند -subset ایجاد خواهد شد
تنظیمات پیشرفته
افزودن محدوده‌های یونیکد بیشتر
برای افزودن مجموعه‌های دیگر کاراکترها، می‌توانید محدوده‌های یونیکد را به لیست اضافه کنید:
python

Hide
unicodes = [
    # محدوده‌های موجود
    # ...
    
    # اعداد فارسی گسترش‌یافته
    0x0660, 0x0669,    # اعداد عربی شرقی
    
    # کاراکترهای ویژه و نشانه‌گذاری
    0x2000, 0x206F,    # علائم عمومی نقطه‌گذاری
    0x2070, 0x209F,    # اندیس‌ها و توان‌ها
]
تنظیمات فشرده‌سازی بیشتر
برای فشرده‌سازی بیشتر فونت، می‌توانید از گزینه‌های پیشرفته استفاده کنید:
python

Hide
options = {
    "desubroutinize": True,      # حذف زیرروال‌ها
    "no-hinting": True,          # حذف اطلاعات hinting
    "layout-features": "*",      # حفظ همه ویژگی‌های طرح‌بندی
    "name-IDs": "*",             # حفظ همه شناسه‌های نام
    "notdef-outline": True,      # حفظ طرح کاراکتر notdef
    "recalc-bounds": True,       # محاسبه مجدد محدوده‌ها
    "recalc-timestamp": False,   # عدم محاسبه مجدد timestamp
}

subsetter = Subsetter(options=options)
بهینه‌سازی گروهی فونت‌ها
برای بهینه‌سازی چندین فونت به صورت همزمان:
python

Hide
import os
from fontTools.subset import Subsetter, load_font, save_font

# تنظیمات یونیکد
unicodes = [
    # محدوده‌های یونیکد
    # ...
]

# پوشه حاوی فونت‌ها
font_dir = "fonts"
output_dir = "fonts-optimized"

# اطمینان از وجود پوشه خروجی
os.makedirs(output_dir, exist_ok=True)

# بهینه‌سازی همه فونت‌ها
for filename in os.listdir(font_dir):
    if filename.endswith(('.ttf', '.otf')):
        input_path = os.path.join(font_dir, filename)
        output_path = os.path.join(output_dir, filename.replace('.', '-subset.'))
        
        print(f"Processing {filename}...")
        font = load_font(input_path)
        
        subsetter = Subsetter()
        subsetter.populate(unicodes=unicodes)
        subsetter.subset(font)
        
        save_font(font, output_path)
        
        # نمایش میزان کاهش حجم
        original_size = os.path.getsize(input_path)
        new_size = os.path.getsize(output_path)
        reduction = (1 - new_size / original_size) * 100
        print(f"  Reduced from {original_size/1024:.1f}KB to {new_size/1024:.1f}KB ({reduction:.1f}% reduction)")
استفاده در پروژه کوشک کنکور هنر
برای پروژه کوشک کنکور هنر، می‌توان این اسکریپت را برای بهینه‌سازی فونت‌های مورد استفاده در وبسایت به کار برد:
فونت‌های Vazirmatn و Kufam را بهینه‌سازی کنید
فایل‌های بهینه‌شده را در پوشه /public/fonts پروژه قرار دهید
در فایل CSS یا کد Next.js، از فونت‌های بهینه‌شده استفاده کنید:
css

Hide
@font-face {
  font-family: 'Vazirmatn';
  src: url('/fonts/Vazirmatn-Regular-subset.woff2') format('woff2'),
       url('/fonts/Vazirmatn-Regular-subset.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
منابع بیشتر
مستندات FontTools
استراتژی‌های بهینه‌سازی فونت وب
تبدیل TTF به WOFF2 برای بهینه‌سازی بیشتر

