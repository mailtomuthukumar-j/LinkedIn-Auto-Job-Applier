# LinkedIn Auto Job Applier — Complete Setup & Usage Guide

> **Language:** English  
> **Purpose:** Step-by-step guide to set up, configure, and run the LinkedIn Auto Job Applier bot

---

## Table of Contents

1. [What Is This Project?](#1-what-is-this-project)
2. [System Requirements](#2-system-requirements)
3. [Installation](#3-installation)
4. [Project Structure Overview](#4-project-structure-overview)
5. [Configuration Files — Every Field Explained](#5-configuration-files--every-field-explained)
   - [config/secrets.py](#51-configsecretspy)
   - [config/personals.py](#52-configpersonalspy)
   - [config/search.py](#53-configsearchpy)
   - [config/questions.py](#54-configquestionspy)
   - [config/settings.py](#55-configsettingspy)
6. [How to Run](#6-how-to-run)
7. [Web UI Walkthrough (Tab by Tab)](#7-web-ui-walkthrough-tab-by-tab)
8. [AI Integration Setup](#8-ai-integration-setup)
9. [Resume Setup](#9-resume-setup)
10. [Monitoring & Logs](#10-monitoring--logs)
11. [Troubleshooting](#11-troubleshooting)
12. [Quick Config via AI (ChatGPT / Claude)](#12-quick-config-via-ai-chatgpt--claude)

---

## 1. What Is This Project?

This is an automated bot that:
- Searches LinkedIn for jobs matching your criteria
- Applies **Easy Apply** jobs automatically
- Fills application forms (text, dropdown, radio, checkbox, textarea)
- Uses **AI (OpenAI / DeepSeek / Gemini)** to answer unknown questions
- Tracks all applied & failed jobs in CSV files
- Comes with a **Web UI** (Flask) so you don't need to edit files manually

---

## 2. System Requirements

| Requirement | Details |
|-------------|---------|
| **OS** | Windows 10/11, macOS, or Linux |
| **Python** | 3.8 or higher |
| **Google Chrome** | Latest version installed |
| **Internet** | Required for LinkedIn access |
| **LinkedIn Account** | Active account (use at your own risk) |

---

## 3. Installation

### Step 1: Install Python packages

Open terminal in the project folder and run:

```bash
pip install flask flask-cors selenium pyautogui undetected-chromedriver
```

If you want AI features, also install:

```bash
pip install openai           # For OpenAI
pip install google-generativeai  # For Google Gemini
```

### Step 2: Add your resume

Place your resume PDF at:

```
all resumes/default/resume.pdf
```

Make sure the filename is exactly `resume.pdf`.

### Step 3 (Optional): Run setup scripts

If you're on Windows, double-click:
- `setup/windows-setup.bat` (or)
- Right-click `setup/windows-setup.ps1` → "Run with PowerShell"

If you're on Linux/Mac:

```bash
bash setup/setup.sh
```

---

## 4. Project Structure Overview

```
Auto_job_applier_linkedIn-FINAL/
│
├── app.py                  ← 🔴 RUN THIS (Flask Web Server)
├── runAiBot.py             ← Bot engine (auto-launched by app.py)
├── GUIDE.md                ← This file
├── README.md               ← Original readme
│
├── config/                 ← ⚙️ ALL CONFIGURATION FILES
│   ├── secrets.py          ← LinkedIn login + AI settings
│   ├── personals.py        ← Your personal info
│   ├── search.py           ← Job search filters
│   ├── questions.py        ← Application form answers
│   └── settings.py         ← Bot behavior settings
│
├── templates/
│   └── index.html          ← Web UI page
│
├── modules/                ← Bot logic modules
│   ├── ai/                 ← AI integrations
│   ├── open_chrome.py      ← Chrome browser control
│   ├── helpers.py          ← Logging, date, file helpers
│   ├── clickers_and_finders.py ← Selenium interaction helpers
│   └── validator.py        ← Config validation
│
├── all resumes/default/    ← 📄 Put your resume PDF here
├── all excels/             ← 📊 Job history CSVs (auto-created)
└── logs/                   ← 📝 Log files (auto-created)
```

---

## 5. Configuration Files — Every Field Explained

You can configure the bot in two ways:
- **Option A:** Use the Web UI (recommended — no file editing needed)
- **Option B:** Edit the `config/*.py` files directly

Below is every field explained so you know exactly what to put where.

---

### 5.1 `config/secrets.py`

| Field | What to Put | Example |
|-------|-------------|---------|
| `username` | Your LinkedIn email/phone | `"your.email@gmail.com"` |
| `password` | Your LinkedIn password | `"yourPassword123"` |
| `use_AI` | Enable AI to answer questions? | `True` or `False` |
| `ai_provider` | Which AI service? | `"openai"`, `"deepseek"`, or `"gemini"` |
| `llm_api_url` | API base URL | `"https://api.openai.com/v1/"` |
| `llm_api_key` | Your API key | `"sk-..."` |
| `llm_model` | Model name | `"gpt-4o-mini"` or `"deepseek-chat"` |
| `stream_output` | Stream AI output? | `False` |

> ⚠️ Your credentials are stored **only locally** on your machine.

---

### 5.2 `config/personals.py`

| Field | What to Put | Example |
|-------|-------------|---------|
| `first_name` | Your first name | `"Muthukumar"` |
| `middle_name` | Middle name (leave blank if none) | `""` |
| `last_name` | Your last/family name | `"J"` |
| `phone_number` | Your mobile number with country code | `"9876543210"` |
| `current_city` | City you live in | `"Chennai"` |
| `street` | Street address | `"12, Main Road"` |
| `state` | Your state | `"Tamil Nadu"` |
| `zipcode` | PIN/ZIP code | `"600001"` |
| `country` | Country name | `"India"` |
| `ethnicity` | Ethnicity (optional) | `"Decline"` or `"Asian"` etc. |
| `gender` | Gender | `"Male"`, `"Female"`, or `"Decline"` |
| `disability_status` | Disability status | `"No"`, `"Yes"`, or `"Decline"` |
| `veteran_status` | Veteran status | `"No"`, `"Yes"`, or `"Decline"` |

---

### 5.3 `config/search.py`

This file controls **what jobs** to search for and **how** to filter them.

| Field | What to Put | Example |
|-------|-------------|---------|
| `search_terms` | List of job titles/keywords to search | `["Python Developer", "Full Stack", "React"]` |
| `search_location` | Location to search in | `"India"` or `"Chennai"` |
| `switch_number` | Max jobs to apply per search term | `20` |
| `randomize_search_order` | Shuffle search terms? | `False` |
| `sort_by` | Sort results by | `"Most recent"` or `"Most relevant"` |
| `date_posted` | How old can the job post be? | `"Past 24 hours"`, `"Past week"`, `"Past month"`, `"Any time"` |
| `salary` | Salary filter (leave blank for any) | `""` |
| `easy_apply_only` | Only Easy Apply jobs? | `True` |
| `experience_level` | Which levels to include | `["Entry level", "Associate"]` |
| `job_type` | Employment types | `["Full-time", "Contract"]` |
| `on_site` | Work mode | `["Remote", "Hybrid", "On-site"]` |
| `companies` | Filter by specific companies (empty = all) | `[]` |
| `location` | Filter by company location | `[]` |
| `industry` | Filter by industry | `[]` |
| `job_function` | Filter by job function | `[]` |
| `job_titles` | Filter by exact titles | `[]` |
| `benefits` | Filter by benefits | `[]` |
| `commitments` | Filter by commitments | `[]` |
| `under_10_applicants` | Only jobs with <10 applicants? | `False` |
| `in_your_network` | Only jobs in your network? | `False` |
| `fair_chance_employer` | Only fair chance employers? | `False` |
| `pause_after_filters` | Pause to review filters before applying? | `True` (recommended) |
| `about_company_bad_words` | Skip companies with these words in "About" | `["Crossover"]` |
| `about_company_good_words` | Force-apply even if bad words exist | `[]` |
| `bad_words` | Skip jobs whose description contains these | `["Senior", "Lead", "C++", "10+ years"]` |
| `security_clearance` | Skip jobs asking clearance? | `False` |
| `did_masters` | Do you have a Master's degree? | `False` |
| `current_experience` | Your years of experience | `2` |

---

### 5.4 `config/questions.py`

This file controls **how the bot answers application form questions**.

| Field | What to Put | Example |
|-------|-------------|---------|
| `default_resume_path` | Path to your resume PDF | `"all resumes/default/resume.pdf"` |
| `years_of_experience` | Your experience for text fields | `"2"` |
| `require_visa` | Do you need visa sponsorship? | `"Yes"` or `"No"` |
| `website` | Your portfolio/website URL | `"https://yourportfolio.com"` |
| `linkedIn` | Your LinkedIn profile URL | `"https://linkedin.com/in/yourname"` |
| `us_citizenship` | Work authorization status | `"Non-citizen seeking work authorization"` |
| `desired_salary` | Expected salary (in rupees, annual) | `600000` (6 LPA) |
| `current_ctc` | Current salary (in rupees, annual) | `300000` (3 LPA) |
| `notice_period` | Notice period in days | `30` |
| `pause_before_submit` | Pause before final submit to review? | `True` |
| `pause_at_failed_question` | Pause if bot can't answer a question? | `True` |

---

### 5.5 `config/settings.py`

Bot behavior settings — you can leave most as default.

| Field | What it Does | Recommended |
|-------|-------------|-------------|
| `close_tabs` | Close external apply tabs automatically? | `True` |
| `follow_companies` | Follow companies you apply to? | `False` |
| `run_non_stop` | Run continuously until stopped? | `False` |
| `alternate_sortby` | Alternate between "Most recent" and "Most relevant"? | `True` |
| `cycle_date_posted` | Cycle through date filters? | `True` |
| `stop_date_cycle_at_24hr` | Stop cycling at "Past 24 hours"? | `True` |
| `file_name` | Path to applied jobs CSV | `"all excels/all_applied_applications_history.csv"` |
| `failed_file_name` | Path to failed jobs CSV | `"all excels/all_failed_applications_history.csv"` |
| `logs_folder_path` | Logs directory | `"logs/"` |
| `click_gap` | Delay between clicks (seconds) | `1` |
| `run_in_background` | Run Chrome headless (no window)? | `False` |
| `disable_extensions` | Disable Chrome extensions? | `False` |
| `safe_mode` | Run in guest profile? | `True` (if Chrome has issues) |
| `smooth_scroll` | Smooth scrolling animation? | `False` |
| `keep_screen_awake` | Prevent PC from sleeping? | `True` |
| `stealth_mode` | Undetected mode (bypass bot detection)? | `True` |

---

## 6. How to Run

There are **two ways** to run this bot — choose whichever suits you.

---

### Method A: Web UI Mode (Recommended for Beginners)

The bot comes with a **Flask Web Dashboard** that lets you fill everything through a browser interface — no manual file editing needed.

**Step 1:** Start the web server

```bash
python app.py
```

**Step 2:** Open your browser and go to:

```
http://localhost:5000
```

**Step 3:** Fill in your details across the 5 tabs:
- **Login tab** → LinkedIn email & password
- **Profile tab** → Your name, phone, city, salary
- **Job Search tab** → Job titles, filters, bad words
- **Launch Bot tab** → Click "⚡ Apply for Jobs"

**Step 4:** The bot runs in the background. Monitor logs from the Launch tab.

> ✅ **How it works internally:** When you click "Apply for Jobs", the web UI saves all your inputs into the `config/*.py` files, then launches `runAiBot.py` as a background process. The bot reads those config files and starts applying.

---

### Method B: Direct Bot Mode (Edit Files & Run)

If you prefer **editing files directly** and running the bot without the web UI, follow this:

#### Step 1: Edit config files manually

Open each file in a text editor (Notepad, VS Code, etc.) and fill in your details:

| File | What to Fill |
|------|-------------|
| `config/secrets.py` | LinkedIn email, password, AI settings |
| `config/personals.py` | Your name, phone, address, gender |
| `config/search.py` | Job titles, location, filters |
| `config/questions.py` | Resume path, salary, notice period |
| `config/settings.py` | Bot behavior (stealth, click speed, etc.) |

Each field is explained in detail in **[Section 5](#5-configuration-files--every-field-explained)** above.

#### Step 2: Run the bot directly

```bash
python runAiBot.py
```

That's it! The bot will:
1. Open Google Chrome
2. Log into LinkedIn automatically (or ask you to log in manually)
3. Search for jobs matching your `search_terms`
4. Apply filters from `config/search.py`
5. Go through each job listing
6. Click "Easy Apply" buttons
7. Fill application forms using your `config/questions.py` answers
8. Submit applications
9. Save history to `all excels/` folder
10. Show a summary popup when finished

#### Step 3: Monitor progress

The bot prints everything to the terminal. You'll see:
- Which job it's currently viewing
- Whether it applied successfully or skipped
- Any errors encountered
- A final summary with counts

Logs are also saved to `logs/log.txt` for later review.

---

### Method B Flow Diagram (What happens when you run `runAiBot.py`)

```
python runAiBot.py
        │
        ▼
  ┌─────────────────┐
  │ Validate Config  │ ← Checks all config files for errors
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  Open Chrome     │ ← Opens browser (stealth/normal/safe mode)
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  Login LinkedIn  │ ← Auto or manual login
  └────────┬────────┘
           │
           ▼
  ┌─────────────────────────────────┐
  │  For Each Search Term:          │
  │  ┌───────────────────────────┐  │
  │  │ Search Jobs → Apply       │  │
  │  │ Filters → Get Listings   │  │
  │  │ → For Each Job:          │  │
  │  │   1. Check blacklist     │  │
  │  │   2. Check bad words     │  │
  │  │   3. Check experience    │  │
  │  │   4. Click Easy Apply    │  │
  │  │   5. Fill form questions │  │
  │  │   6. Submit Application  │  │
  │  │   7. Save to CSV         │  │
  │  └───────────────────────────┘  │
  └────────┬────────────────────────┘
           │
           ▼
  ┌─────────────────┐
  │  Show Summary    │ ← Total applied, failed, skipped
  └─────────────────┘
```

---

## 7. Web UI Walkthrough (Tab by Tab)

When you open `http://localhost:5000`, you'll see 5 tabs:

### Tab 1: 🔐 Login
- Enter your LinkedIn **email/username** and **password**
- These are saved to `config/secrets.py` locally

### Tab 2: 👤 Profile
- Fill your **personal details** (name, phone, city, salary)
- Add your **portfolio URL**, **LinkedIn URL**
- These are saved to `config/personals.py` and `config/questions.py`

### Tab 3: 🔍 Job Search
- Enter **job titles** you want to search (one per line)
- Select **Experience Level** (Entry level, Associate, etc.)
- Select **Job Type** (Full-time, Contract, etc.)
- Add **bad words** to filter out unwanted jobs
- Set **search location** and **date posted**
- These are saved to `config/search.py`

### Tab 4: 🚀 Launch Bot
- Review your settings
- Click **"⚡ Apply for Jobs"** button
- The bot starts in background
- **Live logs** appear in the terminal
- Click **"Refresh Logs"** to check progress
- Click **"Applied Jobs History"** to see results

### Tab 5: 📋 History
- View all jobs you've applied to
- Shows Job ID, Title, Company, Date Applied

---

## 8. AI Integration Setup

If you want the bot to use AI to answer unknown questions:

### OpenAI (Recommended)

1. Get an API key from https://platform.openai.com/api-keys
2. In `config/secrets.py` (or via UI), set:

```python
use_AI = True
ai_provider = "openai"
llm_api_key = "sk-your-actual-key-here"
llm_model = "gpt-4o-mini"  # or "gpt-4" etc.
```

### Google Gemini

```python
use_AI = True
ai_provider = "gemini"
llm_api_key = "your-gemini-api-key"
llm_api_url = "https://generativelanguage.googleapis.com/v1/"
llm_model = "gemini-1.5-flash"
```

### DeepSeek

```python
use_AI = True
ai_provider = "deepseek"
llm_api_key = "your-deepseek-key"
llm_api_url = "https://api.deepseek.com/v1/"
llm_model = "deepseek-chat"
```

---

## 9. Resume Setup

1. Take your **PDF resume**
2. Rename it to `resume.pdf` (exact name, lowercase)
3. Place it in: `all resumes/default/resume.pdf`

The folder structure should be:

```
all resumes/
└── default/
    └── resume.pdf    ← Your file here
```

If the file is missing, the bot will show a warning and use your previously uploaded LinkedIn resume instead.

---

## 10. Monitoring & Logs

### Log Files
All bot activity is logged to:
- `logs/log.txt` — Full log of all actions
- `logs/screenshots/` — Screenshots on errors (for debugging)

### CSV History Files
- `all excels/all_applied_applications_history.csv` — Successfully applied jobs
- `all excels/all_failed_applications_history.csv` — Failed/skipped jobs with reasons

### View Logs from Web UI
- Go to **Launch Bot** tab → click **"Refresh Logs"** to see latest 50 lines

---

## 11. Troubleshooting

| Problem | Solution |
|---------|----------|
| **"Chrome driver not found"** | Set `stealth_mode = True` in `config/settings.py` |
| **"Browser won't open"** | Set `safe_mode = True` in `config/settings.py` |
| **Login fails** | Check your credentials in `config/secrets.py`; try manual login |
| **Not finding any jobs** | Broaden your `search_terms` or `date_posted` filter |
| **Bot is too fast/slow** | Adjust `click_gap` in `config/settings.py` |
| **CVS field size error** | Already fixed — `csv.field_size_limit(1000000)` is set |
| **AI not answering** | Check `llm_api_key` and internet connection |
| **"Daily limit reached"** | LinkedIn restricts Easy Apply — wait 24 hours |
| **Questions not being answered** | Set `use_AI = True` or fill `config/questions.py` with more details |
| **Port 5000 in use** | Change port in `app.py` line 185: `app.run(debug=True, port=5001)` |
| **Error: variable not defined** | Some fields like `linkedin_headline` may be missing — add them to `config/questions.py` |

---

## 12. Quick Config via AI (ChatGPT / Claude)

> **💡 Pro Tip:** Instead of manually filling every config file field, you can use AI to do it instantly!

### How it works:

1. Select ALL files from the `config/` folder:
   - `config/secrets.py`
   - `config/personals.py`
   - `config/search.py`
   - `config/questions.py`
   - `config/settings.py`

2. Copy-paste the entire contents of these files into **ChatGPT** or **Claude AI**

3. Give it a prompt like this:

   > "I want to configure this LinkedIn Auto Job Applier bot for my job search. Here are my details:
   > - Name: Muthukumar J
   > - Email: myemail@gmail.com
   > - LinkedIn password: [your password]
   > - Phone: 9876543210
   > - Location: Chennai, India
   > - Experience: 2 years as Full Stack Developer
   > - Skills: Python, React, Node.js, JavaScript
   > - Looking for: Remote Full Stack Developer jobs, Entry level, Full-time
   > - Current salary: 3.6 LPA, Expected: 6 LPA
   > - Notice period: 30 days
   >
   > Please fill all the config files below with my information. Return the complete file contents that I can directly copy and replace."

4. The AI will return **fully filled config files** — just copy and paste them into your project!

5. Run `python app.py` and you're ready to go 🚀

### Why this is useful:

- ✅ No need to understand every config field manually
- ✅ AI fills all fields intelligently based on your description
- ✅ Takes 2 minutes instead of 20 minutes
- ✅ Reduces errors from manual editing
- ✅ Works with both ChatGPT and Claude

---

## Final Checklist Before Running

- [ ] Python 3.8+ installed
- [ ] All pip packages installed (`pip install flask flask-cors selenium pyautogui undetected-chromedriver`)
- [ ] Google Chrome installed (latest version)
- [ ] Resume placed at `all resumes/default/resume.pdf`
- [ ] Config files filled (via Web UI or manual edit or AI)
- [ ] LinkedIn credentials correct
- [ ] Run `python app.py`
- [ ] Open `http://localhost:5000`
- [ ] Click **⚡ Apply for Jobs**

---

> **⚠️ Disclaimer:** Use this tool responsibly. LinkedIn may temporarily restrict accounts that use automation. This is for personal use only. The original bot is by [Sai Vignesh Golla](https://github.com/GodsScion/Auto_job_applier_linkedIn).
