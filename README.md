# Travel Assistant Chatbot

A sophisticated conversational AI assistant for international travel planning, built with LangChain and OpenAI. This chatbot helps users find flights, get visa information, and answer travel policy questions through natural language interaction.

## 🌟 Features

- **Natural Language Flight Search**: Parse complex travel queries and find matching flights
- **Intelligent Query Understanding**: Extract structured data from conversational input
- **RAG-Powered Information Retrieval**: Answer visa and policy questions using document knowledge base
- **Multi-Alliance Flight Database**: Search across Star Alliance, SkyTeam, and oneworld airlines
- **Flexible Search Criteria**: Handle preferences for airlines, layovers, prices, and travel classes
- **Conversational Interface**: Friendly, context-aware responses with fallback handling

## 🏗️ System Architecture

### Core Components

1. **Travel Assistant (`main.py`)**: Main orchestrator using LangChain agents
2. **Flight Search Engine (`utils/flight_search.py`)**: Handles flight filtering and search logic
3. **RAG Engine (`utils/rag_engine.py`)**: Retrieval-augmented generation for policy questions
4. **Query Parser (`utils/query_parser.py`)**: Natural language to structured data conversion
5. **Prompt Templates (`prompts/system_prompts.py`)**: Modular prompt engineering

### Data Flow

```
User Query → Query Parser → Intent Classification → Tool Selection → Response Generation
     ↓              ↓              ↓                ↓              ↓
 Natural Lang. → Structured → Flight Search → Flight Results → Formatted Response
                   Data    → RAG Query    → Policy Info   → 
```

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- OpenAI API key
- Git

### Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd kavak-travel-assistant
   ```

2. **Create virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**:
   
   **Recommended**: Create a `.env` file in the project root:
   ```
   OPENAI_API_KEY=your-openai-api-key-here
   ```
   
   **Alternative**: Export manually (temporary):
   ```bash
   export OPENAI_API_KEY="your-openai-api-key-here"
   ```
   
   💡 **Note**: The application automatically loads environment variables from `.env` file using python-dotenv.

5. **Run the application**:
   ```bash
   python main.py
   ```

## 📊 Sample Usage

### Flight Search Examples

```
User: "Find me a round-trip to Tokyo in August with Star Alliance airlines only. I want to avoid overnight layovers."

Assistant: I found several Star Alliance flights to Tokyo in August that avoid overnight layovers:

✈️ Turkish Airlines (Star Alliance)
   Route: Dubai → Tokyo
   Dates: 2024-08-15 - 2024-08-30
   Price: $950
   Layovers: Istanbul
   Refundable: Yes

✈️ Singapore Airlines (Star Alliance)
   Route: Dubai → Tokyo
   Dates: 2024-08-17 - 2024-09-01
   Price: $1100
   Layovers: Singapore
   Refundable: Yes
```

### Visa Information Examples

```
User: "Do I need a visa to visit Japan with a UAE passport?"

Assistant: UAE passport holders can enter Japan visa-free for up to 30 days for tourism purposes. Your passport must be valid for at least 6 months from the date of entry. This applies to tourism, short-term business meetings, and transit.

📚 Sources:
- Document chunk 2

⚠️ Important: Please verify this information with official government sources before traveling.
```

### Policy Questions

```
User: "What's the refund policy for refundable tickets?"

Assistant: For refundable tickets:
- Can be canceled up to 48 hours before departure
- Processing fee: 10% of ticket price (minimum $50, maximum $200)
- Refund processing time: 7-14 business days
- Refunds are issued to the original payment method

