# EpiRecipes Search Platform

EpiRecipes is a full-stack web application that allows users to search and filter recipes based on title, ratings, calories, and dietary preferences. The project utilizes Django for the backend, React for the frontend, and indexes data into OpenSearch.

## Project Overview

The goal of this project is to index a dataset of recipes into OpenSearch, allowing users to query and filter recipes efficiently. The platform includes a RESTful API built with Django and a modern UI built with React, which communicates with the backend to display recipe data in real-time.

## Features

Search Recipes: Search recipes by title, rating, and dietary preferences.
Filter Recipes: Filter recipes based on dietary preferences (vegetarian, gluten-free, etc.) and other attributes.
Recipe Details: View detailed information about a recipe, including calories, protein, fat, and sodium content.
Responsive UI: The React frontend provides a clean and intuitive user experience that works on all devices.

## Installation and Setup

### Step 1: Install and Configure OpenSearch

Install OpenSearch using Docker:
Docker simplifies OpenSearch cluster management. To install:

Pull the official OpenSearch images or sample Docker Compose files from the OpenSearch documentation.
Set up an admin password:
For Linux/macOS:

```
export OPENSEARCH_INITIAL_ADMIN_PASSWORD=<your-password>
```

For Windows Command Prompt:

```
set OPENSEARCH_INITIAL_ADMIN_PASSWORD=<your-password>
```

Create a docker-compose.yml file and run the following commands:

```
docker-compose up -d
docker-compose ps
```

Verify Cluster Health:
Check the health of your OpenSearch cluster:

```
curl -X GET "http://localhost:9200/_cluster/health?pretty"
```

### Step 2: Creating and Managing OpenSearch Indices

Create an Index:
Define the index and mappings for your recipe data:

```
PUT /epi_recipes
{
"mappings": {
"properties": {
"title": { "type": "text" },
"rating": { "type": "float" },
"calories": { "type": "float" },
"protein": { "type": "float" },
"fat": { "type": "float" },
"sodium": { "type": "float" },
"tags": {
"type": "object",
"properties": {
"vegetarian": { "type": "boolean" },
"gluten_free": { "type": "boolean" },
"dairy_free": { "type": "boolean" }
}
}
}
}
}
```

Search the Index:
Verify the index mappings:

```
curl -X GET "http://localhost:9200/epi_recipes/_mapping?pretty"
```

### Step 3: Ingesting Data into OpenSearch

Prepare the Data:
Convert the EpiRecipes dataset to JSON format and ensure the structure matches the index mappings.

Bulk Indexing:
Use the Bulk API to index the data:

```
curl -X POST "http://localhost:9200/\_bulk" -H "Content-Type: application/json" --data-binary @path:/to/epi_r_bulk.json
```

Verify Data Upload:
Check the indexed data:

```
curl -X GET "http://localhost:9200/epi_recipes/\_search?pretty"
```

### Step 4: Backend Development with Django

Set Up Django Project:

Install Django:

```
pip install django
```

Create a new Django project:

```
django-admin startproject epirecipes_backend
```

python manage.py startapp recipes
python manage.py migrate
python manage.py runserver
Create API Endpoints:
Build search endpoints to interact with OpenSearch:

```
curl "http://127.0.0.1:8000/api/search/?q=pasta&page=1&size=10"
```

Allow CORS:
Install django-cors-headers to allow frontend cross-origin requests:

```
pip install django-cors-headers
```

### Step 5: Frontend Development with React

Set Up React Project:
Create the frontend using Vite:

```npm create vite@latest
cd <your-project>
npm install
npm run dev
```

Install Additional Libraries:

```
npm install axios react-router-dom
npm install @fortawesome/react-fontawesome @fortawesome/free-brands-svg-icons
npm install react-icons --save
```

### Video Walkthrough

For a visual guide on setting up the project, watch the video on YouTube.

### Conclusion

By following these steps, you will have a full-stack web application that indexes the EpiRecipes dataset into OpenSearch. The Django backend provides the API for managing data and handling search queries, while the React frontend offers users a responsive interface to browse, search, and filter recipes.

Feel free to contribute, report issues, or request features through the repository.
