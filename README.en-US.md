# Open-Resume

An online resume editing tool that supports structured resume editing, real-time multi-template preview, AI polishing, PDF/image recognition, AI chat-based editing, version history, JSON import/export, PDF export, and custom templates.

![Open-Resume Dark Theme](images/dark-theme.png)

## Feature Highlights

- **Structured Editing**: Basic information, education, work experience, project experience, and skills modules.
- **Real-time Preview**: Three built-in templates (Classic, Modern, Minimal) with support for importing custom templates.
- **AI Capabilities**: Rich text polishing, PDF resume recognition, image resume recognition, and AI chat-based modification.
- **Version History**: Retains up to 20 history records, supporting diff visualization, rollback, single-item deletion, and clearing all.
- **Export Capabilities**: Supports JSON import/export and server-side A4 PDF export.
- **Local Storage**: Resume content, history records, theme, AI configurations, and custom templates are all saved in the browser's localStorage.
- **Theme Switching**: Supports light/dark mode.
- **Custom Templates**: Import custom templates via JSON, supporting custom CSS styles and layout parameters; provides AI prompt assistance to generate template JSON.

## Preview

### Version History

![Version History](images/history.png)

### Settings Popup

![Settings Popup](images/settings.png)

## Technology Stack

| Module | Technology |
|---|---|
| Frontend | Vue 3, TypeScript, Vite |
| UI | Ant Design Vue |
| State Management | Pinia |
| Rich Text | wangEditor |
| Backend | Python, FastAPI, Uvicorn |
| PDF Generation | Playwright Chromium |
| PDF Text Extraction | PyMuPDF |
| AI | OpenAI-compatible API |

## Quick Start

### 1. Start Backend

```bash
cd api
python -m venv venv
```

Windows PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

Install dependencies and start:

```bash
pip install -r requirements.txt
playwright install chromium
python main.py
```

Default backend address: `http://localhost:8000`

### 2. Start Frontend

```bash
cd client
npm install
npm run dev
```

Default frontend address: `http://localhost:5173`

If multiple Vite services are running locally, it is recommended to access: `http://127.0.0.1:5173`

## Docker Execution

The project root contains a `Dockerfile` and `.dockerignore`, supporting multi-stage builds.

```bash
docker build -t resume-editor .
docker run -p 8000:8000 resume-editor
```

Access: `http://localhost:8000`

> Note: The first build takes longer as it needs to download Playwright Chromium and system dependencies.

## Feature Details

### Resume Editor

- **Basic Information**: Name, career objective, gender, age, phone number, email, city, years of experience, expected salary, preferred city, avatar, personal website, GitHub, and personal strengths.
- **Education**: School, major, degree, start/end dates, description list; supports adding, deleting, and reordering.
- **Work Experience**: Company, position, city, start/end dates, "present" toggle, overview; supports adding, deleting, and reordering.
- **Project Experience**: Project name, role, start/end dates, link, description, tech stack, responsibilities, achievements; supports adding, deleting, and reordering.
- **Skills**: Categorized skill management with proficiency levels (Not shown / Familiar / Proficient / Expert / Master).
- **Module Ordering**: Adjust module sequence in the settings popup; the editor and preview area update synchronously.

### Templates

Three built-in templates; the preview area updates instantly after switching in the settings popup:

| Template | Layout Style |
|---|---|
| Classic | Traditional resume layout, top contact info + avatar, body arranged by modules |
| Modern | Two-column layout, sidebar for contact and skills, main column for work and projects, English module headers |
| Minimal | Compact minimalist style, streamlined header information, column-based body arrangement |

Supports **Custom Templates**: Settings Popup $\rightarrow$ Custom Templates $\rightarrow$ Import Template JSON. Custom templates can override CSS styles, layout parameters, section labels, etc. AI prompt assistance is provided for rapid template creation.

### AI Features

Before use, fill in the interface address, API Key, and model in `Settings -> AI Configuration`.

| Feature | Description |
|---|---|
| AI Polishing | Professionally rewrites rich text fields |
| AI Chat Edit | Modifies the resume via natural language, supports @ referencing work/project entries |
| PDF Recognition | Uploads a PDF resume and automatically populates the editor |
| Image Recognition | Uploads a resume screenshot and automatically populates the editor |

### Version History

Version history is saved in the browser's localStorage, with a maximum of 20 entries.

| Action | Description |
|---|---|
| Save Version | Click "Save Version" on the left to create a snapshot |
| Diff Display | Shows field differences between "Historic Version $\rightarrow$ Current Edit" for each record |
| Rollback | Restore to a specific historical version |
| Delete Single | Deletes a specific history record |
| Clear All | Deletes all history records |

## Project Structure

```text
resume-editor/
├── api/                     # FastAPI Backend
│   ├── main.py              # Backend entry point
│   ├── requirements.txt     # Python dependencies
│   ├── routes/              # API routes
│   ├── schemas/             # Pydantic models
│   └── services/            # AI, OCR, PDF, and template services
├── client/                  # Vue Frontend
│   ├── package.json
│   ├── vite.config.ts
│   └── src/
│       ├── api/             # Frontend API requests
│       ├── components/      # Components
│       │   └── templates/   # Template rendering components
│       ├── stores/          # Pinia store
│       ├── types/           # TypeScript types
│       ├── utils/           # Utility functions
│       └── views/           # Page views
├── docs/                    # Project documentation
├── images/                  # README screenshots
├── Dockerfile
└── README.md
```

## Backend API

| Method | Path | Description |
|---|---|---|
| GET | `/api/health` | Health check |
| GET | `/api/debug/routes` | View registered routes |
| POST | `/api/pdf/export` | Export PDF |
| POST | `/api/ai/models` | Get model list |
| POST | `/api/ai/polish` | AI text polishing |
| POST | `/api/ai/chat-edit` | AI chat-based resume modification |
| POST | `/api/ai/recognize-pdf` | Recognize PDF resume |
| POST | `/api/ai/recognize-image` | Recognize image resume |

The frontend Vite server proxies `/api` to `http://localhost:8000`.

## Local Storage

| Key | Content |
|---|---|---|
| `resume_editor_current_resume` | Current resume data |
| `resume_editor_version_history` | Version history, max 20 items |
| `resume_editor_ai_settings` | AI configurations |
| `resume_editor_theme` | Theme configurations |
| `resume_custom_templates` | Custom template list |

## Development Commands

Frontend type checking:

```bash
cd client
npx vue-tsc --noEmit
```

Frontend build:

```bash
cd client
npm run build
```

Frontend test:

```bash
cd client
npm test
```

Backend start:

```bash
cd api
python main.py
```

## Related Documentation

- [Feature Details](docs/features.md)
- [Test Guide](docs/test-plan.md)
- [Test Report](docs/test-report.md)

## FAQ

### Page does not change after frontend modifications
Confirm that the browser is accessing the frontend service of the current project. If multiple Vite services are running locally, `localhost` might point to another project; try using `http://127.0.0.1:5173`.

### PDF export failed
Confirm that Chromium is installed in the backend:

```bash
playwright install chromium
```

And confirm the backend service is healthy: `http://localhost:8000/api/health`

### AI features failed
Check if the API Key, interface address, and model are correct. Image recognition requires a model with vision capabilities.