For Business and First class tickets, you can cancel up to 24 hours before departure with only a 5% processing fee.
```

## 🧠 Prompt Engineering Strategy

### System Prompts

Our prompt engineering follows a modular approach with specialized prompts for different components:

1. **Travel Assistant Prompt**: Main conversational agent with tool routing
2. **Flight Search Prompt**: Structured data extraction for flight queries
3. **RAG Prompt**: Context-aware information retrieval
4. **Query Parsing Prompt**: Natural language to JSON conversion
5. **Error Handling Prompt**: Graceful error management

### Behavior Shaping Techniques

- **Role Definition**: Clear assistant persona as travel expert
- **Constraint Setting**: Specific guidelines for tool usage
- **Context Preservation**: Maintaining conversation flow
- **Fallback Strategies**: Handling ambiguous or incomplete queries
- **Output Formatting**: Consistent, user-friendly response structure

## 🔧 Agent Logic

### Tool Selection Strategy

The agent uses a decision tree approach for tool selection:

```python
if "flight" or "search" or "find" in query:
    → use search_flights tool
elif "visa" or "policy" or "refund" in query:
    → use get_visa_info tool
elif query is ambiguous:
    → use parse_travel_query tool first
else:
    → provide general travel advice
```

### Context Management

- **Session Memory**: Maintains conversation context
- **Query History**: References previous searches
- **Preference Learning**: Adapts to user preferences
- **Error Recovery**: Learns from failed queries

## 🔍 RAG Implementation

### Vector Database Setup

- **Embedding Model**: OpenAI text-embedding-ada-002
- **Vector Store**: FAISS for efficient similarity search
- **Chunk Strategy**: Recursive text splitting with 1000 char chunks, 200 char overlap
- **Retrieval**: Top-3 similarity search with score threshold

### Knowledge Base

- **Visa Rules**: Country-specific entry requirements
- **Flight Policies**: Cancellation, refund, and change policies
- **Health Requirements**: Vaccination and health certificate info
- **Travel Insurance**: Coverage recommendations

## 📁 Project Structure

```
kavak-travel-assistant/
├── main.py                 # Main application entry point
├── requirements.txt        # Python dependencies
├── README.md              # Project documentation
├── .env.example           # Environment variables template
├── data/
│   ├── flights.json       # Mock flight database
│   └── visa_rules.md      # Travel policy knowledge base
├── prompts/
│   ├── __init__.py
│   └── system_prompts.py  # All prompt templates
├── utils/
│   ├── __init__.py
│   ├── flight_search.py   # Flight search engine
│   ├── rag_engine.py      # RAG implementation
│   └── query_parser.py    # Natural language parser
└── optional/
    ├── streamlit_app.py   # Web interface (optional)
    └── gradio_app.py      # Alternative web UI (optional)
```

## 🎯 Advanced Features

### Intelligent Query Parsing

- **Date Recognition**: Handles relative dates ("next month", "in August")
- **Airline Alliance Mapping**: Recognizes alliance preferences
- **Layover Preferences**: Understands "direct", "no overnight layovers"
- **Price Constraints**: Extracts budget limitations
- **Travel Class Detection**: Identifies economy/business/first preferences

### Flight Search Capabilities

- **Fuzzy City Matching**: Handles variations in city names
- **Date Flexibility**: ±3 days tolerance for better results
- **Multi-Criteria Filtering**: Combines multiple search parameters
- **Result Ranking**: Sorts by price and convenience
- **Alternative Suggestions**: Provides options when no exact matches

### Conversational Design

- **Proactive Clarification**: Asks for missing information
- **Context Awareness**: References previous queries
- **Error Recovery**: Suggests alternatives for failed searches
- **Friendly Tone**: Maintains helpful, professional demeanor
- **Rich Formatting**: Uses emojis and structured output

## 🧪 Testing

### Test Queries

1. **Complex Flight Search**:
   ```
   "I need a business class flight from Dubai to Tokyo in late August, preferably Star Alliance, under $1500, with no more than one layover"
   ```

2. **Visa Information**:
   ```
   "What are the visa requirements for US citizens traveling to Japan?"
   ```

3. **Policy Questions**:
   ```
   "Can I get a refund if I need to cancel my flight due to a medical emergency?"
   ```

4. **Ambiguous Query**:
   ```
   "Help me plan a trip to Europe next month"
   ```

## 📊 Sample Outputs

### Flight Search Example

**Input**: "Find me a round-trip to Tokyo in August with Star Alliance airlines only. I want to avoid overnight layovers."

**Output**:
```
🛫 Flight Search Results

