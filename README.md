# AI-Movie_recomendation
🎬 CineMatch: Premium AI-Powered Movie Recommender
Python VersionFlaskScikit-LearnTMDb

CineMatch is a full-stack, premium movie recommendation application that goes beyond simple genre matching. By leveraging a weighted TF-IDF (Term Frequency-Inverse Document Frequency) model combined with Cosine Similarity and a hybrid ranking algorithm, CineMatch analyzes storylines, directors, cast members, and user ratings to recommend movies. It fully supports regional Indian languages (Hindi, Marathi, Kannada) alongside English and features an advanced URL-based movie detection system.

🌟 Key Features
🧠 Hybrid Recommendation Engine: Combined Content-Based Filtering using TF-IDF and Cosine Similarity, tuned with custom weights (Director: 3x, Cast: 2x) and raw user rating bias (5% boost per rating point) for optimal suggestions.
🔗 Smart URL Movie Detection: Paste a trailer link or direct page URL from YouTube, Netflix, Hotstar, Amazon Prime, JioCinema, MXPlayer, or IMDb. The system automatically extracts the ID, resolves it via API or URL slugs, cleans the metadata, and presents recommendations.
🔍 Auto-Suggest & Fuzzy Matching: Uses RapidFuzz for real-time autocomplete suggestions and robust typo tolerance if a movie name is entered incorrectly.
🌐 Multi-Language Support: Curated dataset matching local preferences across English, Hindi, Marathi, and Kannada.
💎 Glassmorphism UI: A gorgeous, modern dark-themed web interface built with a responsive grid, smooth animations, interactive movie cards, details modals, and client-side pagination.
⚡ Background Initialization & Caching: Multi-threaded metadata retrieval (ThreadPoolExecutor) at startup, caching processed models to a local .pkl file for instant subsequent load times (under 2 seconds).
📐 Architecture & Workflow
Searches Movie or Pastes URL
HTTP Requests
Query Engine
Finds Index
1st Run Initialization
Parallel Credits Retrieval
Fills Cast/Director
NLP Preprocessing
Vectorization
Instant Startup load
Retrieve Top 100 Sim Scores
Filter by Language
JSON Response
Render UI Cards & Modal
User Interface
Frontend: HTML/CSS/JS
Flask Backend
RapidFuzz Title Match
TF-IDF + Cosine Similarity Matrix
ThreadPoolExecutor
TMDb API
Pandas DataFrame
Clean Tags Generation
Pickle Cache: movies_advanced.pkl
Hybrid Ranking Formula
Top Recommendations List
🧠 Machine Learning Methodology
1. Feature Engineering & Weighting
Instead of standard content-based filtering on plain plots, CineMatch builds a rich document vector ("tags") by combining and weighting different metadata fields:

$$\text{Tags} = \text{Cleaned Title} + \text{Genres} + \text{Overview} + \text{Language} + 2 \times (\text{Top 3 Cast}) + 3 \times (\text{Director})$$

By replicating cast and director terms in the text document, we mathematically amplify their influence during TF-IDF vectorization.

2. Similarity Computation
We convert the text representations of the movies into numerical matrices using a TF-IDF Vectorizer with stop-word removal:

$$\text{Similarity}(A, B) = \cos(\theta) = \frac{A \cdot B}{|A| |B|}$$

3. Hybrid Ranking Formula
To ensure that obscure, poorly rated movies don't crowd out high-quality releases with similar keywords, we apply a custom Hybrid Score to the top 100 similarity matches:

$$\text{Hybrid Score} = \text{Similarity} \times \left(1 + \frac{\text{Rating}}{100} \times 10\right)$$

This formula provides up to a 50% rating boost for highly-rated movies, naturally bubbling up high-quality matches to the top of the feed.

🛠️ Tech Stack
Frontend
Core: HTML5, Vanilla JavaScript (ES6)
Styling: Vanilla CSS3 (Custom Glassmorphic Theme, Custom Keyframe Animations, Flexible Grids)
Typography: Plus Jakarta Sans (Google Fonts)
Backend
Server Framework: Flask, Flask-CORS
Data Processing: Pandas, NumPy
Machine Learning / NLP: Scikit-Learn (TF-IDF Vectorizer, Cosine Similarity)
Text Cleaning & Detection: Re (Regex), Langdetect, RapidFuzz
Parallelization & Network: Requests, ThreadPoolExecutor
📂 Repository Structure
.
├── backend/
│   ├── app.py                  # Core Flask API & recommendation algorithms
│   ├── requirements.txt        # Python dependency manifest
│   ├── movies_advanced.pkl     # Generated ML model cache (created at startup)
│   └── venv/                   # Python virtual environment (ignored by git)
├── frontend/
│   └── index.html              # Single Page Application (UI, CSS, JS Logic)
├── data/
│   └── movies_dataset.csv      # Cache of TMDb fetched movie datasets
├── start_app.bat               # Unified application launcher (Windows)
├── start_backend.bat           # Dedicated backend starter
└── README.md                   # Repository Documentation
🚀 Setup & Execution
Option A: Unified Launcher (Windows)
Double-click the start_app.bat script in the root directory.
The script will automatically resolve Python environment setups, launch the Flask server in the background, serve the frontend locally, and open your browser to http://localhost:3000.
Option B: Manual Setup
1. Setup & Run the Backend
bash
# Navigate to backend directory
cd backend
# Create a virtual environment
python -m venv venv
# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
# Install dependencies
pip install -r requirements.txt
# Start the Flask API server
python app.py
⚠️ Note on First Startup: The backend will detect that the local model is missing and automatically query the TMDb API to build the dataset. This first-run sync retrieves ~2,200 movies with detailed credits in parallel and may take 2–3 minutes depending on your internet connection. Once completed, it creates movies_advanced.pkl so subsequent startups take less than 2 seconds.

2. Serve the Frontend
bash
# Navigate to frontend directory
cd ../frontend
# Start a simple HTTP server (Python 3)
python -m http.server 3000
Open your browser and navigate to http://localhost:3000.

⚙️ REST API Endpoints
Method	Endpoint	Query Parameters	Description
GET	/api/status	None	Returns the initialization state and total movie index count.
GET	/api/recommend	title (str), lang (str), n (int)	Main recommendation endpoint. Evaluates query similarity.
GET	/api/recommend-by-url	url (str), lang (str), n (int)	Extracts platform context, queries details, and returns matches.
GET	/api/search	q (str)	Real-time typo-tolerant autocomplete lookup.
GET	/api/languages	None	Retrieves list of active languages available in the model.
👥 Credits & Acknowledgments
Data Source: Metadata and movie posters supplied by The Movie Database (TMDb) API.
Built as part of the Group Project.
