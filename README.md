<h1 class="center-heading">AI Travel Planner</h1>

### State Management Lifecycle:
1. **`START` & Initialization**: The user query is captured and injected into the global `TravelState` container alongside initial message structures.
2. **`flight_agent`**: Targets the `search_flights` custom tool to query the **AviationStack API**, extracting airline names, departure/arrival hubs, and flight statuses.
3. **`hotel_agent`**: Builds a specialized query string and invokes the **Tavily Search API** to fetch the top 5 highly relevant hotel matches.
4. **`itinerary_agent`**: Ingests the unified state parameters (`user_query`, `flight_results`, `hotel_results`) and sends them to the Llama 3.3 foundation model using an optimized systemic persona to formulate a time-boxed itinerary.
5. **`final_agent`**: Synthesizes the generated components into a clean, unified, user-facing narrative.
6. **State Checkpointing**: The entire computational state is compiled, serialized, and stored into a persistent **PostgreSQL** schema matching the unique `thread_id` session key.

---

## 🔥 Key Features

### 🤖 Modular Multi-Agent Framework
Instead of using a single monolithic prompt, the system segregates business logic into dedicated, purpose-driven computational blocks. This improves maintainability, scales easily, and minimizes token usage.

### 🔌 Live Travel API Integrations
* **AviationStack Integration**: Fetches real-time airline flight records, schedules, and operational status metrics.
* **Tavily Search Engine Optimization**: Leverages semantic web search filtering to obtain rich snippets, URLs, and descriptions of accommodation alternatives without web scraping bottlenecks.

### 💾 Production State Persistence
Utilizes LangGraph’s `PostgresSaver` checkpointer. By indexing sessions via a unique `thread_id`, the system supports:
* Session resumption and conversation recovery.
* Auditing trails of LLM token usages and call frequencies.
* Seamless scaling into multi-user web environments.

---

## 🛠️ Tech Stack

* **Frameworks**: LangGraph, LangChain Core
* **LLM Engine**: ChatGroq (`llama-3.3-70b-versatile`)
* **Persistence Layer**: PostgreSQL (`PostgresSaver`)
* **External APIs**: AviationStack API, Tavily Search API
* **Environment**: Python 3.10+, `python-dotenv`

---

## 📂 Project Structure

```bash

ARCHITECTURAL WORKFLOW
+---------------+
   |     START     |
   +-------+-------+
           |
           v
+--------------------+
|    flight_agent    | <--- Fetches real-time flight metrics via AviationStack API
+-------+------------+
           |
           v
+--------------------+
|    hotel_agent     | <--- Gathers context-aware accommodation data via Tavily API
+-------+------------+
           |
           v
+--------------------+
|  itinerary_agent   | <--- Generates optimized day trip schedules using Llama 3.3
+-------+------------+
           |
           v
+--------------------+
|    final_agent     | <--- Compiles final comprehensive presentation view
+-------+------------+
           |
           v
   +---------------+
   |      END      |
   +---------------+
