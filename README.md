# 🧠 Nietzsche RAG Chatbot

> *"What does not kill me, makes me stronger"* - Now you can discuss this and more with the philosopher himself!

An AI-powered chatbot that lets you have philosophical conversations with Friedrich Nietzsche, built using Retrieval-Augmented Generation (RAG) and powered by Google's Gemini AI. This project processes Nietzsche's complete works and allows you to ask questions as if you're speaking directly to the great philosopher.

## 🌟 What Makes This Special?

This isn't just another chatbot - it's a window into one of history's most influential philosophical minds. By combining modern AI with Nietzsche's complete writings, you can:

- **Ask philosophical questions** and get responses in Nietzsche's authentic voice
- **Explore complex ideas** like the Übermensch, eternal recurrence, and the will to power
- **Engage with his works** including Thus Spake Zarathustra, Beyond Good and Evil, and more
- **Get contextual answers** backed by actual passages from his writings

## 📚 Knowledge Base

The chatbot is trained on Nietzsche's major works:

- **Thus Spake Zarathustra** - His masterpiece on the Übermensch and eternal recurrence
- **Beyond Good and Evil** - Critiques of traditional morality and religion
- **The Genealogy of Morals** - His analysis of the origin of moral values
- **The Birth of Tragedy** - His first major work on art and culture
- **The Antichrist** - His fierce critique of Christianity
- **Ecce Homo** - His philosophical autobiography
- **Human, All Too Human** - Reflections on human nature and society

## 🚀 How It Works

### The Magic Behind the Scenes

1. **Data Processing**: HTML versions of Nietzsche's works are cleaned and processed
2. **Intelligent Chunking**: Texts are split into meaningful chunks with overlap for context
3. **Vector Embeddings**: Each chunk is converted to embeddings using SentenceTransformers
4. **ChromaDB Storage**: Embeddings are stored in a vector database for fast similarity search
5. **Smart Retrieval**: When you ask a question, the most relevant passages are found
6. **AI Response**: Google's Gemini AI generates responses as if Nietzsche himself is speaking

### Technical Architecture

```
Your Question → Vector Search → Relevant Passages → Gemini AI → Nietzsche's Response
```

## 🛠️ Installation & Setup

### Prerequisites

- Python 3.7+
- Google Gemini API key

### Step 1: Clone the Repository

```bash
git clone https://github.com/SujitChintala/Nietzsche-RAG-Chatbot.git
cd Nietzsche-RAG-Chatbot
```

### Step 2: Install Dependencies

```bash
pip install numpy beautifulsoup4 tqdm pathlib langchain chromadb sentence-transformers google-generativeai python-dotenv
```

### Step 3: Set Up Your API Key

1. Get your Google Gemini API key from [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a `.env` file in the project root:

```env
GEMINI_FLASH_API_KEY=your_api_key_here
```

### Step 4: Run the Notebook

Open `main.ipynb` in Jupyter Notebook or VS Code and run all cells to:
- Process the philosophical texts
- Generate embeddings
- Set up the vector database
- Initialize the chatbot

## 💬 Usage Examples

### Starting a Conversation

```python
# Ask about his core philosophy
ask_nietzsche("What is the main theme of your philosophy?")

# Explore specific concepts
ask_nietzsche("What did you mean by 'God is dead'?")

# Discuss the Übermensch
ask_nietzsche("Tell me about the Übermensch concept")

# Ask about morality
ask_nietzsche("What are your thoughts on good and evil?")
```

### Sample Conversations

**You**: "What is the main theme of your philosophy?"

**Nietzsche**: *"The main theme of my philosophy is the revaluation of all values and the affirmation of life itself. I seek to overcome the nihilism that plagues modern man through the creation of new values, the cultivation of individual strength, and the embrace of our earthly existence rather than seeking solace in otherworldly promises."*

## 🏗️ Project Structure

```
Nietzsche-RAG-Chatbot/
├── main.ipynb              # Main notebook with complete workflow
├── README.md              # You are here!
├── data/                  # Original HTML files of Nietzsche's works
├── cleaned_data/          # Processed text files
│   ├── Beyond Good and Evil.txt
│   ├── Thus Spake Zarathustra.txt
│   ├── knowledge_base.txt    # Combined corpus
│   └── ... (other works)
├── embeddings/            # Stored vectors and chunks
│   ├── chunks.npy
│   └── embeddings.npy
└── chromadb/             # Vector database storage
```

## 🧪 How the RAG System Works

### 1. Data Preprocessing
- Cleans HTML files using BeautifulSoup
- Removes page numbers and formatting artifacts
- Combines all works into a single knowledge base

### 2. Text Chunking
- Uses LangChain's RecursiveCharacterTextSplitter
- Chunk size: 1000 characters with 200-character overlap
- Ensures context preservation across chunks

### 3. Embedding Generation
- Uses `all-MiniLM-L6-v2` model from SentenceTransformers
- Generates 384-dimensional vectors for each chunk
- Optimized for semantic similarity search

### 4. Vector Storage & Retrieval
- ChromaDB for efficient similarity search
- Retrieves top-k most relevant passages
- Provides context for AI response generation

### 5. Response Generation
- Google's Gemini-1.5-Flash for natural language generation
- Custom prompt engineering to maintain Nietzsche's voice
- Contextual responses based on retrieved passages

## 🎯 Key Features

- **Authentic Voice**: Responses maintain Nietzsche's philosophical style and terminology
- **Contextual Accuracy**: Answers are grounded in actual passages from his works
- **Fast Retrieval**: Vector search enables quick access to relevant content
- **Scalable Architecture**: Easy to extend with additional philosophical works
- **Interactive Learning**: Perfect for students, researchers, and philosophy enthusiasts

## 🔧 Customization

### Adjusting Response Length
Modify the `top_k` parameter in `ask_nietzsche()` to get more or fewer context passages:

```python
ask_nietzsche("Your question here", top_k=5)  # More context
```

### Changing the AI Model
You can experiment with different Gemini models:

```python
gemini_pro = genai.GenerativeModel('gemini-1.5-pro')  # More powerful model
```

### Adding New Texts
To include additional philosophical works:
1. Add HTML/text files to the `data/` folder
2. Run the data processing cells
3. Regenerate embeddings and update the vector database

## 🤝 Contributing

We welcome contributions! Whether it's:
- Adding more philosophical texts
- Improving the response quality
- Enhancing the user interface
- Bug fixes and optimizations

Please feel free to submit issues and pull requests.

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- **Friedrich Nietzsche** - For the profound philosophical insights
- **Project Gutenberg** - For providing free access to classical texts
- **Google AI** - For the powerful Gemini language model
- **ChromaDB** - For the excellent vector database
- **SentenceTransformers** - For semantic embeddings

## ⚠️ Disclaimer

This chatbot generates responses based on Nietzsche's writings but should not be considered as definitive philosophical guidance. The AI interpretation may not always perfectly capture the nuanced complexity of Nietzsche's thought. Use this as a tool for exploration and learning, not as a substitute for reading the original works.

---

*"You must have chaos within you to give birth to a dancing star."* - Friedrich Nietzsche

Ready to dance with ideas? Start your philosophical journey today! 🌟