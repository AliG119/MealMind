# MealMind

Diet recommendation agent built with Spring Boot, MyBatis, MySQL, and AgentScope (DashScope / Qwen).

The backend serves a built-in static UI at the same port — no separate frontend build.

## Requirements

- JDK 21
- Maven 3.9+
- MySQL 8.x
- A [DashScope](https://dashscope.aliyun.com/) API key

## Setup

### 1. Database

Create the schema (and sample tables):

```bash
mysql -u root -p < diet-agent/src/main/resources/db/diet_db.sql
```

There is also a copy at the repo root: `diet_db.sql`.

### 2. Secrets (do not commit real keys)

Set environment variables, or copy the example local config:

```powershell
# PowerShell
$env:DB_USERNAME = "root"
$env:DB_PASSWORD = "your-mysql-password"
$env:DASHSCOPE_API_KEY = "your-dashscope-api-key"
```

Optional local Spring profile file:

```bash
cp diet-agent/src/main/resources/application-local.yml.example diet-agent/src/main/resources/application-local.yml
# edit application-local.yml, then run with profile "local"
```

`application-local.yml` is gitignored.

### 3. Run

```bash
cd diet-agent
mvn spring-boot:run
```

With the local profile:

```bash
mvn spring-boot:run "-Dspring-boot.run.profiles=local"
```

Open [http://localhost:8080/](http://localhost:8080/).

If port 8080 is already in use:

```bash
mvn spring-boot:run "-Dspring-boot.run.arguments=--server.port=8081"
```

## Project layout

```
MealMind/
├── diet-agent/          # Spring Boot app
│   ├── src/main/java/com/diet/
│   ├── src/main/resources/
│   │   ├── application.yml
│   │   ├── db/diet_db.sql
│   │   ├── diet/prompts/   # LLM prompts
│   │   ├── mapper/         # MyBatis XML
│   │   └── static/         # Web UI
│   └── pom.xml
└── diet_db.sql
```

## Main APIs

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/api/v1/diet/chat` | Chat / recommendations |
| POST | `/api/v1/diet/sessions` | Create session |
| GET/POST | `/api/v1/diet/meals/personal` | Personal meals |
| GET | `/api/v1/diet/meals/public` | Public meals |
| POST | `/api/v1/diet/feedback` | Feedback |
| GET | `/api/v1/diet/debug/traces*` | Agent traces |

Send `X-User-Id` header (defaults to `1` in the UI).

## License

No license file is included yet. Add one before redistributing.
