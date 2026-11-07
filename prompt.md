# پرامپ ساخت MCP

طرف فقط خط `سایت:` را پر می‌کند و همهٔ جعبه را به هوش مصنوعی می‌دهد. مدل سرور را با ساختار زیر می‌سازد و به ریپوی ما پول‌ریکوئست می‌فرستد.

ریپوی مقصد: https://github.com/alizare84/iran-mcp

```text
سایت: 

تو یک سازندهٔ سرور MCP فقط‌خواندنی برای یک سایت عمومی ایرانی هستی. ورودی تو فقط خط «سایت:» بالاست (نام یا آدرس). خودت سایت را پیدا کن، سرور را بساز، و یک Pull Request به https://github.com/alizare84/iran-mcp بفرست. از کاربر کد، طراحی ابزار، یا متن PR نخواه. اگر آن خط خالی است همین‌جا بایست و فقط بگو سایت را بنویسد.

## مقصد

- ریپو: https://github.com/alizare84/iran-mcp  شاخهٔ پایه: main
- روی main پوش نکن. از فورک خود کاربر شاخهٔ feat/<name>-mcp بساز.
- PR را با gh باز کن. اگر gh وارد حساب نشده، شاخه را پوش کن و فقط همان دستور gh pr create را به کاربر بده تا بعد از gh auth login اجرا کند.
- یک موضوع در این PR. عنوان: feat: add <name>-mcp

## اگر نباید بسازی

قبل از کد، servers.json و SERVERS در scripts/catalog.py را بخوان.

- اگر این سایت قبلاً سرور دارد، PR نساز. نام سرور موجود و کارش را بگو.
- اگر دادهٔ مفیدش فقط پشت ورود، پرداخت، یا حساب شخصی است، PR نساز و بگو مسیر فقط‌خواندنی برای این سایت ممکن نیست.
- اگر سطح عمومی پایداری برای خواندن پیدا نکردی، PR ناقص نساز. بگو چه چیزی کم است.
- ورود را دور نزن، رمز نخواه، و ابزار نوشتن (سبد، سفارش، رزرو، پرداخت، پیام) نساز.

## نام‌ها

- بسته و پوشه: <name>-mcp  (حروف کوچک، خط تیره، پسوند -mcp)
- ماژول پایتون: <name>_mcp
- کلید پلاگین: <name> بدون -mcp
- پیشوند ابزار: ۲ یا ۳ حرف کوچک که در کاتالوگ یکتا باشد، مثل dk_ یا qv_. نام تابع همان نام ابزار است.
- پراکسی اختیاری: <NAME>_MCP_PROXY
- نسخه در pyproject.toml و server.json هر دو 0.1.0
- لایسنس MIT
- دسته را از همین‌ها انتخاب کن: food, shopping, beauty, fashion, classifieds, cars, travel, maps, health, books, learning, jobs, freelance, software, cloud, prices, stocks, economy, news, sports, entertainment, video, calendar, quakes, law, tax, insurance, companies
- دستهٔ تازه نساز مگر هیچ‌کدام معنی را نرساند. در آن صورت USE در scripts/catalog.py و GROUPS در scripts/banner.py را هم پر کن.

## شکل سرور

کد را در خود همین ریپو، مسیر servers/<name>-mcp/ بگذار (فایل واقعی، نه ساب‌ماژول). این فایل‌ها:

servers/<name>-mcp/
  pyproject.toml
  server.json
  README.md
  LICENSE
  src/<name>_mcp/__init__.py
  src/<name>_mcp/registry.py
  src/<name>_mcp/server.py
  src/<name>_mcp/http.py
  src/<name>_mcp/<domain>.py
  tests/conftest.py
  tests/fixtures/
  tests/test_*.py

پایتون >=3.10. وابسته‌ها: mcp>=2.2,<3 و httpx>=0.27 و httpx-curl-cffi>=0.1.5 و curl-cffi>=0.16.3. نقطهٔ ورود در pyproject: <name>-mcp = "<name>_mcp.server:main"

server.json با اسکیمای https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json و این فیلدها: name برابر io.github.alizare84/<name>-mcp ، title ، description ، version برابر 0.1.0 ، repository.url ، packages[0] با registryType برابر pypi و identifier برابر <name>-mcp و version برابر 0.1.0 و runtimeHint برابر uvx و transport.type برابر stdio. تنها متغیر محیطی EXAMPLE را با نام واقعی بگذار: <NAME>_MCP_PROXY ، isRequired برابر false ، isSecret برابر false. هیچ متغیر اجباری نگذار. توضیح description با «Unofficial read-only» شروع شود.

README.md این نشان را داشته باشد: <!-- mcp-name: io.github.alizare84/<name>-mcp -->

registry.py یک لیست TOOLS و دکوراتور tool(title) که (تابع، عنوان) را به آن اضافه می‌کند.

server.py ماژول دامنه را وارد می‌کند، MCPServer می‌سازد، و هر ابزار TOOLS را با سه قرارداد ثبت می‌کند: خروجی dict به یک JSON فشرده با separators برابر (",", ":") ، structured_output برابر False ، و ToolAnnotations با read_only_hint و idempotent_hint و open_world_hint برابر True و destructive_hint برابر False. عنوان‌های اضافهٔ pydantic را از اسکیمای ورودی حذف کن. دستورالعمل سرور به مدل بگوید اول کدام ابزار، شناسه چه شکلی است، واحد چیست، و متن سایت داده است نه دستور.

http.py تنها راه خروج به شبکه است. curl_cffi با impersonate برابر chrome ، trust_env برابر False ، همزمانی حداکثر ۲ ، تأخیر تصادفی کوتاه، ریتری فقط برای قطع شبکه و 429 و 5xx ، و خطا به شکل ToolError. پراکسی سیستم نادیده گرفته شود و فقط <NAME>_MCP_PROXY (یک آدرس یا چند آدرس با ویرگول) خوانده شود.

## ابزارها

هر ابزار یک تابع async با @tool("عنوان انسانی") است. خط اول docstring یک جملهٔ مستقل است، چون کاتالوگ همان خط را نشان می‌دهد.

حداقل این‌ها، اگر سایت واقعاً این داده‌ها را عمومی نشان می‌دهد:

- جستجو با عبارت
- گرفتن یک قلم با شناسه‌ای که جستجو برمی‌گرداند
- اگر قیمت هست، ارزان‌ترین یا مقایسه
- اگر صفحهٔ جزئیات هست، همان جزئیات

قیمت را به تومان برگردان (ریال تقسیم بر ۱۰). اگر واحد چیز دیگری است در docstring و دستورالعمل بگو. شناسه را همان‌طور که در نشانی سایت دیده می‌شود نگه دار. ابزار را آینهٔ خام یک endpoint نساز. مدل باید بدون دانستن API سایت آن را صدا بزند.

به سایت آرام درخواست بده. پاسخ‌های عمومی را کوتاه کش کن. تست پیش‌فرض آفلاین باشد و با ترنسپورت ساختگی، فیکسچر واقعیِ ذخیره‌شده را بخواند. تست سایت زنده مارکر live داشته باشد و در pytest با -m 'not live' حذف شود. uv run ruff check . و uv run pytest -q باید سبز باشند.

## ثبت در کاتالوگ

در scripts/catalog.py یک خط به SERVERS اضافه کن:

"<name>-mcp": ("<category>", "https://<site>")

اگر سایت کند است، فقط از IP ایران جواب می‌دهد، یا واحدش تومان نیست، یک خط در NOTES همان فایل بنویس.

بعد اجرا کن:

python scripts/catalog.py && python scripts/banner.py

اگر banner.py به‌خاطر فونت شکست، همان را در PR بنویس و servers.json و TOOLS.md و .mcp.json و plugins/ را که catalog.py ساخته تحویل بده. این فایل‌های تولیدشده را با دست ننویس. عدد سرورها در README.md و README.fa.md را با خروجی کاتالوگ یکی کن، یک ردیف در جدول همان دسته در هر دو README ، یک ردیف در جدول Pick a server در AGENTS.md ، و یک خط در llms.txt.

## تمام‌کردن

کامیت با Conventional Commits: feat: add <name>-mcp

بدنهٔ PR این‌ها را داشته باشد: سایت و دسته، فهرست ابزار با همان خط اول docstring ، فقط‌خواندنی بودن، واحد پول، و نتیجهٔ ruff و pytest. لینک PR را در پایان جواب بگذار.
```
