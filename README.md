<div align="center">

# ⬛ Black Manual SSL for Iran
### The Ultimate Fail-Proof Offline SSL Setup for Restricted Networks

![Status](https://img.shields.io/badge/Status-Anti__Censorship-black?style=for-the-badge)
![OS](https://img.shields.io/badge/OS-Linux-white?style=for-the-badge&logo=linux&logoColor=black&color=white)
![Security](https://img.shields.io/badge/Security-Strict-black?style=for-the-badge)

</div>

**Use Case:** Getting Let's Encrypt / ZeroSSL certificates for servers that cannot connect to the global internet (like Iran-Access servers), using your local computer.

Automated tools like `certbot` frequently fail in restricted environments due to DNS poisoning or CDN API blocks. This manual approach guarantees a 100% successful SSL setup by generating the certificate offline via DNS Challenge and manually installing it on the server.

---

## 🛠️ Phase 1: Generate the Offline SSL Certificate (DNS Challenge)

### Step 1: Initiate the SSL Request
1. Open a browser on a computer with unrestricted internet access (use a VPN if necessary).
2. Go to a free SSL generator website like **[PunchSalad](https://punchsalad.com/ssl-certificate-generator/)** or **[ZeroSSL](https://zerossl.com)**.
3. Enter your exact domain or subdomain (e.g., `panel.yourdomain.com`).

### Step 2: Select the Verification Method
* **CRITICAL:** When asked how you want to verify domain ownership, you must select **DNS Verification (TXT Record)**. 
* Do **NOT** select HTTP/File upload, as it will fail on restricted servers.

### Step 3: Create the DNS TXT Record (The Tricky Part)
The SSL website will generate a "Name/Host" and a "Value" for you.
1. Log in to your domain's DNS management panel (e.g., Cloudflare, ArvanCloud, ParsPack).
2. Create a new **TXT Record**.
3. **Name/Host:** Type **ONLY** `_acme-challenge.panel` 
   * *Warning:* Do **NOT** type the full `_acme-challenge.panel.yourdomain.com`. The DNS panel automatically appends your root domain at the end. Writing the full name will break the verification.
4. **Value/Text:** Paste the long verification string provided by the SSL website.
5. **Proxy Status:** Ensure any CDN or Proxy (the cloud icon) is turned **OFF** (DNS Only) for this record.
6. Save the record.

### Step 4: Verify and Retrieve Certificates
1. Wait about 2 to 3 minutes for the new DNS record to propagate globally.
2. Go back to the SSL generator website and click the **Verify DNS** (or Check DNS) button.
3. Once successful, the site will issue your certificates. You will receive two crucial text blocks:
   * **Certificate (CRT / Fullchain)**
   * **Private Key (KEY)**

---

## 💻 Phase 2: Install Certificates on Your Linux Server

Connect to your server via SSH and follow these commands:

### 1. Create the SSL Directory
```bash
sudo mkdir -p /etc/ssl
```

### 2. Create the Certificate File
```bash
sudo nano /etc/ssl/certificate.crt
```
* Paste your entire **Certificate** text here.
* Save: Press `CTRL + X`, then `Y`, and hit `Enter`.

### 3. Create the Private Key File
```bash
sudo nano /etc/ssl/private.key
```
* Paste your entire **Private Key** text here.
* Save: Press `CTRL + X`, then `Y`, and hit `Enter`.

### 4. Secure the Files (Crucial Step)
Set the correct Linux file permissions so the private key remains hidden from non-root users.
```bash
sudo chmod 644 /etc/ssl/certificate.crt
sudo chmod 600 /etc/ssl/private.key
```

Now, simply enter `/etc/ssl/certificate.crt` and `/etc/ssl/private.key` in your panel's SSL configuration (e.g. Nginx).

---
---

<div align="center" dir="rtl">

# ⬛ راهنمای دریافت و نصب دستی SSL برای ایران
### قطعی‌ترین روش آفلاین برای سرورهای محدود شده (ایران-اکسس و اینترانت)

</div>

<div dir="rtl">

📌 **کاربرد این آموزش چیست؟**
* دریافت رایگان گواهی معتبر از Let's Encrypt یا ZeroSSL.
* مخصوص سرورهایی که اینترنت آزاد ندارند (مثل سرورهای ایران-اکسس).
* انجام تمام مراحل با کامپیوتر شخصی شما (بدون نیاز به اتصال مستقیم سرور به اینترنت جهانی).

⚠️ **چرا ابزارهای خودکار کار نمی‌کنند؟**
* ابزارهایی مثل `certbot` روی سرورهای ایران معمولاً به دلیل فیلترینگ، اختلال در DNS یا تحریم‌ها تایم‌اوت (Timeout) می‌دهند.
* این روش دستی (آفلاین) از طریق تاییدیه DNS، نیاز سرور به اینترنت آزاد را دور می‌زند.
* این یعنی ۱۰۰٪ موفقیت و تضمین دریافت گواهی SSL بدون قطعی!

---

## 🛠️ فاز اول: ساخت گواهی SSL آفلاین (روش DNS Challenge)

### 📌 مرحله ۱: ثبت درخواست SSL
* ۱. 🌐 روی یک سیستم که اینترنت آزاد دارد (در صورت نیاز با VPN) مرورگر خود را باز کنید.
* ۲. 🔗 وارد یکی از سایت‌های رایگان سازنده SSL شوید (پیشنهاد ما: **[PunchSalad](https://punchsalad.com/ssl-certificate-generator/)** یا **[ZeroSSL](https://zerossl.com)**).
* ۳. 📝 آدرس دقیق دامنه یا ساب‌دامین خود را وارد کنید (مثلاً `panel.yourdomain.com`).

### 📌 مرحله ۲: انتخاب روش احراز هویت
* 🚨 **بسیار مهم:** برای تایید مالکیت دامنه، حتماً و حتماً گزینه **DNS Verification (TXT Record)** را انتخاب کنید.
* ❌ به هیچ وجه از روش HTTP/File upload استفاده نکنید (روی سرورهای ایران با خطا مواجه می‌شود).

### 📌 مرحله ۳: ساخت رکورد DNS (بخش حساس)
سایت سازنده SSL به شما یک نام (`Name/Host`) و یک مقدار (`Value`) می‌دهد. حالا باید مراحل زیر را انجام دهید:

* ۱. ⚙️ وارد پنل مدیریت DNS دامنه خود شوید (مثل کلودفلر، آروان‌کلود، پارس‌پک).
* ۲. ➕ یک رکورد جدید از نوع **TXT** بسازید.
* ۳. 🏷️ **بخش Name/Host:** در این کادر **فقط و فقط** عبارت `_acme-challenge.panel` را بنویسید.
  * 🛑 **هشدار شدید:** آدرس کامل دامنه را ننویسید! (مثلاً `_acme-challenge.panel.yourdomain.com` غلط است). پنل‌های DNS خودشان بقیه آدرس را اضافه می‌کنند.
* ۴. 📋 **بخش Value/Text:** رشته متنی طولانی که سایت به شما داده را دقیقاً اینجا کپی و پیست کنید.
* ۵. ☁️ **وضعیت پروکسی:** دقت کنید که آیکون ابر (پروکسی / CDN) برای این رکورد کاملاً **خاموش** (DNS Only) باشد.
* ۶. 💾 در نهایت رکورد را ذخیره کنید.

### 📌 مرحله ۴: تایید و دریافت گواهی‌ها
* ۱. ⏳ حدود ۲ الی ۳ دقیقه صبر کنید تا تنظیمات DNS جدید در کل اینترنت اعمال شود.
* ۲. 🔄 به سایت سازنده SSL برگردید و روی دکمه **Verify DNS** (یا Check DNS) کلیک کنید.
* ۳. 🎉 در صورت موفقیت، سایت ۲ فایل متنی بسیار مهم به شما می‌دهد:
  * 📄 **متن گواهی** (Certificate / CRT / Fullchain)
  * 🔑 **متن کلید خصوصی** (Private Key / KEY)

---

## 💻 فاز دوم: نصب گواهی‌ها روی سرور لینوکس

از طریق SSH (نرم‌افزار پوتی یا ترمینال) به سرور لینوکس خود وصل شوید و این دستورات را خط به خط اجرا کنید:

### ۱. 📁 ساخت پوشه امن برای SSL
```bash
sudo mkdir -p /etc/ssl
```

### ۲. 📄 ساخت فایل Certificate
```bash
sudo nano /etc/ssl/certificate.crt
```
* 📋 تمام متن **Certificate** خود را کپی کرده و اینجا پیست کنید.
* 💾 برای ذخیره: کلیدهای `CTRL + X` را بزنید، سپس حرف `Y` را تایپ کنید و `Enter` بزنید.

### ۳. 🔑 ساخت فایل Private Key
```bash
sudo nano /etc/ssl/private.key
```
* 📋 تمام متن **Private Key** خود را کپی کرده و اینجا پیست کنید.
* 💾 برای ذخیره: کلیدهای `CTRL + X` را بزنید، سپس حرف `Y` را تایپ کنید و `Enter` بزنید.

### ۴. 🛡️ ایمن‌سازی فایل‌ها (مرحله حیاتی)
این دستورات را وارد کنید تا سطح دسترسی فایل‌ها در لینوکس محدود شود و کسی جز مالک اصلی (root) نتواند کلید خصوصی شما را ببیند.
```bash
sudo chmod 644 /etc/ssl/certificate.crt
sudo chmod 600 /etc/ssl/private.key
```

✅ **کار تمام است!**
حالا کافیست مسیر `/etc/ssl/certificate.crt` و `/etc/ssl/private.key` را در تنظیمات پنل خود (مثل انجینکس) وارد کنید.

</div>

---
## 💖 Support the Project

If this tool has helped you manage your Windows services more efficiently, consider supporting its development. Your donations help keep the project updated and maintained.

### 💰 Crypto Donations

You can support me by sending **Litecoin** or **TON** to the following addresses:

| Asset | Wallet Address |
| :--- | :--- |
| **Litecoin (LTC)** | `ltc1qxhuvs6j0suvv50nqjsuujqlr3u4ekfmys2ydps` |
| **TON Network** | `UQAHI_ySJ1HTTCkNxuBB93shfdhdec4LSgsd3iCOAZd5yGmc` |

