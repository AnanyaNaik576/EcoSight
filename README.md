# EchoSight

> AI-powered customer review intelligence platform for fraud detection, trend analysis, and automated insights

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![TypeScript](https://img.shields.io/badge/typescript-5+-blue.svg)](https://www.typescriptlang.org/)
[![FastAPI](https://img.shields.io/badge/fastapi-0.110+-green.svg)](https://fastapi.tiangolo.com/)
[![Next.js](https://img.shields.io/badge/next.js-15.5+-black.svg)](https://nextjs.org/)

## Overview

EchoSight is a comprehensive customer review intelligence platform that leverages AI to detect fraudulent reviews, analyze trends, classify content, and provide intelligent customer Q&A. The platform consists of two main components:

- **AI Engine**: FastAPI microservice powering intelligent review analysis
- **Web Frontend**: Next.js dashboard for visualization and management

## ✨ Key Features

### 🎯 Fake Detection
- Advanced preprocessing with language detection and translation
- Bot and duplicate review detection using TF-IDF similarity
- Account-level spam pattern analysis
- Sentiment and image analysis
- Trust scoring and review bomb detection

### 📊 Trend Analysis
- Sliding-window feature trend detection
- Time-series analysis and visualization
- Automated seller and admin alerts
- Trend snapshot persistence

### 🏷️ Tag Classification
- Automatic review tag extraction
- Feature-level sentiment analysis
- LLM-assisted tagging with rule-based fallback
- Auto-response generation for customer support

### 🤖 Customer Q&A
- Intent detection and understanding
- Retrieval from trusted internal product/review data
- Grounded answer generation with graceful fallback

## 🏗️ Architecture

```
EchoSight/
├── ai_engine/              # FastAPI microservice
│   ├── main.py            # Application entrypoint
│   ├── routers/           # API route handlers
│   ├── pipeline/          # Core processing pipelines
│   ├── db.py              # MongoDB integration
│   └── requirements.txt    # Python dependencies
│
└── web/                    # Next.js frontend
    ├── src/
    │   ├── app/           # App pages and API routes
    │   ├── components/    # React components
    │   └── lib/           # Utilities and seeds
    ├── package.json       # Node dependencies
    └── tsconfig.json      # TypeScript config
```

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- Node.js 18+
- MongoDB (local or cloud)
- Optional: Ollama for local LLM inference

### Installation

#### 1. AI Engine Setup

```bash
# Create and activate Python virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r ai_engine/requirements.txt
```

#### 2. Web Frontend Setup

```bash
cd web
npm install
```

### Configuration

Create a `.env` file in the `ai_engine` directory:

```env
# MongoDB
MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/
MONGODB_DB_NAME=echosight

# Optional: Local LLM (Ollama)
OLLAMA_BASE_URL=http://localhost:11434
MISTRAL_MODEL=mistral

# Feature toggles
CUSTOMER_QA_ENABLE_LLM=true
```

### Running the Services

#### Start AI Engine

```bash
# From repository root
uvicorn ai_engine.main:app --host 0.0.0.0 --port 8000 --reload
```

- **Health Check**: `GET /health`
- **API Docs**: http://localhost:8000/docs
- **OpenAPI Schema**: http://localhost:8000/openapi.json

#### Start Web Frontend

```bash
cd web

# Development mode
npm run dev

# Production build
npm run build
npm start
```

Visit http://localhost:3000 in your browser.

## 📡 API Endpoints

### Fake Detection
```
POST /api/fake-detection/analyze
```
Comprehensive review fraud analysis pipeline including preprocessing, bot detection, sentiment analysis, and trust scoring.

### Trend Analysis
```
POST /api/trend-analysis/detect
GET /api/trend-analysis/timeline/:product_id
```
Time-series trend detection and historical timeline retrieval.

### Tag Classification
```
POST /api/tag-classification/generate
POST /api/tag-classification/auto-respond
```
Automated tag generation and customer support response generation.

### Customer Q&A
```
POST /api/customer-qa/ask
```
Agentic product Q&A with retrieval and grounded answer generation.

## 🔧 Tech Stack

### Backend
- **Framework**: FastAPI + Pydantic
- **Database**: MongoDB
- **ML/AI**: Transformers, PyTorch
- **Image Processing**: Pillow
- **Optional LLM**: Ollama (Mistral)

### Frontend
- **Framework**: Next.js 15 with React 19
- **Styling**: Tailwind CSS 4
- **Database Client**: Mongoose
- **Auth**: JWT + bcryptjs
- **Charts**: Recharts
- **Notifications**: Sonner
- **Language**: TypeScript 5

## 📊 Project Structure

```
ai_engine/
├── main.py                 # FastAPI app with 4 logical microservices
├── db.py                   # MongoDB connection & helpers
├── routers/                # API endpoint handlers
│   ├── fake_detection.py
│   ├── trend_analysis.py
│   ├── tag_classification.py
│   └── customer_qa.py
└── pipeline/               # Processing logic

web/
├── src/
│   ├── app/                # Next.js app directory
│   ├── components/         # Reusable React components
│   ├── lib/
│   │   ├── seeds/          # Database seeding scripts
│   │   └── scripts/        # Maintenance workers
│   └── middleware/         # Auth & request handling
├── package.json            # Dependencies & scripts
└── tsconfig.json           # TypeScript configuration
```

## 🧪 Testing & Development

### Web Development Scripts

```bash
npm run dev              # Start dev server with Turbopack
npm run build           # Production build
npm run lint            # Run ESLint
npm run seed:spam       # Seed spam detection test data
npm run seed:sentiment  # Seed sentiment analysis data
npm run seed:trend      # Seed trend analysis data
npm run seed:extra      # Load additional test reviews
npm run seed:botspam    # Load bot spam demonstration data
npm run cron:reviews    # Run review maintenance worker
```

## 🔄 Service Startup Order

For optimal initialization:

1. **Start MongoDB** - Ensure database is accessible
2. **Start AI Engine** - `uvicorn ai_engine.main:app ...`
3. **Start Web Frontend** - `npm run dev` in `/web`
4. **Verify Health** - Check `/health` on both services

## 📝 Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `MONGODB_URI` | ✅ | MongoDB connection string |
| `MONGODB_DB_NAME` | ❌ | Database name (fallback if not in URI) |
| `OLLAMA_BASE_URL` | ❌ | Local LLM endpoint (e.g., `http://localhost:11434`) |
| `MISTRAL_MODEL` | ❌ | Ollama model name (default: `mistral`) |
| `CUSTOMER_QA_ENABLE_LLM` | ❌ | Enable LLM in Q&A (default: `true`) |

## 🛡️ Graceful Degradation

EchoSight is designed to handle service failures gracefully:

- **MongoDB unavailable**: Pipeline continues with limited historical context
- **Image analysis fails**: Result marked with `image_fetch_error` instead of hard failure
- **Ollama/LLM unavailable**: 
  - Tag generation uses deterministic fallback
  - Auto-response uses template responses
  - Q&A uses grounded fallback answers

## 📚 Documentation

- **AI Engine Docs**: [ai_engine/Readme.md](ai_engine/Readme.md)
- **Web Frontend Docs**: [web/README.md](web/README.md)
- **API Documentation**: Available at `/docs` when running the AI engine

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

**Ananya Naik** — Platform Architecture & Development

## 🎯 Roadmap

- [ ] Advanced analytics dashboard
- [ ] Multi-language support expansion
- [ ] Real-time WebSocket updates
- [ ] Custom model fine-tuning pipeline
- [ ] Integration with third-party review platforms

## 📞 Support

For questions or issues, please open a GitHub issue or contact the development team.

---

<div align="center">

**[⬆ back to top](#echosight)**

</div>
