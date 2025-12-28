```markdown
# Smart Course Builder Using GEN AI

Smart Course Builder Using GEN AI is a Streamlit web app that lets you securely log in, design custom course structures, generate rich course content with Google's Gemini models, and save courses per user with module-wise YouTube recommendations and PDF export. [web:97]

> Note: This project uses Google's Gemini API for content generation. Make sure you have a valid Gemini API key before running the app. [web:97]

---

## ✨ Features

- **Secure login system**
  - Username + password with SHA-256 hashing.
  - Accounts stored in a local JSON file so they persist across restarts (no database needed). [web:128]
  - Separate login and signup screens.

- **Per-user course workspace**
  - Each user sees **only their own courses**.
  - Maximum of **20 courses per user** (can delete old ones to free space).
  - Load any previous course to view its content again.

- **Course builder wizard**
  - Multi-step UI:
    - Step 1: Basic info (category, topic, extra details, number of modules).
    - Step 2: Learner profile (difficulty, duration, target audience).
    - Step 3: Options (teaching style, AI instructions, include YouTube links).
    - Step 4: Generate course with Gemini.
    - Step 5: Preview + module-wise display. [web:97]
  - Forces a clear `Module X: Title` format so modules are parsed correctly.

- **AI-powered content generation**
  - Uses Gemini (e.g. `gemini-2.5-flash`) to generate:
    - Detailed introduction.
    - Learning objectives.
    - Long-form content for each module.
    - Summary / revision section.

- **Module-wise YouTube links**
  - For each module, generates a YouTube **search link** based on course title + module title.
  - Each module section shows a “Watch recommended YouTube video” link after the content.

- **Course preview and PDF export**
  - Adjustable character-length preview of generated course.
  - Full course broken into modules.
  - Download complete course as **PDF** using FPDF.

---

## 🧱 Tech Stack

- **Language:** Python 3.9+
- **Frontend / App Framework:** Streamlit
- **Gen AI:** Google Gemini API (`google-generativeai`) [web:97]
- **PDF generation:** FPDF
- **Storage:** Local JSON file (`users_data.json`) for users and course metadata. [web:128]

---

## 📦 Installation

1. **Clone the repository**

```
git clone https://github.com/srilekhachokkarapu/smart-course-builder-genai.git
cd smart-course-builder-genai
```

2. **Create a virtual environment (recommended)**

```
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
```

3. **Install dependencies**

Create `requirements.txt`:

```
streamlit
google-generativeai
fpdf
```

Install:

```
pip install -r requirements.txt
```

4. **Configure Gemini API key**

For local development, you can either:

- Keep the key hard-coded in the file (for testing only), or
- More securely, set an environment variable:

Update in `smart_course_builder.py`:

```
import os
GEMINI_API_KEY = os.getenv("GEMINI_API_KEY")
```

Then set:

```
export GEMINI_API_KEY="your_gemini_api_key_here"   # macOS/Linux
set GEMINI_API_KEY=your_gemini_api_key_here        # Windows (CMD)
```

Gemini usage requires a valid key from Google AI Studio. [web:97]

---

## 🚀 Running the App Locally

From the project folder:

```
streamlit run smart_course_builder.py
```

This will start the app, typically at:

- `http://localhost:8501`

Open that URL in your browser.

---

## 🔐 Authentication & Data Model

- Users are stored in `users_data.json` with:
  - `name`
  - `password_hash` (SHA-256)
  - `courses`: list of course objects
- Passwords are hashed with `hashlib.sha256` before saving. [web:137]
- Courses include:
  - `id` (timestamp-based)
  - `title`
  - `category`
  - `created_at`
  - `full_text` (full course content)
- Session state (`st.session_state`) tracks:
  - Logged-in user
  - Wizard step
  - Current course content and modules. [web:120]

> For production-grade apps, consider stronger password hashing (e.g., bcrypt/Argon2) and a proper database instead of a JSON file. [web:131]

---

## 🧪 Typical Usage Flow

1. Open the app.
2. Sign up with a username and password (stored for future sessions).
3. Log in → you see **“Smart Course Builder Using GEN AI”** and your personal course list.
4. Use the wizard to define course details and generate content with Gemini.
5. View:
   - Top preview of the course.
   - Each module with text content + YouTube link.
6. Download full course as PDF.
7. Create more courses (up to 20) or delete old ones from your personal list.

---

## 🛠 Project Structure

```
smart-course-builder-genai/
├─ smart_course_builder.py   # Main Streamlit app
├─ requirements.txt          # Python dependencies
└─ users_data.json           # Auto-created local storage for users & courses
```

`users_data.json` is created/updated automatically the first time you create an account or a course. [web:128]

---

## 🤝 Contributing

Suggestions and improvements are welcome:

1. Fork the repository.
2. Create a feature branch.
3. Commit your changes.
4. Open a pull request.
```
