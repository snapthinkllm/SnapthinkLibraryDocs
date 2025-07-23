# Document Analysis

Transform your documents into intelligent, searchable knowledge with SnapThink's powerful document analysis features.

## Supported File Types

SnapThink can process and analyze various document formats:

### 📄 **PDF Documents**
- Research papers and reports
- Books and manuals
- Forms and contracts
- Presentations

### 📊 **CSV Datasets**
- Spreadsheet data
- Survey results  
- Sales and financial data
- Scientific measurements

### 📝 **Text Files**
- `.txt` - Plain text documents
- `.md` - Markdown files
- Research notes and documentation

## How Document Analysis Works

### 1. **Upload Process**
1. Click **"📄 Upload Document"** button
2. Select your file
3. SnapThink automatically:
   - Extracts text content
   - Breaks it into searchable chunks
   - Creates AI embeddings for semantic search
   - Stores everything locally

### 2. **Intelligent Processing**
- **Text Extraction**: Converts PDFs and other formats to searchable text
- **Smart Chunking**: Breaks documents into logical sections
- **Embedding Creation**: Generates semantic vectors for AI understanding
- **Metadata Storage**: Preserves file information and structure

### 3. **Ready for Questions**
Once processed, you can ask the AI anything about your documents!

## Document Intelligence Features

### 🔍 **Semantic Search**
Ask questions in natural language:
- `"What does this paper say about climate change?"`
- `"Find mentions of machine learning algorithms"`
- `"What are the conclusions of this study?"`

### 📋 **Automatic Summarization**
Get instant summaries:
- `"Summarize this document in 3 bullet points"`
- `"What are the key findings?"`
- `"Give me the main topics covered"`

### 🔄 **Cross-Document Analysis**
Compare multiple documents:
- `"How do these two papers differ in their approach?"`
- `"Find common themes across all my uploaded documents"`
- `"Which document best explains topic X?"`

### 📊 **Data Insights** (CSV files)
For datasets, get automatic analysis:
- Column descriptions and data types
- Basic statistics and distributions
- Suggested visualizations
- Data quality insights

## Working with Different File Types

### 📄 PDF Documents

#### Best Practices
- **Text-based PDFs** work best (searchable text)
- **Scanned PDFs** may have limited text extraction
- **Large files** (>50MB) may take longer to process

#### Example Workflow
```
1. Upload: research-paper.pdf
2. Wait for processing (30-60 seconds)
3. Ask: "What methodology did the authors use?"
4. Follow up: "What were their main findings?"
5. Deep dive: "Explain the statistical analysis in detail"
```

### 📊 CSV Datasets

#### Automatic Processing
When you upload a CSV, SnapThink automatically:
- Detects column headers and data types
- Provides file path for Python analysis
- Suggests initial exploration questions

#### File Path Integration
SnapThink provides the exact file path for Python:
```python
import pandas as pd
df = pd.read_csv("/path/to/your/notebook/docs/data.csv")
```

#### Example Analysis
```
User: Upload sales-data.csv
AI: I've processed your CSV file with 1,000 rows and 5 columns:
- Date, Product, Quantity, Price, Region
- File path: /Users/.../notebook-123/docs/sales-data.csv

User: Show me sales trends by month
AI: [Creates Python code to analyze and visualize monthly trends]
```

### 📝 Text Files

#### Ideal for
- Research notes and documentation
- Code documentation (README files)
- Interview transcripts
- Meeting notes

#### Processing Features
- Preserves formatting and structure
- Maintains line breaks and paragraphs
- Searchable by keywords and concepts

## Advanced Analysis Techniques

### 📈 **Data Visualization**
For CSV files, request specific visualizations:
- `"Create a bar chart of sales by region"`
- `"Show the distribution of customer ages"`
- `"Make a time series plot of monthly revenue"`

### 🔎 **Targeted Search**
Find specific information:
- `"What does page 15 say about methodology?"`
- `"Find all mentions of 'statistical significance'"`
- `"Search for data about customer satisfaction"`

### 📝 **Content Extraction**
Extract structured information:
- `"List all the research questions from this paper"`
- `"Extract the key performance metrics"`
- `"What are all the cited authors?"`

### 🧮 **Quantitative Analysis**
For data-rich documents:
- `"Calculate the average response time"`
- `"What's the correlation between variables X and Y?"`
- `"Identify outliers in the dataset"`

## File Management

### 📂 **Notebook Organization**
- Each notebook has its own `docs/` folder
- Files are automatically organized by type
- Full file paths provided for Python access

### 💾 **Storage and Privacy**
- All files stored locally on your computer
- No uploading to external servers
- Files remain in your notebook even offline

### 🔄 **File Operations**
- **View**: Preview file contents in the sidebar
- **Remove**: Delete files from the notebook
- **Export**: Include files when exporting notebooks

## Best Practices

### 📋 **Document Preparation**

#### For PDFs
- ✅ Use text-based PDFs when possible
- ✅ Ensure good scan quality for scanned documents
- ✅ Break very large documents into sections

#### For CSV Files
- ✅ Include clear column headers
- ✅ Use consistent data formats
- ✅ Clean data before uploading when possible

#### For Text Files
- ✅ Use clear, descriptive filenames
- ✅ Structure content with headings when possible
- ✅ Include context and metadata

### 🎯 **Effective Questioning**

#### Start Broad, Get Specific
```
1. "What is this document about?" (overview)
2. "What are the main sections?" (structure)
3. "Tell me more about section X" (detail)
4. "How does this relate to concept Y?" (analysis)
```

#### Use Context Clues
- Reference specific sections: `"In the methodology section..."`
- Mention page numbers: `"What does page 10 discuss?"`
- Build on previous answers: `"Expand on that last point"`

#### Request Different Formats
- `"Summarize in bullet points"`
- `"Create a table of the key findings"`
- `"Explain this in simple terms"`

## Troubleshooting

### Common Issues

#### "Document failed to process"
- **File too large**: Try files under 50MB
- **Unsupported format**: Use PDF, CSV, TXT, or MD
- **Corrupted file**: Try re-saving or using a different version

#### "No text found in PDF"
- **Scanned document**: PDF might be image-only
- **Encrypted PDF**: Remove password protection first
- **Complex layout**: Simple text PDFs work best

#### "Can't find information"
- **Check file processing**: Ensure upload completed successfully
- **Rephrase question**: Try different wording
- **Be more specific**: Add context or page references

#### CSV not loading in Python
- **Check file path**: SnapThink provides the exact path
- **Encoding issues**: Try saving CSV as UTF-8
- **Large files**: May need to process in chunks

## Example Use Cases

### 📚 **Academic Research**
```
Upload: research-paper.pdf
Ask: "What's the research question and hypothesis?"
Follow: "How did they measure the variables?"
Analyze: "What were the limitations of this study?"
```

### 📊 **Business Analysis**
```
Upload: sales-report.csv
Ask: "What were our top-performing products?"
Visualize: "Show monthly revenue trends"
Insights: "Which regions need improvement?"
```

### 📋 **Document Review**
```
Upload: contract.pdf
Ask: "What are the key terms and conditions?"
Search: "Find all mentions of payment terms"
Summary: "What are the main obligations for each party?"
```

## Next Steps

- **[Python Environment](python-environment.md)** - Learn to combine document data with coding
- **[CSV Analysis Guide](../guides/csv-analysis.md)** - Detailed data analysis workflows
- **[PDF Analysis Guide](../guides/pdf-analysis.md)** - Specific techniques for PDF documents

**Ready to analyze your first document?** Upload a file and start asking questions!
