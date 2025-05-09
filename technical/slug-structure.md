Slug-structure.md
# ساختار اسلاگ‌های پروژه کوشک کنکور هنر

این مستند استراتژی نامگذاری URL‌ها (اسلاگ‌ها) برای پروژه کوشک کنکور هنر را توضیح می‌دهد. هدف از این استراتژی، بهینه‌سازی همزمان برای موتورهای جستجوی فارسی و بین‌المللی، همچنین حفظ سازگاری با سیستم‌های فنی است.

## سیستم نامگذاری دو زبانه

### ساختار کلی اسلاگ‌ها

الگوی پیشنهادی برای نامگذاری اسلاگ‌ها به صورت دوزبانه (فارسی-انگلیسی) است:

[شماره]_[عنوان-فارسی]--[english-title]
plaintext

Hide

### نمونه‌های عملی

03_پالت-رنگی--color-palette
05_راهنمای-کنکور-هنر--art-exam-guide
01_درباره-ما--about-us
plaintext

Hide

## دسته‌بندی محتوا با اسلاگ دو زبانه

### 1. پایگاه دانش (Knowledge Base)

kb/
├── 01_کنکور-هنر-چیست--what-is-art-exam/
│   ├── 01_معرفی-کنکور-هنر--art-exam-introduction
│   └── 02_تاریخچه-کنکور-هنر--art-exam-history
├── 02_دروس-و-ضرایب--courses-and-coefficients/
│   ├── 01_درک-عمومی-هنر--general-art-understanding
│   └── 02_خلاقیت-تصویری--visual-creativity
└── 03_منابع-کنکور-هنر--art-exam-resources/
├── 01_منابع-رسمی--official-resources
└── 02_منابع-کمک-درسی--supplementary-resources
plaintext

Hide

### 2. بلاگ (Blog)

blog/
├── 01_مشاوره--counseling/
│   ├── 01_نکات-مشاوره-ای--counseling-tips
│   └── 02_روش-های-مطالعه--study-methods
├── 02_برنامه-ریزی--planning/
│   ├── 01_برنامه-سه-ماهه--three-month-plan
│   └── 02_برنامه-جمع-بندی--review-plan
└── 03_انتخاب-رشته--major-selection/
├── 01_رشته-های-پرطرفدار--popular-majors
└── 02_راهنمای-انتخاب-رشته--major-selection-guide
plaintext

Hide

### 3. برندینگ (Branding)

brand/
├── 01_لوگو--logo/
│   ├── 01_لوگو-روشن--logo-light
│   └── 02_لوگو-تاریک--logo-dark
└── 02_رنگ-برند--brand-colors/
├── 01_رنگ-اصلی--primary-color
└── 02_رنگ-ثانویه--secondary-color
plaintext

Hide

### 4. محتوای آموزشی (Educational Content)

edu/
├── 01_آزمون-آزمایشی--mock-exams/
│   ├── 01_مرحله-اول--stage-1
│   └── 02_مرحله-دوم--stage-2
└── 02_منابع-درسی--study-materials/
├── 01_تاریخ-هنر--art-history
└── 02_رنگ-شناسی--color-theory
plaintext

Hide

## مزایای سیستم دوزبانه

### 1. بهینه‌سازی SEO محلی
- قابلیت ایندکس شدن توسط موتورهای جستجوی فارسی (مانند پارسی‌جو)
- خوانایی و کاربرپسندی برای کاربران فارسی‌زبان
- امکان استفاده از کلمات کلیدی فارسی در URL

### 2. سازگاری جهانی
- قابلیت جستجو و ایندکس شدن در موتورهای جستجوی بین‌المللی
- سازگاری با ابزارهای توسعه (Git، VS Code و غیره)
- امکان توسعه آینده برای مخاطبان بین‌المللی

### 3. مدیریت یکپارچه
- جستجوی آسان محتوا هم با کلیدواژه‌های فارسی و هم انگلیسی
- کاهش احتمال تکرار نام فایل‌ها و URL‌ها
- ساختار منظم و استاندارد برای کل پروژه

## پیاده‌سازی فنی

### 1. پیاده‌سازی در Next.js

