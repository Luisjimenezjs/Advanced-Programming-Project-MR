# Advanced-Programming-Project-MR
# Market Research AI Platform

**1. The demo**
I open the platform and create a new research project. I upload the dataset alongside the "questionnaire guide"—a mapping file that links each question to a specific section and research objective. Immediately, the platform builds the dashboard, splitting the visualizations according to those exact objectives.
I click *Generate Standard Report*. The platform makes a single API call to Gemini (optimizing token usage), analyzes the data against the objectives, and returns a deep analysis that is saved, visualized on screen, and ready to download in a standardized format.
Next, I navigate to the *Segmentation* tab. I select key study variables: age, gender, and three behavioral questions. I run the model; the platform executes a K-prototypes algorithm and returns 4 distinct consumer clusters. Finally, the AI reads the centroids of those clusters and automatically drafts a qualitative persona (e.g., "Cluster 1: Pragmatic Digital Youth").

**2. The shape**
*   **in:** Clean tabular data from Google BigQuery/AWS; questionnaire mapping guide (questions tied to objectives); manual selection of numerical and categorical variables for clustering.
*   **out:** Dashboards structured by research objective; AI-generated standard reports (downloadable and cached); machine learning segmentation profiles (K-means/K-prototypes) with AI-written narratives.
*   **on screen:** A Setup module for questionnaire mapping; a main view divided by Sections/Objectives; an Export module for the one-shot report; an Advanced Analytics module for variable selection and cluster visualization.

**3. The size**
*   **First useful version:**
    *   Google BigQuery or AWS database connection served via a FastAPI backend.
    *   Questionnaire ingestion module to dynamically organize the UI and automated SQL transformations.
    *   One-Shot Reporting: The AI processes findings by objective exactly once, caches the text in the database, and allows for standardized downloads without redundant token spend.
*   **Advanced Analytics Module:**
    *   Interface for selecting specific variables directly from the loaded study.
    *   Clustering execution: K-means for purely numerical data and K-prototypes for mixed data (highly common in survey statistics).
    *   AI Profiling: The LLM interprets the mathematical outputs (centroids) and drafts the qualitative profile of each cluster.
*   **Not this term:**
    *   Historical cross-referencing between multiple agency studies across different years.
    *   Predictive churn or purchase propensity models.

**4. How we would know it works**
*   Given an uploaded questionnaire guide, the UI automatically generates tabs for "Objective 1", "Objective 2", etc., displaying only the relevant data in each.
*   Given a click on "Generate Report", backend logs confirm the Cloud/Gemini API was called exactly once. If the page is refreshed or the report is downloaded later, the system retrieves the cached analysis without spending new tokens.
*   Given a mix of continuous variables (age) and categorical variables (gender, socio-economic status), the system correctly routes the data to the K-prototypes algorithm without throwing type errors, and the AI successfully explains the distinct traits separating the clusters.

**5. What could stop this**
*   **ML Data Type Complexity:** K-prototypes is strict about separating categorical and numerical variables. If the FastAPI backend fails to properly cast columns from BigQuery before feeding them to the model, the algorithm will crash.
*   **Cache and Token Management:** If the application state fails to recognize that a report was already generated, a UI bug could trigger the LLM prompt multiple times, unnecessarily inflating API costs.
*   **Questionnaire Variability:** Assuming the "questionnaire guide" will always arrive in perfect condition. We need a rigid, standardized upload template so the system knows exactly how to parse sections and objectives without breaking the data pipelines.
