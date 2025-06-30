# EmotiVibe: AI-Powered Emotional Wellness Platform

**EmotiVibe** is a modern, responsive web application designed to help users understand and navigate their emotional landscape. By leveraging AI-driven facial emotion analysis, the platform translates complex emotional states into an intuitive, 8-code "VIBE" system based on the scientific PAD (Pleasure-Arousal-Dominance) model. Users can analyze their emotions, receive AI-generated insights, and creatively express themselves through an integrated **Journal Studio**.

## ✨ Key Features

- **AI Emotion Analysis**: Upload an image or use your webcam to receive a detailed emotional analysis, including a dominant VIBE code, PAD values, and a softmax probability distribution across four core emotions.
- **Guest Mode**: Try the core analysis feature up to 3 times without signing up. Anonymous session data is seamlessly merged upon registration.
- **Journal Studio**: A powerful, Fabric.js-based integrated editor to create beautiful, personalized journal entries and emotion cards using templates, rich text, image editing, and AI-assisted tools.
- **Personal Diary \& Dashboard**: Track your emotional journey over time with a FullCalendar-based diary, and gain insights from a comprehensive dashboard featuring KPI cards, charts, and a "Compare Mode."
- **Learn \& Explore**: An interactive "Learn" page with a PAD simulator that allows users to understand the scientific principles behind the VIBE codes.
- **AI Chat \& Insights**: Engage in reflective conversations with a generative AI (powered by Google Gemini \& OpenAI GPT-4o) that understands your emotional context and helps you explore your feelings.
- **Modern UI/UX**: A sleek, emoji-free, and fully responsive design system built with Tailwind CSS, supporting both light and dark modes.
- **PWA \& Accessibility**: Installable as a Progressive Web App for an app-like experience with offline capabilities, and built to meet WCAG 2.1 AA accessibility standards.


## 🛠️ Technology Stack

| Layer | Technologies \& Services |
| :-- | :-- |
| **Frontend** | Vue 3 (Composition API), Vite, Pinia (State Management), Vue Router, Tailwind CSS, Chart.js, FullCalendar, Fabric.js |
| **Backend** | Python 3.11, Flask, SQLAlchemy, PostgreSQL, Flask-Login, Gunicorn |
| **AI/ML** | PyTorch (EfficientNet-B2), Google Gemini 1.5 Flash (Primary), OpenAI GPT-4o (Fallback), NRC-VAD v2.1 (PAD Coefficients) |
| **Database** | PostgreSQL 15 |
| **DevOps** | Docker, Terraform, GitHub Actions, AWS (ECS Fargate, RDS, S3, CloudFront) |
| **Compliance** | GDPR \& CCPA compliant, EXIF data removal, 30-day image data retention policy |

## 🏛️ Architecture

EmotiVibe is architected as a modern **Single-Page Application (SPA)** with a decoupled **Flask REST API** backend.

- **Frontend (`/frontend`)**: A Vue 3 application responsible for all UI rendering and user interactions. It communicates with the backend via RESTful API calls.
- **Backend (`/backend`)**: A Flask server that handles business logic, database operations, user authentication, and serves as a secure gateway for all AI/ML model inferences and third-party LLM API calls.
- **Infrastructure (`/infrastructure`)**: All cloud resources are managed via Terraform (Infrastructure as Code) and deployed through a GitHub Actions CI/CD pipeline.


## 🚀 Getting Started

Follow these instructions to get the EmotiVibe project up and running on your local machine for development and testing purposes.

### Prerequisites

- Python 3.11+ and `pip`
- Node.js 20+ and `npm`
- PostgreSQL 15
- Git


### Backend Setup

1. **Clone the repository:**

```bash
git clone https://github.com/your-username/emotivibe.git
cd emotivibe/backend
```

2. **Create and activate a virtual environment:**

```bash
python3 -m venv venv
source venv/bin/activate
```

3. **Install Python dependencies:**

```bash
pip install -r requirements.txt
```

4. **Set up the environment variables:**
Create a `.env` file in the `/backend` directory and populate it based on `.env.example`:

```ini
# .env
SECRET_KEY="a-very-secret-and-secure-key"
DATABASE_URL="postgresql://user:password@localhost:5432/emotivibe_db"
GOOGLE_API_KEY="AIza..."
OPENAI_API_KEY="sk-..."
```

5. **Set up the database:**
Ensure your PostgreSQL server is running and the database `emotivibe_db` exists. Then, apply migrations:

