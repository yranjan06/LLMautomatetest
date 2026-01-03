# LLM Automate Test

A FastAPI-based service that automatically generates web applications from natural language briefs using LLM, creates GitHub repositories, and deploys them to GitHub Pages.

## Architecture

### Core Components

1. **FastAPI Server** (`app/main.py`)
   - Receives task requests via REST API
   - Manages request deduplication and persistence
   - Orchestrates background processing

2. **LLM Generator** (`app/llm_generator.py`)
   - Processes task briefs using OpenAI API
   - Generates application code (HTML/JS/CSS)
   - Creates README documentation
   - Handles file attachments

3. **GitHub Integration** (`app/github_utils.py`)
   - Creates/updates repositories
   - Commits generated files
   - Enables GitHub Pages deployment

4. **Notification Service** (`app/notify.py`)
   - Reports completion status to evaluation server
   - Implements retry logic with exponential backoff

### System Flow

```
1. POST /api-endpoint → Receive task brief + attachments
2. Validate secret & check for duplicates
3. Background task starts:
   - Decode attachments
   - Generate code via OpenAI
   - Create/update GitHub repository
   - Commit files (code, README, LICENSE)
   - Enable GitHub Pages
   - Wait for deployment (120s)
   - Notify evaluation server
```

### Data Flow

```
Client Request → FastAPI → LLM Generator → GitHub API → GitHub Pages
                    ↓                                        ↓
              Persistence                            Evaluation Server
```

## Prerequisites

- Python 3.9+
- GitHub account with personal access token
- OpenAI API key
- Environment variables configured (see Setup)

## Setup

1. Create virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Configure environment variables in `.env`:
```
GITHUB_TOKEN=your_github_token
GITHUB_USERNAME=your_username
OPENAI_API_KEY=your_openai_key
USER_SECRET=your_secret
```

4. Run the server:
```bash
uvicorn app.main:app --reload
```

## Project Structure

```
LLMautomatetest/
├── app/
│   ├── main.py            # FastAPI application & endpoints
│   ├── llm_generator.py   # OpenAI integration & code generation
│   ├── github_utils.py    # GitHub API operations
│   └── notify.py          # Evaluation server notification
├── requirements.txt       # Python dependencies
├── runtime.txt           # Python version specification
├── test_github.py        # Connection tests
└── README.md
```

## Key Dependencies

- **fastapi** - Web framework
- **openai** - LLM API client
- **PyGithub** - GitHub API wrapper
- **httpx** - HTTP client for notifications
- **python-dotenv** - Environment management
- **uvicorn** - ASGI server

## Security

Authentication via USER_SECRET environment variable for API requests.

