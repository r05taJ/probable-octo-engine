# probable-octo-engine

UdaPlay is an AI-powered video game research assistant for a gaming analytics company. It combines a local semantic knowledge base with an agent that can evaluate evidence, search the web, analyze reviews, detect recent trends, and remember useful conversation context.

## What Is Included

The main implementation lives in [`starter/`](starter/):

- 30 game records across 25 platforms in [`starter/games/`](starter/games/)
- Qualitative launch-era `Reception` summaries for every game
- Persistent ChromaDB vector search using OpenAI embeddings
- Idempotent JSON indexing with stable record IDs
- Semantic-search output with ranked results and distance scores
- Retrieval-process dashboards showing the data flow from query to results
- A tool-enabled UdaPlay agent with short-term and persistent long-term memory

## Agent Tools

The Part 2 notebook provides:

- `retrieve_game`: semantic search over the local game collection
- `get_game_reception`: retrieve launch-era reception summaries
- `evaluate_retrieval`: assess whether local results answer a question
- `game_web_search`: Tavily fallback search
- `analyze_review_sentiment`: analyze sentiment, themes, confidence, and summary
- `detect_trending_games`: identify recent attention signals with source URLs

## Run The Project

1. Create and activate a Python virtual environment.
2. Install the project dependencies: ChromaDB, OpenAI, Tavily, `python-dotenv`, Pydantic, `matplotlib`, and `pdfplumber`.
3. Create `starter/.env` with:

	```text
	OPENAI_API_KEY="YOUR_KEY"
	CHROMA_OPENAI_API_KEY="YOUR_KEY"
	TAVILY_API_KEY="YOUR_KEY"
	```

4. Open the notebooks with `starter/` as the working directory.
5. Run [`Udaplay_01_starter_project.ipynb`](starter/Udaplay_01_starter_project.ipynb) first to synchronize the JSON records and run semantic search.
6. Run [`Udaplay_02_starter_project.ipynb`](starter/Udaplay_02_starter_project.ipynb) to create the tools, agent, dashboards, web fallback, sentiment analysis, trend detection, reception lookup, and long-term memory flow.

## Dashboards

The notebooks include visual retrieval traces:

```text
User Query -> Embed -> Retrieve -> Rank -> Results
```

The Part 2 dashboard also shows retrieval evaluation and whether the agent answers from local knowledge or routes to web search.

## Documentation

See [`starter/README.md`](starter/README.md) for the detailed project guide, data schema, examples, dependencies, and implementation notes.

## Security Notes

- Keep API keys in `starter/.env` and never commit that file.
- Generated ChromaDB data is local runtime state and is excluded from version control.
- Web trend results are evidence from current search coverage, not verified sales data.
- Reception fields are broad qualitative summaries rather than precise review scores.
