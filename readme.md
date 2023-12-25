# Exposure, Embeddings, & Employment: Quantifying AI's Impact on Occupational Data

This is heavily based on Webb (2019)'s paper, The Impact of Artificial Intelligence on the Labor Market https://www.michaelwebb.co/webb_ai.pdf

# Codebase

In the provided Jupyter notebook, a comprehensive data processing and analysis pipeline is established, primarily focusing on patent data and its correlation with various occupations over time.

**Data Cleaning and Initial Processing:**
The process begins by importing the `pandas` library and defining the `process_raw_data` function. This function reads raw patent data from a CSV file, eliminates any duplicate entries, and concatenates the title and abstract of each patent into a new 'all_text' column. The publication dates are converted into datetime objects to extract the year, and the data is then split and saved into separate CSV files for each year. This step ensures that the dataset is clean, organized, and prepared for more detailed analysis.

**Embedding and Similarity Calculation:**
To explore the relationship between patents and occupations, the notebook employs OpenAI's GPT embedding API. It uses a combination of `pandas`, `openai`, and `concurrent.futures` for parallel processing to generate embeddings for each patent's text. With these embeddings, it calculates the cosine similarity between patents and various occupations. These similarities aim to quantify the relevance or association between the content of patents and the skills or knowledge required in different occupations.

**Statistical Analysis and Further Visualization:**
After calculating cosine similarities, the notebook performs statistical analysis to understand the distribution of similarities for each occupation across different years. Using `seaborn` and `matplotlib`, it creates distribution plots for cosine similarities, marking key statistics like mean and median. This section provides insights into how closely related the patents are to each occupation and how this relationship changes over time.

**Detailed Yearly and Occupational Analysis:**
In a more detailed analysis, the script calculates mean cosine similarity for each year and occupation, providing a granular view of the evolving landscape. It also determines the percentage of patents that fall under "exposure" and "high exposure" categories based on predefined thresholds of cosine similarity. These metrics are crucial for understanding the extent to which patents are related to occupational knowledge and trends in this relationship.

**Data Visualization:**
The script also focuses on visualizing changes in the employment landscape across various occupations from 1988 to 2023. It uses `matplotlib` and `pandas` to create line plots, showing the trends of medium exposure percentage over the years for selected occupations such as Accountants & Auditors, Administrative Law Judges, and Computer Network Architects. This visualization provides an initial understanding of how exposure to these occupations has evolved over time.

**Summary and Implications:**
The methodology and results presented in this notebook offer a multi-faceted view of the relationship between patents and occupations over time. By cleaning and processing the data, visualizing trends, embedding text for semantic analysis, and calculating statistical measures, the study provides a comprehensive understanding of how the nature of patents correlates with various occupations. The findings from this analysis can inform policymakers, researchers, and industry professionals interested in the dynamics between technological innovation (as represented by patents) and the labor market.

# Replication

To replicate, go to https://console.cloud.google.com/marketplace/details/google_patents_public_datasets/google-patents-public-data and enter the following query:

```sql
SELECT
  title.text AS title,
  abstract.text AS abstract,
  publication_number as patent_number,
  publication_date
FROM
  `patents-public-data.patents.publications`,
  UNNEST(title_localized) AS title,
  UNNEST(abstract_localized) AS abstract
WHERE
  (title.text LIKE '%neural network%'
  OR abstract.text LIKE '%neural network%'
  OR title.text LIKE '%machine learning%'
  OR abstract.text LIKE '%machine learning%'
  OR title.text LIKE '%deep learning%'
  OR abstract.text LIKE '%deep learning%'
  OR title.text LIKE '%artificial intelligence%'
  OR abstract.text LIKE '%artificial intelligence%')
  AND title.language = 'en'
  AND abstract.language = 'en'
  AND country_code = 'US'
```

Then, create a folder named "data", rename the .csv file to raw_patent_data.csv, and place it inside the folder.

Afterwards, the main Jupyter notebook should work as expected cell-by-cell.