```bash
flask db upgrade
```

6. **Run the Flask server:**

```bash
flask run
# The backend API will be available at http://127.0.0.1:5000
```


### Frontend Setup

1. **Navigate to the frontend directory:**

```bash
cd ../frontend
```

2. **Install Node.js dependencies:**

```bash
npm install
```

3. **Run the Vite development server:**

```bash
npm run dev
# The frontend application will be available at http://localhost:5173
```


## 🔬 Core Scientific Logic: From Emotion to VIBE

The core of EmotiVibe is its unique pipeline that transforms a facial expression into an intuitive VIBE code.

1. **Emotion Classification**: An image containing a face is processed by a PyTorch **EfficientNet-B2** model, which outputs a softmax probability distribution across four core emotions: `anger`, `happy`, `panic`, and `sadness`.
2. **PAD Vector Calculation**: We use the **Pleasure-Arousal-Dominance (PAD)** model to represent emotions in a 3D space. The final PAD vector is a weighted sum of the emotion probabilities and their corresponding coefficients from the **NRC-VAD v2.1 lexicon**.

The formula for each dimension (`dim` can be `pleasure`, `arousal`, or `dominance`) is:
\$ pad_{dim} = \sum_{i=1}^{4} (prob_{emotion_i} \times PAD\_VALUES_{emotion_i, dim}) \$
The `PAD_VALUES` coefficients are:

```json
{
  "anger":   {"pleasure": -0.666, "arousal":  0.730, "dominance":  0.314},
  "happy":   {"pleasure":  0.985, "arousal":  0.470, "dominance":  0.390},
  "panic":   {"pleasure": -0.876, "arousal":  0.876, "dominance": -0.200},
  "sadness": {"pleasure": -0.896, "arousal": -0.424, "dominance": -0.672}
}
```

3. **VIBE Code Generation**: The 3D PAD vector is converted into a 3-letter VIBE code based on the sign of each dimension:
    - Pleasure: `V` (Vibrant) for ≥ 0, `M` (Mellow) for < 0
    - Arousal: `E` (Energized) for ≥ 0, `C` (Calm) for < 0
    - Dominance: `O` (Own) for ≥ 0, `Y` (Yielding) for < 0
4. **Mixed Emotion (AMB) Handling**: To account for neutral or complex expressions, if all PAD values fall within a "dead-zone" threshold of **±0.1**, the system assigns a special **AMB** (Ambiguous) code, and the UI provides guidance accordingly.

## 🔐 API Endpoints

The backend exposes a RESTful API for the frontend. All endpoints are prefixed with `/api`.


| Method | Endpoint | Description | Auth Required |
| :-- | :-- | :-- | :-- |
| `POST` | `/auth/register` | Register a new user. | No |
| `POST` | `/auth/login` | Log in and receive a session cookie. | No |
| `POST` | `/auth/merge-anonymous` | Merge anonymous session data to the new user account. | Yes |
| `GET` | `/auth/me` | Get current authenticated user details. | Yes |
| `POST` | `/api/analyze` | Analyze an image and get emotion analysis results. | No |
| `GET` | `/api/diary` | Get a list of diary entries for the user. | Yes |
| `POST` | `/api/diary` | Create a new diary entry. | Yes |
| `PUT` | `/api/diary/{id}` | Update a specific diary entry. | Yes |
| `GET` | `/api/diary/{id}/insight` | Get AI-generated insights for a diary entry. | Yes |
| `GET` | `/api/diary/{id}/share-emotion/stream` | Start an AI chat session via Server-Sent Events (SSE). | Yes |
| `GET` | `/api/dashboard/stats` | Get aggregated statistics for the user's dashboard. | Yes |
| `GET` | `/api/health` | Health check endpoint for monitoring. | No |

## 🚀 CI/CD Pipeline

This project uses **GitHub Actions** for its Continuous Integration and Continuous Deployment pipeline. The workflow is defined in `.github/workflows/main.yml` and includes the following stages:

1. **Lint \& Test**: Automatically runs linters and unit/integration tests on every push.
2. **Build**: Builds the frontend assets and creates a Docker image for the backend.
3. **Deploy**: Pushes the Docker image to AWS ECR and triggers a deployment to AWS ECS Fargate using Terraform.

## 📄 License

This project is licensed under the MIT License - see the `LICENSE` file for details.