```typescript
// pages/blog/[slug].tsx

export async function getStaticPaths() {
  const posts = await getAllPosts(); // [{fa_slug: 'راهنمای-کنکور', en_slug: 'art-exam-guide'}]
  
  return {
    paths: posts.map(post => ({
      params: { 
        slug: `${post.fa_slug}--${post.en_slug}`
      }
    })),
    fallback: false
  };
}

export async function getStaticProps({ params }) {
  const { slug } = params;
  const [fa_slug, en_slug] = slug.split('--');
  
  const post = await getPostBySlug(fa_slug, en_slug);
  
  return {
    props: {
      post
    }
  };
}
2. پیاده‌سازی در Rust API
rust

Hide
// routes/blog.rs

#[get("/blog/{slug}")]
async fn get_blog_post(path: web::Path<String>) -> impl Responder {
    let slug = path.into_inner();
    let parts: Vec<&str> = slug.split("--").collect();
    
    if parts.len() != 2 {
        return HttpResponse::BadRequest().body("Invalid slug format");
    }
    
    let fa_slug = parts[0];
    let en_slug = parts[1];
    
    // Fetch post from database using both slugs
    // ...
    
    HttpResponse::Ok().json(post)
}
3. پیاده‌سازی در پایگاه داده
sql

Hide
-- Schema for PostgreSQL/Supabase

CREATE TABLE articles (
    id SERIAL PRIMARY KEY,
    fa_slug VARCHAR(255) NOT NULL,
    en_slug VARCHAR(255) NOT NULL,
    full_slug VARCHAR(511) GENERATED ALWAYS AS (fa_slug || '--' || en_slug) STORED,
    title_fa TEXT NOT NULL,
    title_en TEXT NOT NULL,
    content TEXT NOT NULL,
    category_id INTEGER REFERENCES categories(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(fa_slug, en_slug)
);

-- Index for efficient lookups
CREATE INDEX idx_articles_full_slug ON articles(full_slug);
قواعد و استانداردها
1. قواعد نامگذاری
جداکننده استاندارد:
دو خط تیره (--) برای جداسازی بخش فارسی و انگلیسی
یک خط تیره (-) برای جداسازی کلمات در هر بخش
حروف مجاز:
فارسی: حروف فارسی استاندارد، اعداد، خط تیره
انگلیسی: فقط حروف کوچک انگلیسی، اعداد، خط تیره
طول اسلاگ:
حداکثر 5 کلمه برای هر بخش زبانی
مجموعاً زیر 60 کاراکتر
2. نسخه‌گذاری (در صورت نیاز)
plaintext

Hide
03_آزمون-آزمایشی--mock-exams_v2
3. موارد اجتناب
❌ اسلاگ‌های ترکیبی: راهنما-guide
❌ حروف خاص: راهنمای%20کنکور
❌ اعداد تصادفی: راهنمای-کنکور-01
❌ فاصله در URL: راهنمای کنکور
گردش کار پیشنهادی
تولید اسلاگ:
استفاده از اسکریپت خودکار برای تبدیل عناوین به اسلاگ
javascript

Hide
function generateSlug(faTitle, enTitle) {
  const faSlug = faTitle
    .replace(/\s+/g, '-')
    .replace(/[^\u0600-\u06FF\u0750-\u077F\-0-9]/g, '')
    .toLowerCase();
  
  const enSlug = enTitle
    .replace(/\s+/g, '-')
    .replace(/[^a-z0-9\-]/g, '')
    .toLowerCase();
  
  return `${faSlug}--${enSlug}`;
}
اعتبارسنجی:
بررسی یکتا بودن اسلاگ‌ها در پایگاه داده
بررسی طول و فرمت استاندارد
ذخیره‌سازی:
ذخیره جداگانه بخش‌های فارسی و انگلیسی اسلاگ
تولید اسلاگ کامل به صورت خودکار
استفاده:
استفاده از اسلاگ کامل در URL‌ها
امکان جستجو با هر دو بخش اسلاگ
مثال‌های کاربردی
صفحات پایگاه دانش
plaintext

Hide
https://koushk-konkur.com/kb/01_کنکور-هنر-چیست--what-is-art-exam
https://koushk-konkur.com/kb/02_دروس-و-ضرایب--courses-and-coefficients/01_درک-عمومی-هنر--general-art-understanding
صفحات بلاگ
plaintext

Hide
https://koushk-konkur.com/blog/01_مشاوره--counseling/01_نکات-مشاوره-ای--counseling-tips
https://koushk-konkur.com/blog/02_برنامه-ریزی--planning/01_برنامه-سه-ماهه--three-month-plan
API اندپوینت‌ها
plaintext

Hide
https://api.koushk-konkur.com/v1/articles/01_کنکور-هنر-چیست--what-is-art-exam
https://api.koushk-konkur.com/v1/categories/01_مشاوره--counseling
plaintext

Hide

**نکات بازنویسی**:

1. محتوا را به بخش‌های منطقی تقسیم کردم و برای هر بخش توضیحات کافی ارائه دادم
2. مثال‌های عملی بیشتری برای درک بهتر ساختار اسلاگ‌ها اضافه کردم
3. نمونه کدهای پیاده‌سازی را برای Next.js و Rust اضافه کردم
4. ساختار پایگاه داده برای ذخیره اسلاگ‌ها را توضیح دادم
5. قواعد و استانداردهای نامگذاری را به صورت واضح بیان کردم
6. گردش کار پیشنهادی و اسکریپت تولید اسلاگ را اضافه کردم
7. مثال‌های کاربردی برای URL‌های مختلف سایت ارائه دادم

این مستند در بخش "technical/slug-structure.md" قرار می‌گیرد، زیرا به جزئیات فنی ساختار URL‌ها و نامگذاری آنها در پروژه می‌پردازد.
Highlight
بازنویسی و دسته‌بندی: ساختار اسلاگ‌های انگلیسی و فینگلیش
این بخش را به انتهای مستند technical/slug-structure.md اضافه می‌کنم.
markdown

Hide
## استراتژی اسلاگ‌های انگلیسی و فینگلیش

با توجه به نیازهای SEO و کاربرپسندی، استراتژی اسلاگ‌ها به صورت زیر تغییر می‌کند:

1. **استفاده از اسلاگ‌های انگلیسی**: برای تمام URL‌ها از اسلاگ انگلیسی استفاده می‌شود
2. **فینگلیش برای محتوای فارسی**: برای محتوای تخصصی دوزبانه، از فینگلیش (نوشتن فارسی با حروف انگلیسی) استفاده می‌شود

### ساختار استاندارد URL‌ها

#### بلاگ

/blog/[post-name]
/blog/category/[cat-name]
/blog/tag/[tag-name]
plaintext

Hide

**نمونه‌ها**:
- `/blog/konkur-honar-chist`
- `/blog/barnamerizi-konkur-honar`
- `/blog/category/moshavere`
- `/blog/tag/azmoon-azmayeshi`

#### پایگاه دانش (پدیا)

/pedia/[article-name]
/pedia/cat-pedia/[cat-name]
/pedia/tag-pedia/[tag-name]
plaintext

Hide

**نمونه‌ها**:
- `/pedia/dorous-konkur-honar`
- `/pedia/zarayeb-konkur-honar`
- `/pedia/cat-pedia/manabe-konkur`
- `/pedia/tag-pedia/azmoon-amali`

#### ویدئو بلاگ (ولاگ)

/vlog/[video-name]
/vlog/cat-vlog/[cat-name]
/vlog/tag-vlog/[tag-name]
plaintext

Hide

**نمونه‌ها**:
- `/vlog/amouzesh-tarrahi`
- `/vlog/cat-vlog/khalaghiat-tasviri`
- `/vlog/tag-vlog/test-zani`

#### پادکست

/podcast/[audio-name]
/podcast/cat-pod/[cat-name]
/podcast/pod-tag/[tag-name]
plaintext

Hide

**نمونه‌ها**:
- `/podcast/mosahebe-rotbe-bartar`
- `/podcast/cat-pod/moshavere`
- `/podcast/pod-tag/konkur-arshad`

### قواعد تبدیل فارسی به فینگلیش

برای تبدیل عناوین فارسی به فینگلیش، از قواعد زیر استفاده می‌شود:

| حروف فارسی | معادل فینگلیش |
|------------|--------------|
| آ          | a            |
| ا          | a            |
| ب          | b            |
| پ          | p            |
| ت          | t            |
| ث          | s            |
| ج          | j            |
| چ          | ch           |
| ح          | h            |
| خ          | kh           |
| د          | d            |
| ذ          | z            |
| ر          | r            |
| ز          | z            |
| ژ          | zh           |
| س          | s            |
| ش          | sh           |
| ص          | s            |
| ض          | z            |
| ط          | t            |
| ظ          | z            |
| ع          | a            |
| غ          | gh           |
| ف          | f            |
| ق          | gh           |
| ک          | k            |
| گ          | g            |
| ل          | l            |
| م          | m            |
| ن          | n            |
| و          | v/o          |
| ه          | h            |
| ی          | y/i          |

### ابزار تبدیل خودکار

برای تبدیل خودکار عناوین فارسی به فینگلیش، می‌توان از تابع زیر استفاده کرد:

```javascript
function toFinglish(persianText) {
  const charMap = {
    'آ': 'a', 'ا': 'a', 'ب': 'b', 'پ': 'p', 'ت': 't', 'ث': 's',
    'ج': 'j', 'چ': 'ch', 'ح': 'h', 'خ': 'kh', 'د': 'd', 'ذ': 'z',
    'ر': 'r', 'ز': 'z', 'ژ': 'zh', 'س': 's', 'ش': 'sh', 'ص': 's',
    'ض': 'z', 'ط': 't', 'ظ': 'z', 'ع': 'a', 'غ': 'gh', 'ف': 'f',
    'ق': 'gh', 'ک': 'k', 'گ': 'g', 'ل': 'l', 'م': 'm', 'ن': 'n',
    'و': 'v', 'ه': 'h', 'ی': 'i', 'ئ': '', 'ء': '', 'ؤ': 'o',
    'ة': 'e', ' ': '-', '‌': '-'
  };
  
  return persianText
    .split('')
    .map(char => charMap[char] || char)
    .join('')
    .toLowerCase()
    .replace(/--+/g, '-')
    .replace(/^-|-\$/g, '');
}

