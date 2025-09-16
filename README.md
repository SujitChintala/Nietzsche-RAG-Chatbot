# Nietzsche RAG Chatbot

Ever wanted to have a conversation with Friedrich Nietzsche? Yeah, me too. So I built this chatbot that lets you do exactly that.

This project uses Retrieval-Augmented Generation (RAG) to create an AI assistant that responds as if it's Nietzsche himself. I fed it all his major works, and now you can ask him about anything from the Übermensch to his thoughts on morality. The responses are grounded in his actual writings, so you're getting authentic philosophical insights, not just generic AI responses.

## Project Architecture & Structure

The project is pretty straightforward - I wanted to keep it simple but effective. Here's how everything is organized:

```
Nietzsche-RAG-Chatbot/
├── main.ipynb              # The main notebook where everything happens
├── data/                   # Original HTML files of Nietzsche's works
├── cleaned_data/           # Processed text files ready for embedding
│   ├── Beyond Good and Evil.txt
│   ├── Thus Spake Zarathustra.txt
│   ├── knowledge_base.txt    # All works combined into one file
│   └── ... (other works)
├── embeddings/             # Saved numpy arrays of chunks and vectors
│   ├── chunks.npy
│   └── embeddings.npy
└── chromadb/              # Vector database for similarity search
```

I chose this structure because I wanted to separate each step of the process. Raw data is in `data/`, gets cleaned and saved to `cleaned_data/`, then gets converted to embeddings and stored in both numpy files and ChromaDB.
## Complete Workflow

Let me walk you through how this whole thing works, step by step:

### 1. Data Collection & Cleaning
I started with HTML files of Nietzsche's major works - seven books in total including "Thus Spake Zarathustra", "Beyond Good and Evil", "The Genealogy of Morals", and others. The HTML files had a lot of junk - page numbers, formatting artifacts, weird spacing. So I wrote a cleaning function that:

- Uses BeautifulSoup to extract just the text
- Removes page numbers and navigation elements  
- Cleans up whitespace and formatting issues
- Saves each book as a clean text file

### 2. Text Preparation
After cleaning, I combine all the individual books into one massive knowledge base file. This makes it easier to work with later. The combined file ends up being several hundred thousand words - basically Nietzsche's entire philosophical corpus in one place.

### 3. Chunking Strategy
Here's where it gets interesting. You can't just throw a massive text file at an AI model - it has token limits and would lose context. So I used LangChain's RecursiveCharacterTextSplitter to break the text into manageable chunks:

