# AI Chat Features

SnapThink's AI chat is powered by local language models, giving you intelligent assistance while keeping everything private.

## How It Works

SnapThink uses **Ollama** to run AI models directly on your computer:
- **No internet required** once models are downloaded
- **Complete privacy** - conversations never leave your device
- **Multiple model support** - choose the best model for your task

## Available Models

### Recommended Models

#### **Llama 3.2 (3B)** - Best for most users
```bash
ollama pull llama3.2:3b
```
- **Use for**: General conversation, document Q&A, basic coding
- **RAM needed**: 4-6GB
- **Speed**: Fast responses

#### **Llama 3.2 (8B)** - More powerful
```bash
ollama pull llama3.2:8b
```
- **Use for**: Complex analysis, detailed explanations, advanced coding
- **RAM needed**: 8-12GB  
- **Speed**: Slower but more capable

#### **CodeLlama (7B)** - Coding specialist
```bash
ollama pull codellama:7b
```
- **Use for**: Python programming, code review, debugging
- **RAM needed**: 6-8GB
- **Speed**: Medium, optimized for code

### Specialized Models

#### **Mistral (7B)** - Balanced performance
```bash
ollama pull mistral:7b
```
- Good balance of speed and capability
- Excellent for reasoning tasks

#### **Qwen2.5 (7B)** - Multilingual support
```bash
ollama pull qwen2.5:7b
```
- Strong multilingual capabilities
- Good for international documents

## Chat Capabilities

### 🤖 General Conversation
Ask anything you'd ask ChatGPT:
- Explain complex topics
- Get writing assistance
- Brainstorm ideas
- Answer factual questions

**Example:**
```
User: Explain machine learning to a 10-year-old
AI: Imagine teaching a computer to recognize cats in photos...
```

### 📄 Document-Aware Chat
When you upload documents, the AI can:
- Answer questions about document content
- Summarize key points
- Find specific information
- Compare information across documents

**Example:**
```
User: What are the main conclusions in my research paper?
AI: Based on your uploaded paper, the main conclusions are:
1. The proposed method improves accuracy by 15%...
```

### 🐍 Python Code Execution
The AI can write and run Python code:
- Data analysis and visualization
- Mathematical calculations
- File processing
- Statistical analysis

**Example:**
```
User: Calculate the correlation between age and income in my dataset
AI: I'll analyze the correlation for you:

[Python code block]
import pandas as pd
import numpy as np

# Load your dataset
df = pd.read_csv("your_data.csv")
correlation = df['age'].corr(df['income'])
print(f"Correlation: {correlation:.3f}")

[Result: Correlation: 0.742]
```

## Advanced Features

### Context Memory
The AI remembers your entire conversation:
- Reference previous questions and answers
- Build on earlier analysis
- Maintain context across the session

### Multi-Document Analysis
Upload multiple documents and ask comparative questions:
- "Compare the findings in paper A vs paper B"
- "What themes appear across all my documents?"
- "Find contradictions between these sources"

### Code Persistence
Python variables and imports persist throughout the conversation:
- Load data once, use it multiple times
- Build complex analysis step by step
- Create and reuse custom functions

## Best Practices

### 💡 Writing Effective Prompts

#### Be Specific
❌ **Vague**: "Analyze my data"
✅ **Specific**: "Create a histogram of customer ages and identify the most common age group"

#### Provide Context
❌ **No context**: "What does this mean?"
✅ **With context**: "In the context of my sales report, what does the 15% increase in Q3 indicate?"

#### Break Down Complex Tasks
❌ **Too complex**: "Analyze everything and create a complete report"
✅ **Step by step**: 
1. "First, show me summary statistics for my dataset"
2. "Now create visualizations for the top 3 variables"
3. "Finally, identify any correlations or patterns"

### 🎯 Getting Better Results

#### Use Follow-up Questions
- "Can you explain that in simpler terms?"
- "What are the implications of this finding?"
- "Show me a different type of visualization"

#### Request Specific Formats
- "Summarize this in bullet points"
- "Create a table comparing these options"
- "Write this as a formal conclusion"

#### Ask for Code Comments
- "Add comments to explain the code"
- "Show me step-by-step what this code does"
- "Explain the logic behind this approach"

## Model Selection Guide

### When to Use Each Model

#### **Llama 3.2:3b** - Daily driver
- ✅ Quick questions and answers
- ✅ Document summarization
- ✅ Simple data analysis
- ✅ Basic coding tasks
- ❌ Complex reasoning
- ❌ Very long documents

#### **Llama 3.2:8b** - Heavy lifting
- ✅ Complex analysis tasks
- ✅ Detailed explanations
- ✅ Advanced coding
- ✅ Large document processing
- ❌ When you need speed
- ❌ On low-RAM systems

#### **CodeLlama:7b** - Programming focus
- ✅ Writing Python scripts
- ✅ Code debugging
- ✅ Algorithm explanations
- ✅ Data science workflows
- ❌ General conversation
- ❌ Non-coding tasks

### Switching Models
1. Click the model name in the top bar
2. Select from your downloaded models
3. The conversation context carries over

## Performance Tips

### Speed Optimization
- **Use smaller models** for quick tasks
- **Close other applications** to free up RAM
- **Use GPU acceleration** if available

### Quality Optimization
- **Use larger models** for complex tasks
- **Provide clear, detailed prompts**
- **Break complex requests into steps**

### Memory Management
- **Restart conversations** if they get very long
- **Create new notebooks** for different projects
- **Monitor system RAM usage**

## Troubleshooting

### Common Issues

#### "No models available"
1. Install Ollama: `curl -fsSL https://ollama.com/install.sh | sh`
2. Download a model: `ollama pull llama3.2:3b`
3. Restart SnapThink

#### Slow responses
- Try a smaller model (3b instead of 8b)
- Close other applications
- Check available RAM

#### Poor quality answers
- Use a larger model
- Provide more context in your prompts
- Break complex questions into parts

#### Model won't download
- Check internet connection
- Ensure sufficient disk space
- Try downloading manually: `ollama pull model-name`

## Next Steps

- **[Document Analysis](document-analysis.md)** - Learn to work with uploaded files
- **[Python Environment](python-environment.md)** - Master the coding capabilities
- **[CSV Analysis Guide](../guides/csv-analysis.md)** - Practical data analysis examples

**Pro tip:** Start with simple questions to get familiar with your model's style, then gradually work up to more complex tasks!
