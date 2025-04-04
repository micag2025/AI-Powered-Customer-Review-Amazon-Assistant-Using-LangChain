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
SCARPER_API_KEY=your_scaroer_api_key
```

## How to Get the Multilingual Amazon Reviews Corpus (MARC) dataset

More information about the dataset are available on [Amazon Reviews Multi](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi) and in the pubblication of [Phillip Keung, Yichao Lu, György Szarvas and Noah A. Smith. “The Multilingual Amazon Reviews Corpus.” In Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing, 2020](https://aclanthology.org/2020.emnlp-main.369/)

For the purpose of this project, the test.csv file has been chosen and resized.
```` python
import pandas as pd
import os
import csv

# Path to the CSV file
csv_file = os.path.join(path, "test.csv")  # Replace with the actual file name

# Load the dataset into a DataFrame
df = pd.read_csv(csv_file)

# Display the first five rows of the dataset
print(df.head())

# Reading the CSV file using the csv module
with open(csv_file, encoding='utf-8') as data_file:
    print(data_file.readline())  # Read and print the first line
    x = data_file.read()
    print(x)
    data_file.seek(0)  # Reset file pointer to the beginning
    data_file_dict = csv.DictReader(data_file)
    for row in data_file_dict:
        print(row)

# Explore the dataset
dataset = pd.read_csv(csv_file)

# Display the first five items of the dataset
print(dataset.head())

# Display the columns (variables) of the dataset
print(dataset.columns.values)

# Concise summary of the DataFrame
dataset.info()

# Simple statistics of the numerical variables
dataset.describe()

# Size and shape of the DataFrame
print(f"Size: {dataset.size}")
print(f"Shape: {dataset.shape}")

# Data types of the columns
print(dataset.dtypes)

# Group by language, product_category and get value counts of star ratings
star_presence_by_language = dataset.groupby(["language", "product_category", "stars"]).size().unstack(fill_value=0)
print(star_presence_by_language)

# Remove rows where language is 'ja' or 'zh'
dataset = dataset[~dataset["language"].isin(["ja", "zh"])]

# Verify if the filtering worked
print("Remaining languages:\n", dataset["language"].unique())

# Size and shape of the filtered dataset
print(f"Size: {dataset.size}")
print(f"Shape: {dataset.shape}")

# Delete unnecessary columns
columns_to_delete = ["Unnamed: 0", "review_id", "reviewer_id"]
for column in columns_to_delete:
    if column in dataset.columns:
        del dataset[column]

print(dataset.head())

# Select 2 items for each star rating (1 to 5) per language using groupby and sample functions from Pandas
star_presence_by_language = dataset.groupby(["language", "product_category", "review_title", "review_body", "stars"]).size().reset_index(name='count')

# Select 2 items per star rating (1-5) for each language
sampled_data = (
    star_presence_by_language
    .groupby(['language', 'stars'])
    .apply(lambda x: x.sample(n=2, random_state=42) if len(x) >= 2 else x)  # Ensures we get up to 2 items per group
    .reset_index(drop=True)
)

# Sorting the results
sampled_data = sampled_data.sort_values(by=['language', 'stars'])

# Display the result
print(sampled_data)

# Save the resized dataset to a CSV file
sampled_data.to_csv("Resized_Amazon_Reviews_Multi_Dataset.csv", index=False)