- 1000 characters per chunk (roughly 200-300 words)
- 200 character overlap between chunks (so context doesn't get lost at boundaries)
- Smart splitting on paragraphs, sentences, then words

This gives me about 3105 chunks that preserve the flow of Nietzsche's arguments while staying within model limits.

### 4. Embedding Generation
Each chunk gets converted into a vector using SentenceTransformers' `all-MiniLM-L6-v2` model. This creates 384-dimensional vectors that capture the semantic meaning of each passage. I chose this model because it's fast, efficient, and works great for similarity search.

The embedding process takes a few minutes since I'm processing thousands of chunks, but I save everything to numpy files so I only have to do it once.

### 5. Vector Database Setup
I used ChromaDB to store the embeddings and enable fast similarity search. When you ask a question, the system:

- Converts your question to an embedding using the same model
- Searches for the most similar chunks in the database
- Returns the top matches (3 passages)

### 6. Response Generation
Finally, I feed the retrieved passages to Google's Gemini-1.5-Flash model with a carefully crafted prompt that tells it to respond as Nietzsche. The prompt includes the relevant context and asks for concise, authentic responses in his philosophical style.

## Installation and Setup

Getting this running is pretty straightforward. Here's what you need to do:

### What You'll Need
- Python 3.7 or higher
- A Google Gemini API key (free from Google AI Studio)
- Disk space for all the data

### Step 1: Get the Code
```bash
git clone https://github.com/SujitChintala/Nietzsche-RAG-Chatbot.git
cd Nietzsche-RAG-Chatbot
```

### Step 2: Install Dependencies
I tried to keep dependencies minimal. You'll need:

```bash
pip install numpy beautifulsoup4 tqdm pathlib langchain chromadb sentence-transformers google-generativeai python-dotenv
```

### Step 3: Get Your API Key
1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a free account if you don't have one
3. Generate an API key
4. Create a `.env` file in the project root and add:

```env
GEMINI_FLASH_API_KEY=your_api_key_here
```

### Step 4: Run Everything
Open `main.ipynb` in Jupyter or VS Code and run all the cells. The first time will take a while because it's:
- Processing all the text files
- Generating thousands of embeddings
- Setting up the vector database

After that, everything is cached and responses are fast.

## How the System Works

Once everything is set up, using the chatbot is simple. There's one main function: `ask_nietzsche(question)`.

Here's what happens when you ask a question:

1. **Question Processing**: Your question gets converted to an embedding vector
2. **Similarity Search**: ChromaDB finds the most relevant passages from Nietzsche's works
3. **Context Building**: The top matches get combined into a context string
4. **AI Generation**: Gemini generates a response using the context and a prompt that tells it to respond as Nietzsche
5. **Response**: You get back an answer that sounds like it came from the philosopher himself

### Example Usage
```python
# Ask about core concepts
ask_nietzsche("What is the Übermensch?")

# Explore his critiques  
ask_nietzsche("Why did you say God is dead?")

# Discuss morality
ask_nietzsche("What's wrong with traditional moral values?")

# Personal questions
ask_nietzsche("What drove you to philosophy?")
```

The responses are surprisingly good. Because they're grounded in actual passages from his works, they capture his writing style, key concepts, and philosophical positions accurately.

## Customizations

I built this to be easily customizable. Here are some things you can tweak:

### Adjusting Context Size
If you want more detailed responses, increase the number of retrieved passages:

```python
ask_nietzsche("Your question", top_k=5)  # Gets more context
```

### Different AI Models
You can swap out Gemini for other models. I chose Gemini-1.5-Flash because it's fast and free, but you could use:

```python
gemini_pro = genai.GenerativeModel('gemini-1.5-pro')  # More powerful
```

### Adding More Texts
Want to include other philosophers or more of Nietzsche's works? Just:
1. Add the HTML/text files to the `data/` folder
2. Run the cleaning and processing cells again
3. Regenerate the embeddings

### Prompt Engineering
The prompt that tells the AI how to respond is in the `ask_nietzsche` function. You can modify it to change the response style, length, or focus.

### Chunk Size Optimization
Depending on your needs, you might want different chunk sizes:
- Smaller chunks (500-800 chars) for more precise matching
- Larger chunks (1500+ chars) for more context per retrieval

## Contributing

I'd love to see what other people do with this! Some ideas for contributions:

**Content Additions**:
- More Nietzsche works (letters, notebooks, fragments)
- Other philosophers (Schopenhauer, Kierkegaard, etc.)
- Better text cleaning and preprocessing

**Technical Improvements**:
- Web interface instead of just notebook cells
- Better chunking strategies
- Different embedding models
- Response quality improvements

**Features**:
- Conversation memory (so you can have longer discussions)
- Source citations (showing which specific passages informed each response)
- Multiple language support

If you want to contribute, just fork the repo and submit a pull request. I'm pretty open to different approaches and ideas.

**A Note on the Project**: This started as a weekend experiment because I was reading Nietzsche and thought "wouldn't it be cool to ask him questions directly?" It turned into something much more interesting than I expected. The responses are genuinely insightful and feel authentic to his philosophical voice.

The whole thing runs in a single Jupyter notebook because I wanted to keep it simple and accessible. No complex deployment, no web frameworks - just pure Python doing interesting things with text and AI.

---

*"One must have chaos within oneself to give birth to a dancing star."* - F. Nietzsche