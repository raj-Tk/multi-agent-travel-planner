# Real-Time AI Multi-Agent Travel Planner

An AI-powered travel planning application that uses a LangGraph multi-agent workflow to search flights, discover hotels, generate itineraries, and produce a final personalized travel plan. The project includes a Streamlit frontend, PostgreSQL-backed memory, and integrations with Groq, Tavily, and AviationStack APIs.

## Features

- Multi-agent workflow built with LangGraph StateGraph
- Real-time flight data retrieval using AviationStack API
- Hotel and destination research using Tavily Search
- AI-generated itinerary and final travel recommendations using Groq LLaMA 3.3
- PostgreSQL checkpointing for persistent user sessions
- Streamlit frontend with live agent progress updates
- Markdown export for generated travel plans

## Tech Stack

- Python
- LangGraph
- LangChain
- Groq LLaMA 3.3
- Tavily Search API
- AviationStack API
- PostgreSQL
- Streamlit
- psycopg
- python-dotenv

## Project Structure

```text
Travel_Agent_AI/
|-- main.py
|-- frontend.py
|-- tools/
|   |-- flight_tool.py
|   `-- tavily_tool.py
|-- travel_plans/
|-- .env
`-- README.md
```

## How It Works

The application uses four agents:

1. **Flight Agent**: Fetches flight information using AviationStack.
2. **Hotel Agent**: Searches hotel and destination information using Tavily.
3. **Itinerary Agent**: Generates a structured travel itinerary using an LLM.
4. **Final Agent**: Combines flights, hotels, and itinerary into a final travel plan.

LangGraph manages the agent flow, while PostgreSQL checkpointing stores thread-based session memory.

## Environment Variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
AVIATIONSTACK_API_KEY=your_aviationstack_api_key
DATABASE_URL=postgresql://username:password@localhost:5432/langgraph_memory
```

## Database Setup

Install PostgreSQL and create a database:

```sql
CREATE DATABASE langgraph_memory;
```

Make sure the `DATABASE_URL` in `.env` points to this database.

## Installation

Create and activate a virtual environment:

```powershell
python -m venv langraph_env3
.\langraph_env3\Scripts\activate
```

Install dependencies:

```powershell
pip install langgraph langchain langchain-groq langchain-community langchain-tavily psycopg[binary] psycopg_pool python-dotenv tavily-python requests streamlit
```

## Run the Streamlit Frontend

From the project root, run:

```powershell
.\langraph_env3\Scripts\python.exe -m streamlit run frontend.py --server.port 8501
```

Open the app in your browser:

```text
http://localhost:8501
```

Keep the terminal open while using the app.

## Run from CLI

You can also run the backend flow directly:

```powershell
.\langraph_env3\Scripts\python.exe main.py
```

Then enter a travel request, for example:

```text
Plan a 5-day Dubai trip with flights, hotels, and sightseeing.
```

## Debugging Notes

If PostgreSQL checkpoint setup fails with:

```text
CREATE INDEX CONCURRENTLY cannot run inside a transaction block
```

make sure the psycopg connection uses autocommit:

```python
_conn = psycopg.connect(DATABASE_URL, autocommit=True)
```

If the frontend shows "This page can't be reached", the Streamlit server is not running. Start it again using:

```powershell
.\langraph_env3\Scripts\python.exe -m streamlit run frontend.py --server.port 8501
```

