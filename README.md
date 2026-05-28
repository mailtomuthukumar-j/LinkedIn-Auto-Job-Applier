# ⚡ LinkedIn Auto Job Applier

Automatically apply to LinkedIn **Easy Apply** jobs using a **Web UI** or **Direct Bot Mode** — no manual config file editing needed.

> **Created by:** Muthukumar  
> **Original bot:** [Sai Vignesh Golla](https://github.com/GodsScion/Auto_job_applier_linkedIn)  
> **Web UI layer & enhancements:** Muthukumar

---

## 📋 Features

- ✅ **Web UI Dashboard** — Fill everything from your browser (no file editing)
- ✅ **Easy Apply Automation** — Auto-clicks and fills Easy Apply forms
- ✅ **Smart Form Filling** — Handles text, dropdown, radio, checkbox, textarea questions
- ✅ **AI Answering** — Uses OpenAI / DeepSeek / Gemini for unknown questions
- ✅ **Skill Extraction** — AI extracts required skills from job descriptions
- ✅ **Filter System** — Blacklist companies, bad words, experience level, salary
- ✅ **Job History** — CSV tracking of applied & failed jobs with full details
- ✅ **Screenshots on Error** — Captures screenshots for debugging failures
- ✅ **24/7 Mode** — Can run non-stop with auto-cycling of search filters
- ✅ **Stealth Mode** — Undetected ChromeDriver to bypass bot detection

---

## 🚀 Quick Start (2 Minutes)

### 1. Install Requirements

```bash
pip install flask flask-cors selenium pyautogui undetected-chromedriver
```

For AI features, also install:

```bash
pip install openai google-generativeai
```

### 2. Add Your Resume

Place your PDF resume at:

```
all resumes/default/resume.pdf
```

### 3. Run the Web UI

```bash
python app.py
```

### 4. Open Browser

Go to: **http://localhost:5000**

Fill in your details across the tabs and click **⚡ Apply for Jobs**.

---

## 📁 Project Structure

```
├── app.py                  ← 🔴 RUN THIS (Flask Web Server)
├── runAiBot.py             ← Bot engine (auto-launched by app.py or run directly)
├── GUIDE.md                ← Detailed step-by-step usage guide
├── templates/
│   └── index.html          ← Web UI page (dark theme)
│
├── config/                 ← ⚙️ All configuration files
│   ├── secrets.py          ← LinkedIn login + AI settings
│   ├── personals.py        ← Your personal info (name, phone, address)
│   ├── search.py           ← Job search terms & filters
│   ├── questions.py        ← Application form answers (salary, visa, etc.)
│   └── settings.py         ← Bot behavior (stealth, speed, run mode)
│
├── modules/
│   ├── ai/                 ← AI integrations (OpenAI, DeepSeek, Gemini)
│   ├── open_chrome.py      ← Chrome browser session creator
│   ├── helpers.py          ← Logging, date parsing, file utilities
│   ├── clickers_and_finders.py  ← Selenium interaction helpers
│   ├── validator.py        ← Config validation (type/range checks)
│   ├── resumes/            ← Resume extraction & generation
│   └── images/             ← UI reference images for button detection
│
├── all resumes/default/    ← 📄 Put your resume PDF here
├── all excels/             ← 📊 Job history CSVs (auto-created)
└── logs/                   ← 📝 Logs & screenshots (auto-created)
```

---

## 🖥️ Two Ways to Run

### Method A: Web UI Mode (Recommended)

```bash
python app.py
# Then open http://localhost:5000
```

The Web UI has **5 tabs**:

| Tab | What to Fill |
|-----|-------------|
| 🔐 Login | LinkedIn email & password |
| 👤 Profile | Name, phone, city, salary, portfolio URL |
| 🔍 Job Search | Job titles, location, filters, bad words |
| 🚀 Launch Bot | Click to start + live log viewer |
| 📋 History | View all previously applied jobs |

When you click **Apply for Jobs**, the UI saves everything to `config/*.py` and launches the bot in background.

---

### Method B: Direct Bot Mode (Edit Files & Run)

Edit config files manually, then run the bot directly:

```bash
python runAiBot.py
```

The bot will:
1. Validate all config files
2. Open Chrome (stealth/normal mode)
3. Log into LinkedIn
4. Search & filter jobs
5. Auto-apply to Easy Apply jobs
6. Save results to CSV

---

## ⚙️ Config Files — What Goes Where

### `config/secrets.py` — Login & AI

| Field | Example |
|-------|---------|
| `username` | `"your.email@gmail.com"` |
| `password` | `"yourLinkedInPassword"` |
| `use_AI` | `True` or `False` |
| `ai_provider` | `"openai"`, `"deepseek"`, or `"gemini"` |
| `llm_api_key` | `"sk-..."` |

### `config/personals.py` — Your Info

| Field | Example |
|-------|---------|
| `first_name` / `last_name` | `"Muthukumar"` / `"J"` |
| `phone_number` | `"9876543210"` |
| `current_city` / `state` | `"Chennai"` / `"Tamil Nadu"` |
| `country` | `"India"` |
| `gender` | `"Male"`, `"Female"`, or `"Decline"` |

### `config/search.py` — Job Filters

| Field | Example |
|-------|---------|
| `search_terms` | `["Python Developer", "Full Stack"]` |
| `search_location` | `"India"` |
| `experience_level` | `["Entry level", "Associate"]` |
| `job_type` | `["Full-time", "Contract"]` |
| `on_site` | `["Remote", "Hybrid", "On-site"]` |
| `date_posted` | `"Past week"` or `"Past 24 hours"` |
| `bad_words` | `["Senior", "C++", "10+ years"]` |
| `easy_apply_only` | `True` |

### `config/questions.py` — Application Answers

| Field | Example |
|-------|---------|
| `default_resume_path` | `"all resumes/default/resume.pdf"` |
| `years_of_experience` | `"2"` |
| `require_visa` | `"Yes"` or `"No"` |
| `desired_salary` | `600000` (6 LPA, in rupees) |
| `notice_period` | `30` (days) |
| `pause_before_submit` | `True` |

### `config/settings.py` — Bot Behavior

| Field | Recommended |
|-------|-------------|
| `stealth_mode` | `True` (bypass detection) |
| `safe_mode` | `True` (if Chrome has issues) |
| `run_in_background` | `False` (see browser) |
| `click_gap` | `1` (delay between clicks) |
| `keep_screen_awake` | `True` |
| `close_tabs` | `True` |

> 🔍 **Full details** for every single field → see [`GUIDE.md`](GUIDE.md) Section 5

---

## 🤖 AI Integration

The bot can use AI to answer questions it doesn't understand and extract skills from job descriptions.

### Setup

In `config/secrets.py`:

```python
use_AI = True
ai_provider = "openai"       # or "deepseek" or "gemini"
llm_api_key = "your-api-key"
llm_model = "gpt-4o-mini"   # or "deepseek-chat" or "gemini-1.5-flash"
```

### What AI Does

| Task | Description |
|------|-------------|
| Answer text questions | "How many years of experience do you have in React?" |
| Answer textarea questions | "Tell us why you're a good fit" — writes cover letters |
| Extract skills | Reads job description and lists required skills |
| Handle unknown fields | Falls back to AI when no config answer matches |

---

## 📄 Resume Setup

```
all resumes/
└── default/
    └── resume.pdf    ← Your PDF file here (exact name)
```

If missing, the bot warns you and uses your previously uploaded LinkedIn resume.

---

## 📊 Monitoring

### Live Logs
```
logs/log.txt           ← Full bot activity log
logs/screenshots/      ← Error screenshots
```

### CSV History
```
all excels/all_applied_applications_history.csv   ← Successful applications
all excels/all_failed_applications_history.csv    ← Failed/skipped jobs
```

### Web UI Logs
Go to **Launch Bot** tab → click **"Refresh Logs"**.

---

## 🔧 Quick Troubleshooting

| Problem | Fix |
|---------|-----|
| Chrome won't open | Set `safe_mode = True` in `config/settings.py` |
| Bot detection | Set `stealth_mode = True` in `config/settings.py` |
| Login fails | Check credentials or login manually |
| No jobs found | Broaden `search_terms` or `date_posted` |
| AI not working | Check `llm_api_key` is correct |
| Daily limit | LinkedIn restricts Easy Apply — wait 24h |
| Port 5000 in use | Edit `app.py` line 185: `port=5001` |

---

## 💡 Pro Tip: Quick Config via AI

Instead of manually filling every config field, do this:

1. Open all files in `config/` folder
2. Copy-paste them into **ChatGPT** or **Claude AI**
3. Tell the AI: *"Here are my details: [your name, experience, skills, salary, etc.]. Fill all these config files for me."*
4. The AI returns fully filled files — just copy-paste back

Takes **2 minutes** instead of 20.

---

## ⚠️ Important Notes

- Your LinkedIn credentials are **never sent anywhere** — stored only in local `config/secrets.py`
- This tool is for **personal use only**
- LinkedIn may temporarily restrict accounts using automation — **use responsibly**
- Make sure **Google Chrome** is installed before running

---

## 🛠️ Requirements

| Software | Version |
|----------|---------|
| Python | 3.8+ |
| Google Chrome | Latest |
| pip packages | flask, flask-cors, selenium, pyautogui, undetected-chromedriver |

---

## 📜 License

GNU Affero General Public License v3.0 — see [LICENSE](LICENSE).

---

## 🙏 Credits

- **Original Author:** [Sai Vignesh Golla](https://github.com/GodsScion/Auto_job_applier_linkedIn) — Built the core bot engine
- **Web UI & Enhancements:** [Muthukumar](https://github.com/mailtomuthukumar-j) — Added Flask dashboard, improved config workflow, documentation
- **Contributors:** Dheeraj Deshwal, Yang Li, Tim L, and the open-source community
