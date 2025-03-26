# Setup Instructions

## How to Get Your Own OpenAI API Key 

To install the OpenAI Python library:

```bash
pip install openai
```

The library needs to be configured with your account's secret key, which is available on the website.

You can either set it as the `OPENAI_API_KEY` environment variable before using the library:

```bash
export OPENAI_API_KEY='sk-...'
```

Or, set `openai.api_key` to its value:

```python
import openai
openai.api_key = "sk-..."
```

## To set up an API key in an .env file, follow these steps:

1. **Create or Open the .env File**:
   - If you don’t have a `.env` file in your project directory, create one. Ensure it is in the root folder of your project.
2. **Add Your API Key to the .env File**:
   - Open the `.env` file and add your API key in the following format:
     ```plaintext
     API_KEY=your_api_key_here  # replace your_api_key_here with the actual key.
     ```
3. **Load the .env File in Your Code**:
   - For Python (using `dotenv` package):
     ```bash
     pip install python-dotenv
     ```
   - Then, in your Python script:
     ```python
     from dotenv import load_dotenv
     import os
     load_dotenv()  # Load environment variables from .env file
     api_key = os.getenv("API_KEY")
     print(api_key)  # Test if it's loaded correctly
     ```

### Example API keys in .env file:
```plaintext
OPENAI_API_KEY=your_openai_api_key
LANGSMITH_API_KEY=your_langsmith_api_key
TAVILY_API_KEY=your_tavily_api_key



## How to Get the Multilingual Amazon Reviews Corpus (MARC) dataset

Data Loading Amazon Reviews Multi Dataset and Display:
More information about the dataset on [Amazon Reviews Multi](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

For the purpose of this project, the original dataset has been resized 


