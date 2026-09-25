🛒 AI Shopping Assistant

An AI-powered shopping agent that helps users discover products, compare customer ratings, shop using natural-language requests, search for similar products from an uploaded image, and place orders through a conversational interface.

The project combines Streamlit, LangChain, Groq-hosted LLMs, and a local SQLite product database to create an end-to-end shopping assistant.

✨ Features

🔎 Natural-language product search

Ask for products using normal conversational language.

Example: I want organic honey under $20 with a 4.5+ rating.

💰 Price filtering

Supports maximum-price requirements when searching products.

🌱 Organic-product filtering

Users can request organic products specifically.

⭐ Review and rating analysis

Retrieves the average customer rating and review count for products from SQLite.

🖼️ Shop by Image

Upload a JPG, JPEG, PNG, or WEBP product image.

A vision model extracts product attributes and generates a search query.

The extracted information is then used to find similar products in the store.

🛍️ Conversational ordering

The agent can place an order after the user explicitly confirms the selected product.

Orders are stored in the SQLite database.

💬 Chat-based interface

Streamlit provides the user-facing shopping assistant and maintains conversation history.

🗃️ Local product database

SQLite stores products, reviews, and orders.

The included database setup script seeds the application with sample shopping data.

🧠 How the Agent Works

The agent uses a tool-based workflow:

                     ┌──────────────────────┐
                     │       User           │
                     │ Text / Product Image │
                     └──────────┬───────────┘
                                │
                                ▼
                     ┌──────────────────────┐
                     │     Streamlit UI     │
                     │      app.py          │
                     └──────────┬───────────┘
                                │
                                ▼
                     ┌──────────────────────┐
                     │   Shopping Agent     │
                     │  shopping_agent.py   │
                     └──────────┬───────────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
       ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
       │   Product   │   │   Ratings   │   │   Checkout  │
       │   Search    │   │     API     │   │    Tool     │
       └──────┬──────┘   └──────┬──────┘   └──────┬──────┘
              │                 │                 │
              └─────────────────┼─────────────────┘
                                ▼
                         ┌─────────────┐
                         │  SQLite DB  │
                         │  store.db   │
                         └─────────────┘

Image Search:
User Image → Vision LLM → Product Attributes → Product Search → Ratings → Results

🔧 Agent Tools

The shopping agent exposes four main tools:

1. search_products

Searches the product database by:

Product name

Description

Category

Maximum price

Organic status

2. get_rating

Retrieves:

Average product rating

Total number of reviews

3. checkout

Creates an order in the orders table and returns an order confirmation.

The agent is instructed not to call checkout until the user explicitly confirms the purchase.

4. describe_product_image

Uses a vision-capable LLM to analyze an uploaded product image and extract:

Product type

Search query

Organic status

Product description

The extracted information is then passed to product search.

🗄️ Database

The project uses SQLite through store.db.

The database contains three main tables:

products

Stores:

id

name

category

price

description

is_organic

reviews

Stores:

id

product_id

rating

reviewer_name

review_text

orders

Stores:

id

product_id

product_name

price

ordered_at

The database setup script provides sample products across categories including honey, oils, nuts and seeds, grains, tea, coffee, snacks, and dairy alternatives.

🛠️ Tech Stack

Technology

Purpose

Python

Core application language

Streamlit

Web-based chat interface

LangChain

Agent and tool orchestration

Groq

LLM inference

Qwen

Text reasoning model configured in the agent

Llama Vision

Image understanding model configured in the agent

SQLite

Local product, review, and order database

python-dotenv

Environment variable management

📁 Project Structure

shopagent/
│
├── app.py
├── shopping_agent.py
├── reviews_api.py
├── setup_db.py
├── store.db
├── README.md
│
├── app-checkpoint.py
├── reviews_api-checkpoint.py
└── reviews_api.cpython-314.pyc

File Overview

app.py
Streamlit frontend. It provides the chat interface, image upload functionality, conversation history, and calls the shopping agent.