Found 2 flights matching your criteria:

✈️ Flight 1:
   Airline: Turkish Airlines (Star Alliance)
   Route: Dubai → Tokyo
   Dates: 2024-08-15 to 2024-08-30
   Price: $950 USD
   Class: Economy
   Layovers: Istanbul (no overnight)
   Refundable: Yes

✈️ Flight 2:
   Airline: Singapore Airlines (Star Alliance)
   Route: Dubai → Tokyo
   Dates: 2024-08-17 to 2024-09-01
   Price: $1,100 USD
   Class: Business
   Layovers: Singapore (no overnight)
   Refundable: Yes

📊 Search Statistics:
- Total flights found: 2
- Average price: $1,025
- Star Alliance flights: 2
- Refundable options: 2
```

### Visa Information Example

**Input**: "Do I need a visa for Japan with a UAE passport?"

**Output**:
```
🌍 Visa Information for Japan

Good news! UAE passport holders can enter Japan visa-free for up to 30 days for tourism purposes. 
Your passport must be valid for at least 6 months from the date of entry.

This applies to:
- Tourism
- Business meetings (short-term)
- Transit

General Requirements:
- Valid passport with at least 6 months validity
- Return or onward ticket
- Sufficient funds for the duration of stay
- No criminal record

📚 Sources:
- Document chunk visa_japan_001

⚠️ Important: Please verify this information with official government sources before traveling.
```

### Policy Question Example

**Input**: "What's the refund policy for cancellations?"

**Output**:
```
💰 Flight Refund Policy

Refund policies vary by ticket type:

**Economy Class (Refundable tickets):**
- Can be canceled up to 48 hours before departure
- Subject to a 10% processing fee
- Refund processing time: 7-14 business days
- Refunds issued to original payment method

**Business and First Class:**
- Can cancel up to 24 hours before departure
- Only 5% processing fee
- Faster processing times

**Non-refundable tickets:**
- Generally no refunds available
- May qualify for travel credits in some cases
- Medical emergencies may be exceptions (documentation required)

📚 Sources:
- Document chunk refund_policy_001

⚠️ Important: Please verify this information with official government sources before traveling.
```

## 🔧 Configuration

### Environment Variables

- `OPENAI_API_KEY`: Your OpenAI API key (required)
- `OPENAI_MODEL`: Model to use (default: gpt-3.5-turbo)
- `EMBEDDING_MODEL`: Embedding model (default: text-embedding-ada-002)
- `MAX_TOKENS`: Maximum tokens per response (default: 1000)
- `TEMPERATURE`: Model temperature (default: 0.7)

### Customization Options

- **Flight Database**: Replace `data/flights.json` with your data
- **Knowledge Base**: Update `data/visa_rules.md` with current policies
- **Prompts**: Modify prompts in `prompts/system_prompts.py`
- **Search Logic**: Customize filtering in `utils/flight_search.py`

## 🚀 Deployment Options

### Local Development
```bash
python main.py
```

### Streamlit Web App
```bash
streamlit run streamlit_app.py
```

### Gradio Interface
```bash
python gradio_app.py
```

### Docker Deployment
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

## 🔍 Troubleshooting

### Common Issues

1. **OpenAI API Key Error**:
   - Ensure `OPENAI_API_KEY` is set correctly
   - Check API key validity and billing status

2. **No Flight Results**:
   - Verify `data/flights.json` exists and is valid
   - Check search criteria for typos
   - Try broader search parameters

3. **RAG Not Working**:
   - Ensure `data/visa_rules.md` exists
   - Check FAISS installation
   - Verify embedding model access

4. **Import Errors**:
   - Ensure all dependencies are installed
   - Check Python version compatibility
   - Verify virtual environment activation

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- LangChain team for the excellent framework
- OpenAI for powerful language models
- FAISS team for efficient vector search
- The travel industry for inspiration

## 📞 Support

For questions or issues:
- Create an issue in the repository
- Check the troubleshooting section
- Review the documentation

---

**Note**: This is a technical demonstration project. Always verify travel information with official sources before making travel decisions.
