<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:050510,40:0f0c29,70:302b63,100:00ff9d&height=230&section=header&text=PROXY%20ROTATOR&fontSize=64&fontColor=00ff9d&animation=twinkling&desc=scrape%20%C2%B7%20check%20%C2%B7%20rotate%20%C2%B7%20repeat&descAlignY=76&descSize=17&fontAlignY=38" />

<a href="https://github.com/THEBLODYGHOSTS/proxy-rotator">
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=2600&pause=900&color=00FF9D&center=true&vCenter=true&width=760&lines=%5B%2B%5D+106+sources+scraped+in+parallel;%5B%2B%5D+latency+measured+through+each+proxy;%5B%2B%5D+system+proxy+rotated+every+3s;%5B%2B%5D+auto-refill+before+the+pool+runs+dry" />
</a>

<br/>

<a href="#"><img src="https://img.shields.io/badge/Python-3.8%2B-00ff9d?style=flat-square&labelColor=0d1117&logo=python&logoColor=00ff9d" /></a>
<a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-00ff9d?style=flat-square&labelColor=0d1117" /></a>
<a href="#"><img src="https://img.shields.io/badge/Sources-106-00ff9d?style=flat-square&labelColor=0d1117" /></a>
<a href="#"><img src="https://img.shields.io/badge/Proxies-HTTP%20%7C%20SOCKS4%20%7C%20SOCKS5-7c3aed?style=flat-square&labelColor=0d1117" /></a>
<a href="#"><img src="https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows%20%7C%20Android-00ff9d?style=flat-square&labelColor=0d1117" /></a>
<a href="https://t.me/NulInetwork"><img src="https://img.shields.io/badge/by-THEBLODYGHOSTS-7c3aed?style=flat-square&labelColor=0d1117&logo=github&logoColor=00ff9d" /></a>

</div>

```console
root@localhost:~# proxy-rotator

[+] 106 sources              ──▶  31,429 unique candidates
[+] checking with 400 threads ──▶  143 alive
[+] sorted by latency (ms)    ──▶  182 · 204 · 231 · 258 …
[~] rotating every 3s         ──▶  45.132.19.7:8080  (182 ms)
[~] pool low                  ──▶  auto-refill in background ⟲
```

> یک ابزار تک‌فایلی و بدون وابستگی برای **دامپ پراکسی از ۱۰۶ منبع**، **تست سلامت و پینگ واقعی**،
> و **چرخش خودکار پراکسی سیستم** هر ۳ ثانیه. وقتی پراکسی‌ها ته کشید، خودش دوباره دامپ می‌کند.
>
> `Build it. Break it. Understand it. Rebuild it better.`

---

## ⚡ نصب

### 🐧 لینوکس / مک

```bash
git clone https://github.com/THEBLODYGHOSTS/proxy-rotator.git
cd proxy-rotator
chmod +x install.sh
./install.sh --with-socks
```

یا بدون clone:

```bash
curl -fsSL https://raw.githubusercontent.com/THEBLODYGHOSTS/proxy-rotator/main/install.sh | bash
```

### 🪟 ویندوز

```powershell
git clone https://github.com/THEBLODYGHOSTS/proxy-rotator.git
cd proxy-rotator
powershell -ExecutionPolicy Bypass -File .\install.ps1 -WithSocks
```

### 📦 با pipx

```bash
pipx install "git+https://github.com/THEBLODYGHOSTS/proxy-rotator.git"
# یا از داخل ریپو:
pipx install .
```

### 🧪 بدون نصب

```bash
python3 proxy_rotator.py --once
```

> **نیازمندی:** فقط Python 3.8+ — هیچ پکیجی لازم نیست.
> `PySocks` فقط برای پراکسی‌های SOCKS اختیاری است (`--with-socks`).

---

## 🧠 چطور کار می‌کند

```text
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌────────────┐
│  SCRAPE  │──▶│  CHECK   │──▶│   SORT   │──▶│  ROTATE  │──▶│   REFILL   │
│ 106 srcs │   │ 400 thr  │   │  lowest  │   │  every   │   │ background │
│  ~31k IP │   │ real HTTP│   │  latency │   │   3 s    │   │    ⟲       │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └────────────┘
```

| مرحله | چه اتفاقی می‌افتد |
|:--:|---|
| **۱. دامپ** | ۱۰۶ منبع (GitHub raw، APIها، سایت‌های لیست) هم‌زمان دانلود و هر `ip:port` با regex استخراج و یکتا می‌شود |
| **۲. تست** | برای هر کاندید یک درخواست HTTP واقعی **از داخل خود پراکسی** به سرویس echo-IP زده می‌شود؛ زمان رفت‌وبرگشت = **پینگ واقعی** |
| **۳. فیلتر** | پراکسی مرده، خطادار، یا **شفاف** (که IP خودت را برمی‌گرداند) حذف می‌شود |
| **۴. چرخش** | هر ۳ ثانیه سریع‌ترین پراکسی استفاده‌نشده روی سیستم ست می‌شود، بدون تکرار |
| **۵. تازه‌سازی** | وقتی باقی‌مانده به آستانه برسد، در پس‌زمینه دوباره دامپ+تست می‌شود و لیست merge می‌شود |

> **چرا ICMP ping نه؟** باز بودن پورت یا جواب دادن ICMP هیچ تضمینی برای کارکردن پراکسی نیست.
> «پینگ» اینجا round-trip واقعی HTTP از مسیر پراکسی است.

