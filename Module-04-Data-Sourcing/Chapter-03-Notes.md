# Document Processing

## Scraping PDFs

A significant portion of a data scientist's time is dedicated to sourcing required data and preparing it for machine learning models. This tutorial focuses on automating the scraping of multiple PDF files from a given URL and extracting tabular information from those downloaded PDFs for further analysis.

### Required Python Libraries
* **BeautifulSoup:** Used to parse the provided URL and download the required PDF content.
* **Tabula:** Used to read tabular content embedded within a PDF and convert it into a structured format, such as a CSV or Excel file.
* **OS:** Used to check for and create directories for saving the downloaded files.

#### 1. Setup and Directory Creation
The first step involves setting up the environment by mounting Google Drive, which serves as the storage destination for the downloaded PDFs. The script requires the input URL (for instance, a Premier League publications page hosting around 50 seasonal PDFs, including handbooks and schedules) and the target folder path within Google Drive. If the specified destination folder does not already exist, the `os` library is utilized to create it.

```python
import os
from bs4 import BeautifulSoup
import tabula

# Example setup based on the tutorial's description
save_location = "/content/drive/MyDrive/colab_notebooks/premier_league"
url = "https://www.premierleague.com/publication"
# Create the directory using the OS library if it doesn't exist
if not os.path.exists(save_location):
    os.makedirs(save_location)
```
#### 2. Scraping PDFs with BeautifulSoup

Instead of manually clicking and downloading each file, the script automates the process. The tutorial describes the following logic:
* A `BeautifulSoup` object is created to parse the target URL.
* The script searches through the parsed HTML object for all header anchor tags (links) that end with the .pdf extension.
* To properly name the saved files, the script utilizes a split function with a forward slash (/) on the URL string and isolates the very last segment (using the -1 index position) to act as the exact file name.
* A for loop iterates through every identified PDF link, copies the content, and saves it to the designated Google Drive folder.

```python
import requests

# Creating a soup object (Conceptual representation of the video's steps)
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')

# Loop through links, split the URL to get the file name, and save the content
for link in soap.select("a[href$='.pdf']"):
    # local files will have the same name as the file name in the url 
    filename = os.path.join(folder_location, link['href'].split('/')[-1])
    with open(filename,'wb') as f:
        f.write(request.get(urljoin(url,link['href'])).content)
```

#### 3. Extracting Tables with Tabula

After the PDFs are locally saved, tabula is used to extract structured table data into a data frame, CSV, or Excel file. By passing the specific page number (for example, page 18, which contains the final club standings for the season), Tabula can pull the required table information.

```python
form tabula import convert_into
combined_pdf = folder_location + "/pfdname.pdf"
convert_into(combined_pdf, folder_name + "/table_output.csv", output_format="csv", pages = 18, area= [[275,504,640,900]])
pd.read_csv(folder_name + "/table_output.csv")
```
# Convert PDFs to Markdown

Microsoft has released a new utility tool called MarkItDown, designed to convert a wide variety of file types into Markdown format . Markdown is a lightweight, plain-text markup language that is highly portable, platform-independent, and easy for humans to read and edit without needing to know complex coding languages. This conversion tool is highly beneficial for text analysis, indexing, and dataset generation.

**Supported File Formats**
*   **Documents:** PDF, Microsoft Word, PowerPoint, and Excel.
*   **Web & Data:** HTML (with special handling for Wikipedia-like sites), XML, JSON, and CSV.
*   **Media:** Audio files and images (supports Optical Character Recognition or OCR).
*   **Archives:** Compressed ZIP files.

To begin using the tool, you should set up a virtual environment and install the required packages. If you intend to use its advanced OCR capabilities via a Large Language Model (LLM), you will also need to install the OpenAI package.

## 1. Basic Document Conversion (Without LLM)


```bash
pip install markitdown openai
```

You can easily convert a standard document, such as a PDF, into Markdown without utilizing an LLM. You simply import the library, instantiate the client, pass the file path, and print the output. You can also display the formatted Markdown directly in a Jupyter Notebook using Python's IPython.display library.

```python
from markitdown import MarkItDown
markitdown = MarkItDown()
result = markitdown.convert('mypdf.pdf')
print(result.text_content)
# to convert it into markdown
form IPython.display import markdown.display
display(markdown(result.text_content))
```

## 2. Image OCR Conversion (With LLM)
For extracting text from images using Optical Character Recognition (OCR), MarkItDown can integrate with an LLM like OpenAI's gpt-4o. To do this, you must have an active OpenAI API key.
```python
from markitdown import MarkItDown
from openai import OpenAI
client = OpenAI()
md = markdown(llm_client = client , llm_model = 'gpt-4o')
result = md.convert('myImage.png')
print(result.text_content)
# to convert it into markdown
form IPython.display import markdown.display
display(markdown(result.text_content))
```
## 3. Command Line Interface (CLI) Usage
If you prefer not to write Python code or use a Jupyter Notebook, MarkItDown can be executed directly from your terminal as a CLI tool. You can pass the file directly, or utilize Linux pipe functionality to feed files into the utility.
```bash
### Basic CLI usage
markitdown your_document.pdf

### Using Linux pipe functionality
cat your_document.txt | markitdown

```


