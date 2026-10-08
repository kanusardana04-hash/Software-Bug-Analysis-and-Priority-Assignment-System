# Software-Bug-Analysis-and-Priority-Assignment-System

A full-stack web application that analyses bug reports and automatically assigns a priority. The **backend is written in C++17** (no external libraries) and the **frontend uses HTML, CSS and JavaScript**.

🔗 **Live Demo:** https://YOUR-APP.onrender.com

---

## 📌 About the Project

In software projects, many bugs are reported and the team must decide which to fix first. This system does that automatically. For every bug it:

- detects the **category** (Security, Stability, Performance, etc.)
- calculates a **priority score from 0 to 100**
- assigns a priority level **P1 to P4**
- flags **possible duplicate** reports

## ✨ Features

- Report bugs with severity, reproducibility, users affected, module and description
- Live priority preview while typing
- Automatic category detection using keyword analysis
- Duplicate detection using Jaccard similarity
- Filter by priority or status, and search
- Status tracking: Open, In Progress, Resolved, Closed
- Dashboard with statistics
- REST API
- Data saved in a file, so it persists between restarts

## 🧮 How Priority Is Calculated

```
score = 100 × (0.40 × Severity + 0.25 × UserImpact + 0.20 × Reproducibility + 0.15 × KeywordRisk)
```

| Factor | Weight | Description |
|---|---|---|
| Severity | 40% | Rated 1 to 5 by the reporter |
| User Impact | 25% | log10(users + 1) / 4, capped at 1 |
| Reproducibility | 20% | Rated 1 to 5 |
| Keyword Risk | 15% | Words like *crash*, *injection*, *data loss* raise the score |

| Score | Priority |
|---|---|
| 75 and above | 🔴 P1 – Critical |
| 55 to 74 | 🟠 P2 – High |
| 35 to 54 | 🟡 P3 – Medium |
| Below 35 | 🟢 P4 – Low |

## 🛠️ Tech Stack

| Part | Technology |
|---|---|
| Backend | C++17, POSIX sockets, std::thread |
| Frontend | HTML5, CSS3, JavaScript |
| Build | Make, g++ |
| Deployment | Docker, Render |

## 📁 Project Structure

```
bug-priority-system/
├── backend/
│   ├── src/
│   │   ├── main.cpp        # HTTP server and REST API
│   │   ├── analyzer.hpp    # Priority scoring, categories, duplicates
│   │   ├── store.hpp       # Thread-safe storage
│   │   └── json.hpp        # JSON helpers
│   └── tests/
│       └── test_analyzer.cpp
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── app.js
├── data/bugs.tsv           # Sample data
├── docs/REPORT.md          # Project report
├── Dockerfile
├── render.yaml
└── Makefile
```

## 🚀 Run Locally

Requires Linux, macOS or WSL with `g++` and `make`.

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
cd YOUR-REPO

make test      # run unit tests
make           # build the server
./bin/server   # start the server
```

Open **http://localhost:8080** in your browser.

Optional environment variables: `PORT` (default 8080), `WEB_ROOT`, `DATA_FILE`.

## 🔌 REST API

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/bugs` | List all bugs, highest priority first |
| POST | `/api/bugs` | Create a new bug |
| POST | `/api/analyze` | Preview priority without saving |
| PUT | `/api/bugs/{id}` | Update status |
| DELETE | `/api/bugs/{id}` | Delete a bug |
| GET | `/api/stats` | Statistics |
| GET | `/api/health` | Health check |

**Example request**

```bash
curl -X POST http://localhost:8080/api/bugs \
  -H "Content-Type: application/json" \
  -d '{"title":"App crashes on export","description":"Crash when exporting PDF","module":"Reports","severity":4,"reproducibility":4,"usersAffected":300}'
```

**Example response**

```json
{
  "id": 6,
  "title": "App crashes on export",
  "category": "Stability",
  "priority": "P2",
  "score": 73.4,
  "status": "Open",
  "duplicateOf": 0
}
```

## 🧪 Testing

Run `make test` to execute the unit tests. They check priority levels, category detection, input clamping and duplicate detection.

## ☁️ Deployment (Render)

1. Push this repository to GitHub.
2. On [render.com](https://render.com), click **New → Web Service** and connect the repo.
3. Select the **Docker** runtime and the free plan.
4. Click **Deploy**.

> The free tier may reset saved data on restart. The app returns to the sample data.

## ⚠️ Limitations

- Keyword analysis is simple and English-only
- Weights are chosen by judgement, not learned from data
- Uses a flat file instead of a database
- No login or user roles

## 🔮 Future Work

- Machine-learning based classification
- SQLite or PostgreSQL database
- User authentication and roles
- Assigning bugs to developers
- Charts and trend reports

## 👤 Author

**Kashish**
UID: 26MCA20152
