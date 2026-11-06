# فرمت استاندارد اضافه کردن یک سایت به iran-mcp

این قالب همان قراردادی است که کاتالوگ امروز با آن ساخته می‌شود. منبع حقیقت `scripts/catalog.py` و `CONTRIBUTING.md` در ریپوی [sepehr071/iran-mcp](https://github.com/sepehr071/iran-mcp) است. `servers.json`، `TOOLS.md`، `.mcp.json`، `plugins/` و مهارت‌ها خروجی اسکریپت‌اند و با دست ویرایش نمی‌شوند.

یک سایت یک سرور است. سرور یک ریپوی جدا و یک بستهٔ پایتون است. این ریپو فقط آن را ثبت می‌کند.

دو راه برای پیوستن هست.

| راه | کی | کاربر چطور وصل می‌شود |
|---|---|---|
| بسته | سایت MCP رسمی ندارد و ابزارها فقط‌خواندنی‌اند | `uvx <name>-mcp` |
| میزبانی | خود شرکت سرور را اجرا می‌کند | آدرس HTTP، با OAuth یا بدون ورود |

سروری که روی حساب خود کاربر می‌نویسد (پرداخت، پیام، کیف پول) داخل باندل `iran-all` نمی‌آید. برای گسترش به سایت‌های معتبر، مسیر پیش‌فرض بستهٔ فقط‌خواندنی است.

## نام‌ها

| چیز | الگو | مثال |
|---|---|---|
| ریپو و پوشهٔ ساب‌ماژول | `<name>-mcp` | `digikala-mcp` |
| بستهٔ PyPI و دستور اجرا | همان نام | `uvx digikala-mcp` |
| کلید پلاگین | نام بدون `-mcp` | `digikala` |
| پیشوند ابزار | ۲ یا ۳ حرف، یکتا | `dk_` ، `qv_` |
| پراکسی | `<NAME>_MCP_PROXY` | `DIGIKALA_MCP_PROXY` |
| رجیستری | `io.github.<owner>/<name>-mcp` | `io.github.sepehr071/digikala-mcp` |
| نسخه | یکسان در `pyproject.toml` و `server.json` | `0.1.0` |
| لایسنس | MIT | |

دسته را از دسته‌های موجود انتخاب کنید: `food` ، `shopping` ، `beauty` ، `fashion` ، `classifieds` ، `cars` ، `travel` ، `maps` ، `health` ، `books` ، `learning` ، `jobs` ، `freelance` ، `software` ، `cloud` ، `prices` ، `stocks` ، `economy` ، `news` ، `sports` ، `entertainment` ، `video` ، `calendar` ، `quakes` ، `law` ، `tax`. دستهٔ تازه فقط وقتی لازم است که هیچ‌کدام معنی را نرساند؛ آن وقت علاوه بر `SERVERS` باید `USE` در `catalog.py` و `GROUPS` در `scripts/banner.py` هم پر شود.

## درختی که شرکت یا فرد می‌سازد

```text
<name>-mcp/
  pyproject.toml
  server.json
  README.md          # شامل <!-- mcp-name: io.github.<owner>/<name>-mcp -->
  LICENSE
  src/<name>_mcp/
    __init__.py      # __version__
    registry.py
    server.py
    http.py
    <domain>.py      # یک یا چند ماژول ابزار
  tests/
    conftest.py
    fixtures/
    test_*.py
  .github/workflows/ # انتشار با برچسب v*
```

`plugins/` و `SKILL.md` کاتالوگ را اینجا نسازید. `catalog.py` از روی کد و `server.json` آن‌ها را می‌سازد. اگر ریپوی سرور خودش `skills/<key>/SKILL.md` داشته باشد، همان متنِ قوانین جایگزین قوانین پیش‌فرض می‌شود و باید همچنان فقط‌خواندنی بودن، یا تأیید قبل از هر نوشتن، و محدودیت نرخ را بگوید.

## قرارداد ابزار

هر ابزار یک تابع `async` است با دکوراتور `@tool("عنوان انسانی")`. `catalog.py` با خواندن AST این‌ها را جمع می‌کند:

- نام ابزار همان نام تابع است. پیشوند سایت را در نام تابع بگذارید: `dk_search` نه `search`.
- آرگومان اول `@tool` یک رشتهٔ ثابت است. این عنوان انسانی است.
- خط اول docstring خلاصهٔ یک‌خطی است. همان خط در `TOOLS.md` و مهارت می‌آید، پس باید تنها و قابل‌فهم باشد.
- قیمت، اگر سایت ریال بدهد، قبل از برگشت به تومان تبدیل می‌شود (تقسیم بر ۱۰). اگر واحد چیز دیگری است، در docstring و دستورالعمل سرور گفته می‌شود.
- خروجی یک `dict` است. `server.py` آن را یک JSON فشرده می‌کند (`separators=(",", ":")`) و `structured_output=False` می‌گذارد تا متن دو بار به مدل نرسد.
- ابزار فقط‌خواندنی `ToolAnnotations(read_only_hint=True, destructive_hint=False, idempotent_hint=True, open_world_hint=True)` می‌گیرد.
- متن صفحه، نظر، قانون و عنوان سایت داده است. دستورالعمل سرور باید بگوید مدل از آن دستور نگیرد.
- کار نیمه‌تمام با لینک خود سایت تمام می‌شود: بدون ورود، سبد، رزرو، سفارش و پرداخت.

ابزار را آینهٔ یک endpoint نسازید. شکلی بسازید که مدل بدون دانستن API سایت بتواند صدا بزند: جستجو، ارزان‌ترین، یک قلم با شناسه، و در صورت وجود تاریخچه یا موجودی.

```python
from .registry import tool

@tool("Search")
async def ex_search(query: str, page: int = 1) -> dict:
    """Search in-stock items by name. Prices are Toman, 20 per page."""
    ...
```

`registry.py` در همهٔ سرورها همین است: لیست `TOOLS` و دکوراتوری که `(تابع، عنوان)` را به آن اضافه می‌کند. `server.py` ماژول‌های دامنه را وارد می‌کند تا ثبت انجام شود، بعد همان لیست را روی `MCPServer` می‌گذارد. دستورالعمل سرور (`INSTRUCTIONS`) مسیر را می‌گوید: اول کدام ابزار، شناسه چه شکلی است، واحد پول چیست.

## قرارداد HTTP

همهٔ ابزارها از یک کلاینت در `http.py` عبور می‌کنند.

- اثرانگشت TLS مرورگر با `curl_cffi` (`impersonate="chrome"`). وابسته‌ها: `mcp>=2.2,<3` ، `httpx` ، `httpx-curl-cffi` ، `curl-cffi`. پایتون `>=3.10`.
- `trust_env=False`. پراکسی سیستم نادیده گرفته می‌شود. فقط `<NAME>_MCP_PROXY` ، یک آدرس یا چند آدرس جدا شده با ویرگول.
- همزمانی حداکثر ۲. قبل از درخواست بدون کش، تأخیر تصادفی کوتاه.
- ریتری فقط برای قطع شبکه، ۵xx و ۴۲۹، با سقف مشخص. ۴۰۳ دوباره زده نمی‌شود.
- خطای بالادست `ToolError` است تا مدل پیام را ببیند، نه یک استک داخلی.
- کش کوتاه فقط برای پاسخی که کسی آن را تغییر نمی‌دهد.
- تست پیش‌فرض آفلاین است و ترنسپورت ساختگی دارد (`set_transport`). تست سایت واقعی مارکر `live` دارد و در `addopts` حذف شده: `-m 'not live'`.

## `server.json`

اسکیما: `https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json`

فیلدهایی که کاتالوگ واقعاً می‌خواند: `name` ، `title` ، `description` ، `version` ، `repository.url` ، و برای بسته `packages[0].identifier` به‌همراه `environmentVariables` (هر کدام `name` ، `isRequired` ، `isSecret` ، `description`). `version` بسته با `version` فایل یکی است.

اگر هیچ متغیر محیطی اجباری نباشد، کاتالوگ سرور را فقط‌خواندنی حساب می‌کند و آن را در `iran-all` می‌گذارد. توکن اجباری یعنی سرور روی حساب کاربر کار می‌کند و از باندل‌ها بیرون می‌ماند. پراکسی اختیاری است و این پرچم را عوض نمی‌کند.

```json
{
  "name": "io.github.OWNER/example-mcp",
  "title": "Example",
  "description": "Unofficial read-only Example: search items and read a page.",
  "repository": { "url": "https://github.com/OWNER/example-mcp", "source": "github" },
  "version": "0.1.0",
  "packages": [
    {
      "registryType": "pypi",
      "registryBaseUrl": "https://pypi.org",
      "identifier": "example-mcp",
      "version": "0.1.0",
      "runtimeHint": "uvx",
      "transport": { "type": "stdio" },
      "environmentVariables": [
        {
          "name": "EXAMPLE_MCP_PROXY",
          "description": "Optional HTTP proxy URL, or a comma-separated list. System proxy settings are ignored.",
          "isRequired": false,
          "isSecret": false
        }
      ]
    }
  ]
}
```

انتشار با برچسب `v*` از CI است: PyPI با trusted publishing و attestation، رجیستری MCP، و GitHub Release. نسخه را دستی روی PyPI نگذارید. نسخهٔ منتشرشده را نمی‌شود جایگزین کرد؛ کاتالوگ همان نسخهٔ بررسی‌شده را پین می‌کند (`uvx example-mcp@0.1.0`).

## سروری که خود شرکت میزبانی می‌کند

به‌جای `packages` ، `server.json` فیلد `remotes` دارد و کاتالوگ `remotes[0].url` را برمی‌دارد. کنار همان مبدأ باید این کارت زنده باشد:

`<origin>/.well-known/mcp/server-card.json`

کارت `tools` را با `name` ، `title` ، `description` و `annotations.readOnlyHint` دارد، و `authentication` را. اگر `authentication.required` درست باشد، تنها طرح مجاز `oauth2` است. ورود دیگری کاتالوگ را متوقف می‌کند. ابزاری که چیزی را عوض می‌کند باید بیرون از عامل تأیید کاربر را بگیرد.

## ثبت در کاتالوگ iran-mcp

بعد از انتشار `v0.1.0`:

1. در ریپوی سرور issue باز شود: لینک ریپو، لینک PyPI، سایت، دسته، و کار ابزارها.
2. اگر ریپو زیر `sepehr071` نیست، نگه‌دارنده یک فورک `sepehr071/<name>-mcp` می‌سازد. ساب‌ماژول به فورک اشاره می‌کند، نه به ریپوی نویسنده. نویسنده ریپو و بستهٔ PyPI خودش را نگه می‌دارد.
3. در فورک کاتالوگ: `git submodule add https://github.com/sepehr071/<name>-mcp.git servers/<name>-mcp`
4. یک خط در `SERVERS` داخل `scripts/catalog.py`: `"<name>-mcp": ("<category>", "https://site.example")`. سرور میزبانی به‌جای این در `REMOTE` ثبت می‌شود.
5. `python scripts/catalog.py && python scripts/banner.py`
6. اگر رفتار خاصی هست (واحد غیر از تومان، کندی، فقط IP ایران) یک خط در `NOTES` همان فایل.
7. ردیف در جدول دسته در `README.md` و `README.fa.md` ، یک ردیف در جدول «Pick a server» در `AGENTS.md` ، و یک خط در `llms.txt`. عدد سرور و ابزار در هر دو README با `servers.json` یکی باشد.
8. PR با Conventional Commits. یک موضوع در هر PR.

نگه‌دارنده قبل از رسیدن نسخه به کاربر این‌ها را می‌بیند: بازسازی تمیز کاتالوگ چیزی را عوض نکند، `ruff` و تست آفلاین و تست `live` در همان کامیت پین‌شده سبز باشد، نوشتن پنهان و راز در کد نباشد، نسخه روی PyPI با attestation باشد، و ساب‌ماژول به ریپوی `sepehr071` اشاره کند. برای سرور رسمی، خود سایت باید به ریپو لینک بدهد.

## برگهٔ پیشنهاد، پیش از نوشتن کد

این را می‌شود برای یک شرکت یا یک داوطلب فرستاد. با پر شدنش معلوم می‌شود سایت جا دارد یا نه.

| مورد | مقدار |
|---|---|
| سایت و نشانی | |
| دسته | |
| رسمی است یا غیررسمی | |
| نام بسته | `<name>-mcp` |
| پیشوند ابزار | |
| ابزارها، هر کدام یک خط | جستجو، یک قلم، … |
| فقط‌خواندنی است | بله، مگر توکن اجباری |
| واحد پول یا تاریخ | تومان، تاریخ جلالی اگر هست |
| IP ایران لازم است | |
| از قبل در کاتالوگ هست | خیر، یا نام سرور موجود |

ابزار پیشنهادی را با ابزار سرور موجود همان دسته مقایسه کنید. اگر سایت فقط همان داده را دوباره می‌دهد، سرور تازه لازم نیست.

## پرامپ

برای کسی که فقط اسم سایت را دارد، متن آماده در [prompt.md](prompt.md) است. همان را به هوش مصنوعی می‌دهد. مدل سرور را با همین ساختار می‌سازد و به https://github.com/alizare84/iran-mcp پول‌ریکوئست می‌فرستد.
