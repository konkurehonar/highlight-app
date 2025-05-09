Structure
# ساختار پروژه کوشک کنکورهنر

## 📖 نمای کلی پروژه

**نام پروژه:** کوشک کنکورهنر (Koushk Art Entrance Exam)  
**هدف:** وب‌اپلیکیشن سئومحور برای انتشار سریع محتوا و جذب ترافیک بالا با بلاگ و قابلیت RAG در فازهای بعدی  

## 🎯 اهداف نسخه اولیه (MVP)

1. انتشار اولین مقاله بلاگ (همراه با ۱۰ دسته‌بندی بلاگ در حالت قفل)
2. ایجاد زیرساخت مونو-ریپو با NX (`apps/`, `packages/`, `tools/`, `docker/`)
3. پیاده‌سازی CI: GitLab CI با یک Pipeline ساده (install → lint → build → deploy)
4. طراحی برندینگ اولیه (لوگو، پالت رنگ، تایپوگرافی)

## 🛠️ پشته فناوری

- **فرانت‌اند:** Next.js + React (میزبانی روی Vercel)
- **بک‌اند:** Rust با فریم‌ورک Axum در Docker
- **مدیریت محتوا و پایگاه داده RAG:** AppFlowy + Supabase (pgvector در آینده)
- **کش و صف پیام:** Redis, NATS
- **CI/CD:** GitLab CI + Docker Compose روی VPS
- **مانیتورینگ:** Grafana + Prometheus

## 📂 ساختار مخزن کد

/
├─ apps/
│  ├─ web/        # پروژه Next.js
│  └─ api/        # پروژه Rust (Axum)
├─ packages/
│  └─ ui/         # کامپوننت‌های React مشترک
├─ tools/
│  └─ scripts/    # اسکریپت‌های اتوماسیون و مایگریشن
├─ docker/
│  └─ docker-compose.yml
├─ nx.json
├─ workspace.json
└─ README.md
plaintext

Hide

## 🚀 شروع به کار

1. **کلون کردن** مخزن کد و اجرای دستورات زیر:
   ```bash
   git clone <repo-url> && cd repo
   npm install
راه‌اندازی کانتینرهای محلی:
bash

Hide
docker-compose up -d
حالت توسعه:
فرانت‌اند: cd apps/web && npm run dev
بک‌اند: cd apps/api && cargo run
دسترسی به برنامه:
فرانت‌اند: http://localhost:3000
بک‌اند API: http://localhost:8000
داشبورد Supabase: http://localhost:54323
🎨 برندینگ و هویت بصری
لوگو: /assets/logo.svg (نسخه‌های تیره/روشن)
پالت رنگ:
رنگ اصلی: #1E40AF
رنگ تأکیدی: #F59E0B
رنگ متن: #1F2937
رنگ پس‌زمینه: #F9FAFB
رنگ خطا: #DC2626
رنگ موفقیت: #059669
تایپوگرافی:
عنوان‌ها: Kufam
متن: VazirMatn
🧪 تست و کیفیت کد
تست فرانت‌اند: Jest + React Testing Library
bash

Hide
cd apps/web && npm run test
تست بک‌اند: Rust test framework
bash

Hide
cd apps/api && cargo test
لینت و فرمت:
فرانت‌اند: ESLint + Prettier
بک‌اند: Clippy + rustfmt
📦 استقرار و انتشار
محیط توسعه: Vercel Preview Deployments + Docker محلی
محیط Production: Vercel (فرانت‌اند) + VPS پارس‌پک (بک‌اند)
فرآیند انتشار:
Push به شاخه main برای استقرار خودکار
تگ زدن نسخه برای انتشار رسمی: git tag v1.0.0 && git push --tags
📚 مستندات
API: OpenAPI/Swagger در آدرس /api/docs
کامپوننت‌ها: Storybook برای UI کامپوننت‌ها
راهنمای توسعه‌دهندگان: در پوشه docs/
🔄 گردش کار توسعه
ایجاد شاخه جدید از main: git checkout -b feature/name
توسعه و تست محلی
ارسال درخواست merge/pull
بررسی کد و اجرای CI
ادغام با شاخه اصلی پس از تأیید