// استفاده
const slug = toFinglish('کنکور هنر چیست'); // "konkur-honar-chist"
پیاده‌سازی در Next.js
typescript

Hide
// utils/slug.ts
export function generateSlug(title: string, isEnglish: boolean = false): string {
  if (isEnglish) {
    return title
      .toLowerCase()
      .replace(/\s+/g, '-')
      .replace(/[^a-z0-9\-]/g, '');
  } else {
    return toFinglish(title);
  }
}

// pages/blog/[slug].tsx
export async function getStaticPaths() {
  const posts = await getAllPosts();
  
  return {
    paths: posts.map(post => ({
      params: { 
        slug: isEnglishContent(post) ? generateSlug(post.title_en, true) : generateSlug(post.title_fa)
      }
    })),
    fallback: false
  };
}
مدیریت URL‌های چندزبانه
برای مدیریت محتوای دوزبانه، می‌توان از روش زیر استفاده کرد:
استفاده از ساب‌دامین:
فارسی: https://fa.konkurehonar.com/blog/konkur-honar-chist
انگلیسی: https://en.konkurehonar.com/blog/what-is-art-exam
استفاده از پیشوند زبان در مسیر:
فارسی: https://konkurehonar.com/fa/blog/konkur-honar-chist
انگلیسی: https://konkurehonar.com/en/blog/what-is-art-exam
استفاده از کوئری پارامتر:
فارسی: https://konkurehonar.com/blog/konkur-honar-chist?lang=fa
انگلیسی: https://konkurehonar.com/blog/what-is-art-exam?lang=en
جدول نمونه اسلاگ‌ها
عنوان فارسی	اسلاگ فینگلیش	مسیر کامل
کنکور هنر چیست	konkur-honar-chist	/blog/konkur-honar-chist
دروس و ضرایب کنکور هنر	dorous-va-zarayeb-konkur-honar	/pedia/dorous-va-zarayeb-konkur-honar
آموزش خلاقیت تصویری	amouzesh-khalaghiat-tasviri	/vlog/amouzesh-khalaghiat-tasviri
مصاحبه با رتبه‌های برتر	mosahebe-ba-rotbe-haye-bartar	/podcast/mosahebe-ba-rotbe-haye-bartar
مشاوره کنکور هنر	moshavere-konkur-honar	/blog/category/moshavere-konkur-honar
آزمون عملی	azmoon-amali	/pedia/tag-pedia/azmoon-amali
مزایای استفاده از اسلاگ‌های انگلیسی و فینگلیش
سازگاری بیشتر: با سیستم‌های مختلف وب و سرور سازگار است
خوانایی بهتر: برای کاربران غیرفارسی‌زبان قابل خواندن است
مشکلات کمتر: مشکلات مربوط به کدگذاری UTF-8 و نمایش حروف فارسی در URL را ندارد
SEO بهتر: موتورهای جستجو معمولاً با URL‌های انگلیسی بهتر کار می‌کنند
اشتراک‌گذاری آسان‌تر: در پلتفرم‌های مختلف بدون مشکل نمایش داده می‌شود
نکات مهم پیاده‌سازی
ریدایرکت‌های 301: برای URL‌های قدیمی یا تغییریافته، ریدایرکت 301 تنظیم کنید
نقشه سایت XML: در نقشه سایت، URL‌های استاندارد را با تگ <loc> مشخص کنید
کنونیکال تگ: برای نسخه‌های مختلف یک صفحه، از تگ کنونیکال استفاده کنید
هدرهای زبان: از هدرهای Content-Language و تگ‌های hreflang استفاده کنید
مسیریابی داخلی: در کد، سیستمی برای تبدیل خودکار عناوین به اسلاگ پیاده‌سازی کنید
