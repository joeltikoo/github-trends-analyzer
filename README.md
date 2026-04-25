# GitHub Trends Analyzer

> Analyze GitHub repository trends using the GitHub API. Explore language popularity, domain classifications, rising repositories, and growth patterns through comprehensive data visualization.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=flat&logo=jupyter&logoColor=white)](https://jupyter.org/)

## Project Overview

This project provides a complete data analysis pipeline for exploring GitHub repository trends. By leveraging the GitHub REST API, it collects, processes, and visualizes data from thousands of repositories to uncover insights about:

- **Language Popularity** - Which programming languages dominate the open-source ecosystem
- **Domain Classification** - How repos are categorized (Web Dev, ML, DevOps, etc.)
- **Rising Stars** - Repositories gaining traction quickly
- **Language Trends** - How language popularity shifts over time
- **Repository Metrics** - Stars, forks, activity patterns, and growth rates
- **Language Combinations** - Which languages are commonly used together

## Sample Visualizations

The analysis generates publication-ready charts including:

- Language popularity distribution by repo count and total stars
- Heatmap of programming languages by domain
- Top repositories with their primary languages
- Rising repositories ranked by growth rate
- Language trend lines over recent years
- Language combination analysis
- Repository metrics correlations
- Domain distribution pie charts
- License usage patterns

## Features

### Data Collection
- Fetch top repositories using GitHub Search API
- Configurable search queries (stars threshold, language filters, etc.)
- Rate limit management and API throttling
- Secure token handling with environment variables

### Data Processing
- Clean and transform raw API responses
- Calculate derived metrics (stars per day, activity scores, rising scores)
- Intelligent domain classification based on topics and descriptions
- Language usage analysis and percentage calculations
- Repository age and growth rate computations

### Analysis & Insights
- Most popular programming languages
- Language specialization by domain (Web, ML, Mobile, DevOps, etc.)
- Rising repositories identification
- Language trend analysis over time
- Common language combinations
- Repository engagement metrics
- License distribution analysis
- Correlation matrix for key metrics

### Visualization
- 10+ publication-ready charts using Seaborn and Matplotlib
- Customizable color palettes and styles
- High-resolution exports (300 DPI)
- Interactive Jupyter notebook environment

## Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab
- GitHub account (for API token)
- Basic understanding of Python, Pandas, and data analysis

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/joeltikoo/github-trends-analyzer.git
cd github-trends-analyzer
```

### 2. Create Virtual Environment (Recommended)

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

**Required packages:**
- `requests` - API calls
- `pandas` - Data manipulation
- `numpy` - Numerical operations
- `matplotlib` - Plotting
- `seaborn` - Statistical visualizations
- `python-dotenv` - Environment variable management
- `jupyter` - Notebook environment

### 4. Set Up GitHub API Token

**Create a `.env` file** in the project root:

```bash
touch .env
```

Add your GitHub Personal Access Token:

```
GITHUB_TOKEN=your_github_token_here
```

**To get a GitHub token:**
1. Go to [GitHub Settings → Developer settings → Personal access tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Give it a name (e.g., "GitHub Trends Analyzer")
4. Select scopes: `public_repo` (or just `repo` for private repos)
5. Click "Generate token"
6. Copy the token and paste it in your `.env` file

> **Important:** Never commit your `.env` file to Git! It's already included in `.gitignore`.

## Usage

### Quick Start

1. Launch Jupyter Notebook:
```bash
jupyter notebook notebook.ipynb
```

2. Run the cells sequentially from top to bottom

3. Visualizations and CSV exports will be generated automatically

### Customization Options

**Adjust search parameters** (Cell 4):
```python
# Collect top repositories with custom filters
top_repos = search_repositories(
    query='stars:>5000 language:python',  # Custom query
    sort='stars',
    per_page=100,
    max_pages=10  # Adjust for more/fewer repos
)
```

**Common search queries:**
- `stars:>1000` - Repos with 1000+ stars
- `language:python` - Python repos only
- `created:>2024-01-01` - Repos created after Jan 1, 2024
- `topics:machine-learning` - ML-related repos
- Combine filters: `stars:>5000 language:javascript created:>2023-01-01`

**Modify domain classification** (Cell 8):
```python
# Add your own domain keywords
domains = {
    'Your Domain': ['keyword1', 'keyword2', 'keyword3'],
    # ... existing domains
}
```

**Change visualization styles:**
```python
# Adjust color palettes
sns.barplot(..., palette='viridis')  # Try: viridis, plasma, rocket, mako
```

## Output Files

The notebook generates several output files:

### CSV Exports
- `github_repos_raw.csv` - Complete raw dataset
- `language_summary.csv` - Language statistics summary
- `domain_summary.csv` - Domain classification summary  
- `rising_repositories.csv` - Top rising repos by growth rate

### Visualizations (PNG, 300 DPI)
- `popular_languages.png` - Language popularity charts
- `language_by_domain.png` - Heatmap of languages vs domains
- `top_repositories.png` - Top starred repositories
- `rising_repositories.png` - Rising repos by growth rate
- `language_trends.png` - Language trends over time
- `language_combinations.png` - Common language pairs
- `repository_metrics.png` - Multi-panel metrics analysis
- `domain_distribution.png` - Domain pie chart
- `correlation_matrix.png` - Metric correlations
- `license_distribution.png` - License usage patterns

## Analysis Examples

### Example 1: Find Python's Most Popular Domain
```python
python_repos = df_exploded[df_exploded['primary_language'] == 'Python']
print(python_repos['domains'].value_counts().head())
```

### Example 2: Identify Fast-Growing Repos
```python
# Repos with >100 stars/day
fast_growing = df_clean[df_clean['stars_per_day'] > 100]
print(fast_growing[['name', 'stars', 'stars_per_day', 'primary_language']])
```

### Example 3: Compare Language Communities
```python
# Average engagement by language
engagement = df_clean.groupby('primary_language').agg({
    'stars': 'mean',
    'forks': 'mean',
    'watchers': 'mean'
}).round(0)
```

## Sample Insights

Based on analyzing 1000+ top GitHub repositories:

- **JavaScript** and **Python** consistently rank as the most popular languages
- **Machine Learning** and **Web Development** dominate repository domains
- Repos combining **Python + Jupyter** are highly common in data science
- **TypeScript** shows strong growth in recent years
- **MIT License** is the most popular open-source license
- Rising repositories average **50+ stars/day** in their growth phase

## Troubleshooting

### API Rate Limit Exceeded
```python
# Check your remaining rate limit
remaining, reset_time = check_rate_limit()
print(f"Remaining: {remaining}, Resets at: {datetime.fromtimestamp(reset_time)}")
```

**Solutions:**
- Use a GitHub token (increases limit from 60 to 5000/hour)
- Reduce `max_pages` parameter
- Add `time.sleep()` between requests

### Timezone Errors
Ensure all datetime operations use UTC:
```python
df_clean['age_days'] = (pd.Timestamp.now(tz='UTC') - df_clean['created_at']).dt.days
```

### Memory Issues with Large Datasets
```python
# Reduce dataset size
top_repos = search_repositories(query='stars:>10000', max_pages=5)
```

## Contributing

Contributions are welcome! Here are some ideas:

- Add new analysis types (contributor networks, commit patterns)
- Improve domain classification accuracy
- Create interactive dashboards (Plotly, Streamlit)
- Add time-series forecasting for language trends
- Implement sentiment analysis on repo descriptions
- Add support for organization-level analysis

Check out `CONTRIBUTING.md` for more info on contributing.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Additional Resources (that I used)

- [GitHub API Documentation](https://docs.github.com/en/rest)
- [Pandas User Guide](https://pandas.pydata.org/docs/user_guide/index.html)
- [Seaborn Tutorial](https://seaborn.pydata.org/tutorial.html)
- [Data Analysis with Python Course](https://www.coursera.org/learn/data-analysis-with-python)
