# 🎬 Movie Recommender System

A machine learning-based web application that intelligently recommends movies similar to your selection. Built with Streamlit, this system uses cosine similarity to analyze movie characteristics and suggest the top 5 most similar films.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Technical Stack](#technical-stack)
- [Installation & Setup](#installation--setup)
- [Project Structure](#project-structure)
- [Usage Guide](#usage-guide)
- [How It Works](#how-it-works)
- [Required Files](#required-files)
- [Troubleshooting](#troubleshooting)
- [API Information](#api-information)

---

## 🎯 Project Overview

The Movie Recommender System is an intelligent movie discovery tool that:
- Analyzes your movie selection
- Compares it against a database of movies using similarity metrics
- Displays the 5 most similar movies with their posters
- Fetches real movie poster images from The Movie Database (TMDB) API

This system is perfect for discovering movies based on your preferences without navigating through endless catalogs.

---

## ✨ Features

✅ **Smart Recommendations** - Uses machine learning similarity metrics to find related movies  
✅ **Beautiful UI** - Clean, intuitive interface built with Streamlit  
✅ **Movie Posters** - Displays poster images for each recommended movie  
✅ **Large Database** - Works with thousands of movies  
✅ **Fast Performance** - Instantly generates recommendations  
✅ **Robust Error Handling** - Gracefully handles API failures and missing data  
✅ **Retry Logic** - Implements automatic retries for network requests  

---

## 🛠️ Technical Stack

| Component | Technology |
|-----------|------------|
| **Frontend Framework** | Streamlit |
| **Language** | Python 3.x |
| **ML Model** | Cosine Similarity |
| **Data Processing** | Pandas, Pickle |
| **API Integration** | TMDB API (requests library) |
| **Deployment** | Heroku (via Procfile) |

---

## 📦 Installation & Setup

### Prerequisites

- Python 3.7 or higher
- pip (Python package manager)
- Internet connection (for API calls)

### Step 1: Clone or Download the Project

```bash
cd movie-recommender-system
```

### Step 2: Create a Virtual Environment

```bash
python -m venv .venv
```

### Step 3: Activate Virtual Environment

**On Windows:**
```bash
.venv\Scripts\activate
```

**On macOS/Linux:**
```bash
source .venv/bin/activate
```

### Step 4: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 5: Verify Required Data Files

Ensure these files exist in the project root:
- `movies_dict.pkl` - Dictionary of all movies
- `similarity.pkl` - Pre-computed similarity matrix
- `app.py` - Main application file

### Step 6: Run the Application

```bash
streamlit run app.py
```

The application will open in your default browser at `http://localhost:8501`

---

## 📂 Project Structure

```
movie-recommender-system/
├── app.py                    # Main Streamlit application
├── movies_dict.pkl           # Pickled movie data dictionary
├── similarity.pkl            # Pre-computed similarity matrix
├── requirements.txt          # Python package dependencies
├── setup.sh                  # Deployment configuration script
├── Procfile                  # Heroku deployment configuration
└── README.md                 # This file
```

### File Descriptions

| File | Purpose |
|------|---------|
| **app.py** | Main application logic with Streamlit UI and recommendation engine |
| **movies_dict.pkl** | Serialized dictionary/dataframe containing movie titles and IDs |
| **similarity.pkl** | Pre-computed cosine similarity matrix between all movies |
| **requirements.txt** | Lists all Python package dependencies |
| **setup.sh** | Configures Streamlit for Heroku deployment |
| **Procfile** | Tells Heroku how to run the application |

---

## 🚀 Usage Guide

### Starting the Application

1. **Activate your virtual environment** (if not already active)
2. **Run the app:**
   ```bash
   streamlit run app.py
   ```
3. **Wait for the browser to open** at `http://localhost:8501`

### Using the Recommender

1. **Select a Movie** - Choose any movie from the dropdown list
2. **Click "Recommend"** - Hit the recommendation button
3. **View Results** - See the 5 most similar movies with their posters
4. **Try Again** - Select another movie and repeat!

### Example

```
Select Movie: "The Shawshank Redemption"
Click: [Recommend Button]
Results:
  1. The Green Mile
  2. Forrest Gump
  3. The Godfather
  4. Pulp Fiction
  5. Fight Club
```

---

## 🧠 How It Works

### The Recommendation Algorithm

1. **User Selects a Movie** - Chooses from the available movie database
2. **Find Movie Index** - Locates the movie in the pre-loaded data
3. **Calculate Similarity** - Retrieves the similarity scores for that movie
4. **Sort by Similarity** - Ranks all other movies by similarity score
5. **Get Top 5** - Selects the 5 most similar movies (excluding the original)
6. **Fetch Posters** - Retrieves poster images from TMDB API
7. **Display Results** - Shows titles and posters in a beautiful layout

### Data Files Explained

- **movies_dict.pkl**: Contains movie information (titles, IDs, etc.)
- **similarity.pkl**: Matrix where `similarity[i][j]` = similarity score between movie i and j

### Similarity Metric

The system uses **cosine similarity** which:
- Measures the angle between movie feature vectors
- Ranges from 0 (completely different) to 1 (identical)
- Captures genre, ratings, cast, plot, and other attributes

---

## 📥 Required Files

### Movies Dictionary (`movies_dict.pkl`)

This file should contain or be convertible to a Pandas DataFrame with at least:
```python
{
    'title': ['Movie 1', 'Movie 2', ...],
    'movie_id': [123, 456, ...],
    # Other columns can be included
}
```

### Similarity Matrix (`similarity.pkl`)

A 2D numpy array/matrix where:
- Shape: (number_of_movies, number_of_movies)
- Values: Similarity scores between movies (0 to 1)
- Should be symmetric: `similarity[i][j] == similarity[j][i]`

---

## ⚠️ Troubleshooting

### Issue: Import Error for pickle files

**Problem:** `FileNotFoundError: movies_dict.pkl not found`

**Solution:** 
- Ensure `movies_dict.pkl` and `similarity.pkl` are in the same directory as `app.py`
- Verify file names are spelled correctly (case-sensitive on Linux/Mac)

### Issue: API Errors when fetching posters

**Problem:** `Error fetching poster for movie`

**Solution:**
- Check your internet connection
- Verify the TMDB API key is valid
- The app has built-in retry logic that will attempt 3 times
- Placeholder images will be shown if the API fails

### Issue: Empty dropdown list

**Problem:** No movies appear in the selectbox

**Solution:**
- Verify `movies_dict.pkl` is not corrupted
- Try rebuilding the pickle files from your source data
- Check that the DataFrame has a 'title' column

### Issue: Streamlit not found

**Problem:** `ModuleNotFoundError: No module named 'streamlit'`

**Solution:**
```bash
pip install -r requirements.txt
```

### Issue: Application won't start

**Problem:** Port 8501 already in use

**Solution:**
```bash
streamlit run app.py --server.port 8502
```

---

## 🌐 API Information

### The Movie Database (TMDB) API

**Endpoint Used:**
```
https://api.themoviedb.org/3/movie/{movie_id}?api_key={API_KEY}&language=en-US
```

**Response Example:**
```json
{
  "poster_path": "/path/to/poster.jpg",
  "title": "Movie Title",
  "release_date": "2024-01-01",
  ...
}
```

**Rate Limiting:**
- The app implements automatic retries with exponential backoff
- Handles HTTP 429 (Too Many Requests) errors gracefully

**Network Features:**
- Custom headers to mimic browser requests
- 10-second timeout per request
- Automatic retry on network failures (up to 3 times)

---

## 🚀 Deployment (Heroku)

To deploy on Heroku:

1. **Install Heroku CLI** - Download from heroku.com
2. **Login to Heroku:**
   ```bash
   heroku login
   ```
3. **Create a new app:**
   ```bash
   heroku create your-app-name
   ```
4. **Push to Heroku:**
   ```bash
   git push heroku main
   ```

The `setup.sh` and `Procfile` handle configuration automatically.

---

## 📝 License

This project uses The Movie Database (TMDB) API. Please refer to TMDB's API terms of service.

---

## 💡 Tips for Best Results

- The quality of recommendations depends on the quality of your training data
- More movies in the database = better recommendations
- The similarity matrix should be pre-computed for faster performance
- Consider updating movie data periodically to keep recommendations fresh

---

## 📞 Support

For issues or questions:
1. Check the Troubleshooting section above
2. Verify all required files are present
3. Ensure dependencies are correctly installed
4. Check internet connection for API calls

---

**Happy Movie Discovering! 🍿**