shopping_agent.py
Core AI agent. It configures the Groq models and defines product search, rating, checkout, and image-analysis tools.

reviews_api.py
Provides functions for calculating product average ratings and review counts from SQLite.

setup_db.py
Creates the SQLite database, tables, sample products, and sample reviews.

store.db
Local SQLite database containing the application's product, review, and order data.

🚀 Getting Started

Prerequisites

Make sure you have:

Python 3.10+

A Groq API key

Internet access for Groq model inference

1. Clone the repository

git clone https://github.com/shashwat-90/shopagent.git
cd shopagent

2. Create a virtual environment

Windows:

python -m venv venv
venv\Scripts\activate

macOS/Linux:

python3 -m venv venv
source venv/bin/activate

3. Install dependencies

Install the packages required by the project:

pip install streamlit langchain langchain-groq python-dotenv

If you maintain a requirements.txt, you can instead use:

pip install -r requirements.txt

4. Configure the API key

Create a .env file in the project directory:

GROQ_API_KEY=your_groq_api_key_here

Do not commit your .env file or expose your API key publicly.

5. Initialize the database

Run:

python setup_db.py

This creates/updates store.db and populates the sample products and reviews.

6. Start the Streamlit application

streamlit run app.py

Then open the local Streamlit URL shown in your terminal.

💬 Example Queries

Try prompts such as:

I want organic honey under $20 with a 4.5+ rating.

Find me an organic product under $15.

Show me highly rated coffee.

I want an organic product below $10.

You can also upload a product image using Shop by Image and ask the agent to find similar products.

🛒 Ordering Flow

The agent follows a confirmation-based ordering process:

User Request
     ↓
Search Products
     ↓
Retrieve Ratings
     ↓
Filter Matching Products
     ↓
Show Products + IDs
     ↓
User Explicitly Confirms
     ↓
Checkout
     ↓
Order Saved in SQLite

The agent's system instructions explicitly require confirmation before checkout and require the product ID to come from the agent's previous product results.

🖼️ Image Search Flow

Upload Product Image
        ↓
Image Saved Temporarily
        ↓
Vision Model Analyzes Image
        ↓
Extract Product Type / Search Query / Organic Status
        ↓
Search Local Product Database
        ↓
Retrieve Ratings
        ↓
Display Similar Products

🔐 Security Notes

Store API keys in environment variables.

Never upload .env containing secrets to GitHub.

The application uses local SQLite data for its sample store.

Checkout should only be triggered after explicit user confirmation.

Uploaded images are temporarily stored for agent processing.

⚠️ Current Limitations

Product search is limited to the local SQLite catalog.

The application does not connect to live e-commerce marketplaces.

Checkout is a simulated/local database order rather than a real payment gateway.

Product recommendations are based on the available catalog and customer ratings.

Image similarity is implemented through vision-based attribute extraction followed by database search; it is not a dedicated visual-embedding similarity engine.

The configured LLM model names must be available to the Groq account being used. If a configured model is unavailable, update the model configuration in shopping_agent.py to a currently supported Groq model.

🔮 Possible Future Improvements

Add semantic/vector product search.

Add real e-commerce API integrations.

Add shopping cart functionality.

Add user accounts and personalized preferences.

Add order history and order tracking.

Add price comparison across multiple stores.

Add product embeddings for more accurate image/product similarity.

Add product images to search results.

Add payment gateway integration.

Deploy the application to a cloud platform.

Add automated tests and CI/CD.

📌 Project Highlights

This project demonstrates how an AI agent can combine:

LLMs + Tool Calling + SQL + Reviews + Computer Vision + Conversational UI

instead of simply generating text. The model can reason over a shopping request, call specialized tools, retrieve structured information, present matching products, and perform an action only after user confirmation.

👨‍💻 Author

Shashwat Mishra

GitHub: https://github.com/shashwat-90

📄 License

No license is currently specified in the repository. If you intend to distribute the project as open source, add an appropriate LICENSE file.
