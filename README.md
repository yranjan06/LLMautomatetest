## LLM Automate Test

### What is it?

LLM Automate Test is a FastAPI service that takes a natural language task brief, generates a complete web application using an LLM, creates a GitHub repository, and deploys the app to GitHub Pages automatically.


---

How it Works

1. FastAPI receives a task brief via API.


2. The LLM generates HTML, CSS, JavaScript, and README files.


3. A GitHub repository is created or updated automatically.


4. Files are committed and GitHub Pages is enabled.


5. After deployment, the evaluation server is notified.



The entire flow runs asynchronously in the background.

---

### How to Run

#### 1. Setup Environment

Create a `.env` file:

```env
GITHUB_TOKEN=your_github_token
GITHUB_USERNAME=your_username
OPENAI_API_KEY=your_openai_key
USER_SECRET=your_secret

#### 2. Install and Start

python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload

Server runs at:

http://127.0.0.1:8000


