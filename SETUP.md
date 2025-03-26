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
```

## How to Get the Multilingual Amazon Reviews Corpus (MARC) dataset

Data Loading Amazon Reviews Multi Dataset and Display:  

More information about the dataset on [Amazon Reviews Multi](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

For the purpose of this project, the text.csv file has been choosen and has been resized 
```` python
import pandas as pd

# Assuming the dataset contains a CSV file
csv_file = os.path.join(path, "test.csv")  # Replace with the actual file name
df = pd.read_csv(csv_file)

# Display first five rows
print(df.head())

#Load the required library from Python
import csv
import pandas as pd

with open(csv_file, encoding='utf-8') as data_file:
    print(data_file.readline())
    x = data_file.read()
    print(x)
    data_file.seek(0)  # Reset file pointer to the beginning
    data_file_dict = csv.DictReader(data_file)
    for row in data_file_dict:
        print(row)

# Explore the variables that are enclosed in dataset 
dataset = pd.read_csv(csv_file)
gdp_frame_or = pd.DataFrame(dataset)
gdp_frame_or.columns.values  

print(dataset.head(5)) # first items of the dataset
print(dataset.columns.values) #View of the columns (variables) of the dataset
dataset.info()     #concise summary of the dataframe like the index dtype and columns, non-null values and memory usage 
dataset.describe() #Simple statistics of the numerical variables

size =dataset.size # dataframe.size
print(size)
shape =dataset.shape  # dataframe.shape
print(shape)

print(dataset.dtypes)

#Display Group by language,  product_category and get value counts of star ratings
star_presence_by_language = dataset.groupby(["language", "product_category", "stars"]).size().unstack(fill_value=0)
print(star_presence_by_language)

# Remove rows where language is 'ja' or 'zh'
dataset = dataset[~dataset["language"].isin(["ja", "zh"])]

# Verify if the filtering worked
print("Remaining languages:\n", dataset["language"].unique())

size =dataset.size # dataframe.size
print(size)
shape =dataset.shape  # dataframe.shape
print(shape)

columns_to_delete = ["Unnamed: 0", "review_id", "reviewer_id"]
for column in columns_to_delete:
	if column in dataset.columns:
		del dataset[column]

print(dataset.head())

#  select 2 items for each star rating (1 to 5) per language using the groupby and sample functions from Pandas.
import pandas as pd

# Assuming 'dataset' is already loaded and processed as 'star_presence_by_language'

star_presence_by_language = dataset.groupby(["language", "product_category", "review_title", "review_body", "stars"]).size().reset_index(name='count')

# Select 2 items per star rating (1-5) for each language
sampled_data = (
    star_presence_by_language
    .groupby(['language', 'stars'])
    .apply(lambda x: x.sample(n=2, random_state=42) if len(x) >= 2 else x)  # Ensures we get up to 2 items per group
    .reset_index(drop=True))

# Sorting the results
sampled_data = sampled_data.sort_values(by=['language', 'stars'])

# Display result
print(sampled_data)

#dataset_en_10.to_csv("filtered_dataset.csv", index=False)
sampled_data.to_csv("Resized_dataset.csv", index=False)
