# LangGraph Research Assistant 🔍

**A simple iterative research agent built with LangGraph that searches the web and refines answers automatically.**

Built for Google Colab | Uses Groq (Free) | Real Web Search | No API Cost for Search

---

## 🎯 What Does This Do?

This notebook demonstrates **LangGraph** - a framework for building AI agents that can:
- ✅ Loop and retry (not just linear chains)
- ✅ Make decisions (should I search again?)
- ✅ Refine queries automatically
- ✅ Prevent infinite loops (max 3 iterations)

**Example Flow:**
```
Question → Search → Bad Answer? → Refine Query → Search Again → Good Answer ✓
```

---

## 🚀 Quick Start (Google Colab)

### 1️⃣ Open in Colab
Click here → [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1OS_WRHqcO5uNQ6CPtqxcD1u-q2BpAalL?usp=sharing)

### 2️⃣ Get Your Free Groq API Key
1. Go to [groq.com](https://console.groq.com/)
2. Sign up (it's free!)
3. Create an API key
4. Copy it

### 3️⃣ Add API Key to Colab
```python
# In Colab: Click 🔑 (Secrets) on left sidebar
# Add: Name = "GROQ", Value = "your-api-key-here"
```

### 4️⃣ Run All Cells
Press `Runtime > Run all` and watch it work!

---

## 📦 What Gets Installed

```bash
# This happens automatically in Cell 1:
!pip install langgraph langchain_groq langchain_community -U ddgs
```

**No OpenAI required!** Uses:
- **Groq** → Free fast LLM (llama-3.3-70b)
- **DuckDuckGo** → Free web search (no API key!)

---

## 🔧 How It Works

### 1. **State** (Shared Memory)
```python
question: str           # Original question
search_query: str       # Refined search query
search_results: str     # What we found
answer: str            # Final answer
needs_more_info: bool  # Should we loop?
iteration_count: int   # Safety counter (max 3)
```

### 2. **Nodes** (Workers)
- `analyze_question` → Creates/refines search query
- `search_web` → Searches DuckDuckGo
- `generate_answer` → Uses LLM to answer

### 3. **Edges** (Flow Control)
```
START → analyze → search → generate → ⚡ Decision Point
                              ↑              ↓
                              └─────(loop)───┘  or END
```

**The Magic:** Conditional edge decides:
- If `needs_more_info == True` AND `iterations < 3` → Loop back
- Otherwise → Stop and return best answer

---

## 💡 Example Output

```
❓ Question: How many episodes in One Piece anime?

🔍 Searching for: How many episodes in One Piece anime?
✅ Found results (length: 1237 chars)
🤖 Generating answer (iteration 1)...
⚠️  Answer incomplete, needs more research
🔄 Looping back for more research (iteration 1/3)

🔍 Searching for: One Piece anime total episodes count 2024
✅ Found results (length: 760 chars)
🤖 Generating answer (iteration 2)...
✅ Answer complete!

FINAL ANSWER:
The One Piece anime has released more than 1150 episodes as of November 2024.

Total iterations: 2
```

---

## 🎨 Customize It

### Change the Question
```python
question = "What are the latest AI trends?"  # Your question here
```

### Adjust Max Iterations
```python
# In should_continue_research function:
if state["needs_more_info"] and state["iteration_count"] < 5:  # Change 3 to 5
```

### Use Different LLM
```python
llm = ChatGroq(
    model="mixtral-8x7b-32768",  # Faster, cheaper
    # or "llama-3.3-70b-versatile"  # Smarter, slower
)
```

---

## 🧠 Why LangGraph vs LangChain?

| Feature | LangChain | LangGraph |
|---------|-----------|-----------|
| **Flow** | Linear (A→B→C) | Graph (loops, branches) |
| **Decisions** | No | Yes |
| **Retries** | Manual | Built-in |
| **Use Case** | Simple RAG | Complex agents |

**Example:**
- **LangChain:** `Question → Retrieve → Answer` ✅ Good for simple Q&A
- **LangGraph:** `Question → Search → Bad? → Retry → Good!` ✅ Better for complex tasks

---

## 📊 Real-World Use Cases

1. **Research Assistant** - Searches until it finds complete answer
2. **Customer Support** - Routes questions to right department
3. **Code Reviewer** - Analyzes → Finds issues → Suggests fixes → Loops
4. **Content Writer** - Research → Outline → Write → Self-edit → Publish

---

## 🛠️ Troubleshooting

**Error: "NameError: name 'GROQ' is not defined"**
- Solution: Add your Groq API key in Colab Secrets (🔑 icon on left)

**Search returns empty results**
- DuckDuckGo might be rate-limiting
- Try again in a few seconds

**Answer says "NEED_MORE_INFO" 3 times**
- Question might be too specific or recent
- Try rephrasing the question

---

## 📚 Learn More

- [LangGraph Docs](https://langchain-ai.github.io/langgraph/)
- [Groq API Docs](https://console.groq.com/docs)
- [LangChain Docs](https://python.langchain.com/)

---

## 🤝 Contributing

Found a bug? Have an improvement?
1. Fork this notebook
2. Make changes
3. Share your version!

---

## 📝 License

MIT License - Use freely in your projects!

----

### ⭐ If this helped you understand LangGraph, give it a star!

**Built with ❤️ for beginners learning AI agents**

---

## 🎓 Next Steps

After this, try:
1. Add memory (save conversation history)
2. Use multiple agents (researcher + writer)
3. Add human-in-the-loop (ask user for clarification)
4. Connect to your own data (RAG with iteration)
