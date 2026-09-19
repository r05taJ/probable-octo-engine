# UdaPlay - AI Game Research Agent Project

## Project Overview
UdaPlay is an AI-powered research agent for video game research. It combines a local ChromaDB knowledge base with OpenAI-powered reasoning, Tavily web search, structured evaluation, visual retrieval dashboards, and persistent conversation memory.

## Project Structure

### Part 1: Offline RAG (Retrieval-Augmented Generation)
In this part, you'll build a Vector Database using ChromaDB to store and retrieve video game information efficiently.

Features:
- Persistent ChromaDB collection using OpenAI embeddings
- Idempotent indexing with `upsert`, safe to rerun
- Semantic search with ranked results and distance scores
- 30 game records across 25 platforms
- Qualitative launch-era `Reception` summaries for every game
- Retrieval-process dashboard showing query, embedding, retrieval, ranking, and results

Each game document contains:
  - Name
  - Platform
  - Genre
  - Publisher
  - Description
  - Year of Release
  - Reception

### Part 2: AI Agent Development
Build an intelligent agent that combines local knowledge with web search capabilities.

The agent has the following capabilities:
1. Answer questions using the local game knowledge base
2. Evaluate whether retrieved documents are sufficient
3. Search the web when local evidence is insufficient
4. Summarize launch-era game reception
5. Analyze sentiment and themes in user-provided reviews
6. Detect games receiving recent attention using web evidence
7. Maintain short-term conversation state
8. Store and recall long-term memories across sessions
9. Return structured outputs validated with Pydantic models
10. Visualize the retrieval and routing process

Available tools:
1. `retrieve_game`: Search the vector database for game information
2. `get_game_reception`: Retrieve qualitative launch reception summaries
3. `evaluate_retrieval`: Assess the quality of retrieved results
4. `game_web_search`: Search the web for additional information
5. `analyze_review_sentiment`: Classify review sentiment, score, confidence, themes, and summary
6. `detect_trending_games`: Find recent trend signals and supporting source URLs

## Requirements

### Environment Setup
Create a `.env` file with the following API keys:
```
OPENAI_API_KEY="YOUR_KEY"
CHROMA_OPENAI_API_KEY="YOUR_KEY"
TAVILY_API_KEY="YOUR_KEY"
```

### Project Dependencies
- Python 3.11+
- ChromaDB
- OpenAI Python SDK
- Tavily Python SDK
- python-dotenv
- Pydantic
- matplotlib
- pdfplumber

### Directory Structure
```
project/
├── starter/
│   ├── games/           # JSON files with game data
│   ├── lib/             # Custom library implementations
│   │   ├── llm.py       # LLM abstractions
│   │   ├── messages.py  # Message handling
│   │   ├── ...
│   │   └── tooling.py   # Tool implementations
│   ├── Udaplay_01_starter_project.ipynb  # Part 1 implementation
│   └── Udaplay_02_starter_project.ipynb  # Part 2 implementation
```

## Getting Started

1. Create and activate a virtual environment
2. Install required dependencies
3. Set up your `.env` file with necessary API keys
4. Follow the notebooks in order:
  - Run `Udaplay_01_starter_project.ipynb` from the `starter/` directory. It loads the JSON records, synchronizes the persistent collection, runs semantic search, and renders the retrieval dashboard.
  - Run `Udaplay_02_starter_project.ipynb` from the `starter/` directory. It creates the tools, agent, dashboards, web fallback, reception lookup, sentiment analysis, trend detection, and long-term memory flow.

The notebooks use relative paths such as `games/` and `chromadb/`, so set the notebook working directory to `starter/`.

### Indexing and Semantic Search

Part 1 synchronizes every JSON file into the `udaplay` collection using stable filename IDs. Re-running the indexing cell updates existing records instead of creating duplicates.

The reusable search function is:

```python
search_games("Which PC games are role-playing games?", n_results=5)
```

Results include the game metadata, description, reception summary, and Chroma distance. Lower distance indicates a closer semantic match.

### Dashboards

Part 1 includes `show_retrieval_process_dashboard()`, which visualizes:

`User Query -> Embed -> Retrieve -> Rank -> Results`

Part 2 includes `show_agent_retrieval_dashboard()`, which additionally shows retrieval evaluation and whether the agent answers locally or uses web fallback.

## Testing Your Implementation

After completing both parts, test your agent with questions like:
- "When was Pokémon Gold and Silver released?"
- "Which one was the first 3D platformer Mario game?"
- "Was Mortal Kombat X released for PlayStation 5?"

Additional examples:
- "How was Half-Life 2 received when it was released?"
- Provide a game review to `analyze_review_sentiment`
- Use `detect_trending_games(timeframe="past month")` for recent trend signals

## Advanced Features

The advanced section uses a persistent Chroma collection named `long_term_memory`. It stores question-and-answer interactions as memory fragments, recalls semantically relevant memories, and injects them into later agent queries.

## Notes
- Do not commit `.env` or generated ChromaDB data.
- Web trend results depend on current Tavily search coverage and should not be treated as verified sales data.
- Reception summaries are broad qualitative descriptions, not direct review-score measurements.
- Run the notebooks in order after installing dependencies and configuring API keys.
