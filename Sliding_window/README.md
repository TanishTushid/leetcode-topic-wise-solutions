# FastAPI Production-Ready Templates & ML/DL Serving Workspace 🚀

Welcome to your production-ready FastAPI and Machine Learning deployment workspace. This workspace contains two complete templates designed to help you build secure web backends and deploy production-grade machine learning pipelines.


---

## 📁 Workspace Structure

```
d:\FastApi\
├── README.md                          # This workspace directory guide
├── fastapi-template/                  # Complete REST API & WebSocket Template
│   ├── app/                           # Core FastAPI application package
│   │   ├── main.py                    # Main setup, middleware, global error handlers
│   │   ├── config.py                  # Settings management via pydantic-settings
│   │   ├── database.py                # Async database engine & session dependency
│   │   ├── dependencies.py            # Reusable JWT & API Key auth guards
│   │   ├── models/                    # SQLAlchemy 2.0 ORM models
│   │   ├── schemas/                   # Pydantic v2 schemas (input/output validation)
│   │   ├── services/                  # Clean business logic decoupling layer
│   │   ├── routers/                   # API routes (Auth, Users, Parameters, Files, WebSockets)
│   │   ├── middleware/                # Custom ASGI middleware (Request ID logger)
│   │   └── utils/                     # JWT tokens & password hashing helpers
│   ├── tests/                         # Async test suite using pytest + httpx AsyncClient
│   ├── alembic/                       # Database migrations folder
│   ├── Dockerfile                     # Multi-stage production container
│   ├── docker-compose.yml             # Local dev stack (API, PostgreSQL, Redis, Celery)
│   └── requirements.txt               # Backend dependencies
└── ml-dl-deploy-structure/            # ML/DL Training-to-Deployment Pipeline
    ├── configs/                       # YAML configuration files (features & hyperparameters)
    │   ├── model_config.yaml          # Model options (RandomForest, GBM, XGBoost, DL)
    │   └── training_config.yaml       # Feature list, data splits, and serving settings
    ├── src/                           # Machine Learning pipeline source package
    │   ├── data/                      # Data engineering (clean, split, validate schemas)
    │   ├── features/                  # Feature engineering (imputers, standard scalers, cyclical dates)
    │   ├── models/                    # Model actions (training, evaluation, batch prediction)
    │   └── utils/                     # System configs and loggers
    ├── api/                           # FastAPI serving microservice
    │   ├── main.py                    # Single-row & batch inference endpoints
    │   ├── schemas.py                 # Pydantic schemas validating input features
    │   ├── predictor.py               # Singleton preloading model/pipeline artifacts
    │   └── health.py                  # Liveness and readiness orchestrator probes
    ├── tests/                         # Test suite (schema assertions, prediction validation)
    ├── docker/                        # Training & serving containers + Docker Compose
    ├── Makefile                       # Automation shortcut utilities
    ├── requirements-train.txt         # Training dependencies (pandas, scikit-learn, etc.)
    └── requirements-serve.txt         # Serving dependencies (fastapi, uvicorn, etc.)
```

---

## 💎 Template 1: Production FastAPI Template (`/fastapi-template`)

A modular, copy-paste-ready template built with best practices:

*   **🔒 Auth Deep Dive**: JWT Access and Refresh token rotation, OAuth2 password form flow (built-in support for Swagger UI's "Authorize" lock button), and Secure API-Key headers for service-to-service calls.
*   **🗄️ Database Integration**: Async SQLAlchemy 2.0 ORM structure paired with Alembic migration setups. Default configured for SQLite, production-ready for PostgreSQL.
*   **📡 Parameter Showcase**: A complete endpoint list demonstrating how to query, parse, and validate **Path, Query, Header, Cookie, JSON Body, HTML Form, and Enum** parameters with custom Pydantic validations.
*   **📁 File Manager**: Safe uploads enforcing MIME-type whitelists and file size checks. Includes file download responses and low-memory `StreamingResponse` generators.
*   **🔌 Realtime WebSockets**: Bidirectional connection managers broadcasting messages to dynamic chatrooms with URL JWT token authentication.
*   **🛡️ Robust Middleware**: Custom request timing logging injected with tracing UUID header hashes (`X-Request-ID`), default CORS headers, and response Gzip compression.
*   **⚠️ Uniform Error Schema**: Flattened Pydantic error list outputs, global uncaught error safety wrappers, and customized HTTP error envelopes.

### Quick Start: FastAPI Web Server

```bash
# 1. Enter directory and setup venv
cd fastapi-template
python -m venv .venv
.venv\Scripts\activate      # Windows
# source .venv/bin/activate # Linux/macOS

# 2. Install dependencies & copy env config
pip install -r requirements.txt
copy .env.example .env

# 3. Launch live hot-reload server
uvicorn app.main:app --reload --port 8000
```
*   **Swagger documentation**: http://localhost:8000/docs
*   **ReDoc documentation**: http://localhost:8000/redoc
*   **Unit Tests**: Run `pytest` inside the directory to run the async test client.

---

## 🧠 Template 2: ML/DL Serving pipeline (`/ml-dl-deploy-structure`)

A production template built to package model training into an active FastAPI microservice:

*   **📁 Decoupled Ingestion**: Separated folders for `data/raw/` (immutable), `data/processed/` (clean splits), and `artifacts/` (serialized models).
*   **✅ Data Quality Controls**: Data quality scripts catching missing variables, schema mismatches, and data leakage between train/test splits.
*   **⚙️ Fitted Pipelines**: Saves serializable pipelines (`feature_pipeline.pkl`) fitted ONLY on the training split, preventing lookahead leakage.
*   **📊 Diagnostics & Metrics**: Automatic generation of Confusion Matrices, ROC curves, feature importances, and validation metric JSONs.
*   **🚀 Predictor Singleton**: Pre-loads model objects into memory during server startup (avoiding slow filesystem reads during API calls) and handles thread-safe batch scoring.
*   **🐳 Containerized Pipeline**: Orchestrates training and serving sequentially via Docker Compose.

### Quick Start: ML Pipeline & Serving

```bash
# 1. Navigate to directory and install packages
cd ml-dl-deploy-structure
pip install -r requirements-train.txt -r requirements-serve.txt

# 2. Process data (Raw -> Clean Splits)
make data

# 3. Train your model (Serializes artifacts/models/)
make train

# 4. Generate evaluation plots and reports
make evaluate

# 5. Serve predictions using FastAPI
make serve
```
*   **Batch Prediction Endpoint**: `POST http://localhost:8000/predict/batch` (max size configurable in `training_config.yaml`).
*   **Auto-pipeline (Docker)**: Run `docker compose up` inside the `docker/` folder. It will train the model, save the outputs, and boot up the FastAPI serving API.

---

## 🛠️ Combined Developer Cheat Sheet

| Task | FastAPI Template Command | ML/DL Pipeline Command |
|---|---|---|
| **Setup dependencies** | `pip install -r requirements.txt` | `make setup` |
| **Run local server** | `uvicorn app.main:app --reload` | `make serve` |
| **Run unit tests** | `pytest` | `make test` |
| **Generate Migrations** | `alembic revision --autogenerate -m "msg"` | *N/A* |
| **Update Database** | `alembic upgrade head` | *N/A* |
| **Run Pipeline** | *N/A* | `make data` -> `make train` -> `make evaluate` |
| **Docker Launch** | `docker compose up -d` | `cd docker && docker compose up` |
