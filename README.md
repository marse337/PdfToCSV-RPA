# PDF to CSV Converter

This script extracts text from PDF files located in a folder named `PDFs` and saves the processed data into CSV files in a folder named `CSVoutput`.

## Prerequisites

Ensure you have the following installed:

- **Python 3.x**
  
- Required Python packages:
  - `PyPDF2`: For extracting text from PDFs.
  - `csv`: For reading and writing CSV files.

To install the necessary packages, use the following command:

```bash
pip install PyPDF2
```

- **Python 3.x**
Script Overview
Directory Setup:

The script identifies its own location and checks for two directories:
PDFs (where it expects the input PDFs)
CSVoutput (where it saves the resulting CSVs)

For each PDF in the PDFs directory, the script extracts text using the PyPDF2 library.
Text Processing:

The extracted text is then transformed to fit a CSV format by splitting it into lines based on double spaces.
CSV Creation:

The processed data is saved as a CSV in the CSVoutput directory. The CSV retains the original PDF's name but with a .csv extension.
Output:

For each PDF processed, the script prints a message indicating the PDF's name and the CSV's save location.
Usage
Place your desired PDF files in the PDFs folder alongside the script.
Here is the following python script:

```python
import os
import PyPDF2
import csv

# Get the absolute path of the script's directory
script_directory = os.path.dirname(os.path.abspath(__file__))

# Folder paths where the PDFs are located and the CSVs will be saved
pdf_folder_path = os.path.join(script_directory, 'PDFs')
csv_folder_path = os.path.join(script_directory, 'CSVoutput')

# Make sure output folder exists
if not os.path.exists(csv_folder_path):
    os.makedirs(csv_folder_path)

# Loop through each PDF file
for filename in os.listdir(pdf_folder_path):
    if filename.endswith('.pdf'):
        pdf_path = os.path.join(pdf_folder_path, filename)
        csv_path = os.path.join(csv_folder_path, filename.replace('.pdf', '.csv'))

        # Step 1: Extract Text from PDF
        # Open PDF file
        pdfFile = open(pdf_path, 'rb')

        # Create pdf reader object
        pdfReader = PyPDF2.PdfReader(pdfFile)

        # Initialize a variable to store the extracted text
        extracted_text = ""

        # Loop through pages
        for pageNum in range(len(pdfReader.pages)):
            # Get the page object
            pageObj = pdfReader.pages[pageNum]
            # Extract text
            pageText = pageObj.extract_text()
            # Add extracted text to the overall text
            extracted_text += pageText

        # Close PDF file  
        pdfFile.close()

        # Step 2: Process the Extracted Text
        # Replace newline characters with spaces and condense into one line
        one_line_text = extracted_text.replace("\n", "")
        # Split the string at each occurrence of double space
        split_text = one_line_text.split("  ")
        # Create data matrix
        data_matrix = [split_text[i:i+7] for i in range(0, len(split_text), 7)]

        # Step 3: Write to CSV
        # Write the data to a CSV file
        with open(csv_path, 'w', newline='') as csvfile:
            csvwriter = csv.writer(csvfile)
            csvwriter.writerows(data_matrix)

        print(f'Text extracted from {filename} and saved to {csv_path}')
```



vbnet
Copy code

You can copy and paste the above Markdown content into your GitHub `README.md` f