---

## 🧰 استفاده

```bash
proxy-rotator                      # چرخش هر ۳ ثانیه
proxy-rotator --once               # فقط دامپ+تست، بدون تغییر تنظیمات
proxy-rotator --dry-run            # اجرای کامل بدون دست زدن به سیستم
proxy-rotator --interval 5 --min-pool 20 --refill-at 40
```

| گزینه | پیش‌فرض | توضیح |
|---|:--:|---|
| `--interval` | `3` | فاصله تعویض پراکسی (ثانیه) |
| `--timeout` | `6` | تایم‌اوت هر اتصال |
| `--threads` | `400` | کارگر موازی دامپ/تست |
| `--protocols` | `http,https` | نوع پراکسی‌ها (`http,https,socks4,socks5`) |
| `--max-check` | `2000` | حداکثر کاندید در هر دامپ |
| `--min-pool` | `5` | حداقل پراکسی سالم برای شروع |
| `--refill-at` | `15` | آستانه تازه‌سازی خودکار |
| `--refill-cooldown` | `15` | حداقل فاصله دو دامپ خودکار |
| `--backend` | `auto` | `gnome, windows, macos, adb, env, dry-run` |
| `--sources` | داخلی | فایل سفارشی منابع |
| `--state-dir` | `.` | محل فایل‌های خروجی |
| `--once` / `--top` | — | فقط یک‌بار اجرا / تعداد نتایج |
| `--allow-same-ip` | خاموش | پراکسی شفاف هم قبول شود |
| `--no-restore` | خاموش | در پایان پراکسی پاک نشود |
| `--list-sources` | — | چاپ ۱۰۶ منبع داخلی |

<details>
<summary><b>📄 ذخیره و ویرایش لیست منابع</b></summary>

```bash
proxy-rotator --list-sources > sources.txt
# هر خط:  <url>  <protocol>
proxy-rotator --sources sources.txt
```
</details>

---

## 📊 خروجی‌ها

| فایل | محتوا |
|---|---|
| `alive.txt` | پراکسی‌های سالم، مرتب بر اساس کمترین پینگ (`ip:port`) |
| `alive.csv` | همان + پروتکل + تأخیر + IP خروجی |
| `scraped.txt` | همه کاندیدهای خام |
| `current.txt` | پراکسی فعال فعلی |
| `rotation.log` | تاریخچه چرخش‌ها |

---

## 📱 ست کردن روی گوشی

```bash
adb devices                                   # USB Debugging روشن باشد
proxy-rotator --backend adb                   # اندروید
proxy-rotator --backend adb --adb-serial XXX  # چند دستگاه
```

در پایان با `Ctrl+C` خودکار پاک می‌شود. دستی:

```bash
adb shell settings put global http_proxy :0
```

<details>
<summary><b>🖥 بک‌اندهای دیگر</b></summary>

| سیستم | دستور | کاری که می‌کند |
|---|---|---|
| **GNOME** | `proxy-rotator --backend gnome` | `gsettings org.gnome.system.proxy` |
| **Windows** | `proxy-rotator --backend windows` | رجیستری WinINET + refresh |
| **macOS** | `proxy-rotator --backend macos --service "Wi-Fi"` | `networksetup` |
| **Env** | `proxy-rotator --backend env` | فقط `HTTP_PROXY` برای همین پروسه |
</details>

---

## 🗂 ساختار پروژه

```text
proxy-rotator/
├── proxy_rotator.py     # کل برنامه (تک‌فایل، stdlib only)
├── install.sh           # نصب‌کننده لینوکس / مک
├── install.ps1          # نصب‌کننده ویندوز
├── pyproject.toml       # نصب با pipx / pip
├── requirements.txt     # بدون وابستگی (PySocks اختیاری)
├── LICENSE              # MIT
└── README.md
```

---

## 🛡 نکات امنیتی

- پراکسی عمومی **ناشناس و غیرقابل‌اعتماد** است؛ گرداننده می‌تواند ترافیک HTTP را ببیند یا تغییر دهد.
- با پراکسی عمومی **لاگین نکن** و اطلاعات حساس نفرست.
- فقط روی شبکه‌ها و سرورهایی استفاده کن که **مجاز** هستی و قوانین سرویس‌دهنده را نقض نکن.
- برخی سایت‌ها استفاده از پراکسی را ممنوع کرده‌اند؛ مسئولیت رعایت آن‌ها با توست.
- این ابزار برای تست، پژوهش و استفاده شخصی است — **بدون هیچ ضمانتی**.

---

## 📫 Contact

<div align="center">

<a href="mailto:thenightghost@proton.me"><img src="https://img.shields.io/badge/ProtonMail-thenightghost%40proton.me-0d1117?style=for-the-badge&logo=protonmail&logoColor=00ff9d&labelColor=0d1117" /></a>
<a href="https://t.me/NulInetwork"><img src="https://img.shields.io/badge/Telegram-%40NulInetwork-0d1117?style=for-the-badge&logo=telegram&logoColor=00ff9d&labelColor=0d1117" /></a>
<a href="https://github.com/THEBLODYGHOSTS"><img src="https://img.shields.io/badge/GitHub-THEBLODYGHOSTS-0d1117?style=for-the-badge&logo=github&logoColor=00ff9d&labelColor=0d1117" /></a>

<br/><br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00ff9d,50:302b63,100:050510&height=120&section=footer" />

</div>
