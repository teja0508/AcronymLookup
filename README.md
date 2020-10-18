# AcronymLookup
Chandrateja Reddy  

This repo contains My project for Devopedia: We decided to create a system to identify acronym definitions from context, both because this was a problem that had not been solved before and because the system is something that we personally could use. Furthermore, the problem gave us the opportunity to get creative in our data collection. Below is an overview of our process with an emphasis on which files to run/modify in order to improve upon our work. To view our machine learning pipeline, read all code for training and testing, and view all data visualizations, please see the `.ipynb` files . To read more about the research specifics, see our paper, `Machine-Learning Approach for Cross-Domain Acronym Definition Identification.pdf`. 

## Setting Up The Environment
For convenience, we used a Python `virtualenv` to manage dependencies. To run our code, set up and activate a Python 2 `virtualenv`, then run `pip install -r requirements.txt`.

## Data Pre-Processing
A large part of the contribution made in this work was in the tools to create and automatically annotate a dataset of acronyms and their ground truth definitions, along with their context in the raw HTML.

### Download text from Wikipedia
In the `data` folder, run `python generateURLSpreadsheet.py`. This will recursively list all pages on Wikipedia under the categories listed in `categories.txt`. Each category will be created as its own `{Name of Category}.csv` file in the `categories` folder. If you want to change the number of subcategories to traverse, or the number of URLs per category, you can modify those as arguments to the script. By default the script runs as if you invoked it using `python generateURLSpreadsheet.py --recursionDepth=3 --numURLs=3000`. 

### Downloaded Files Of Devopedia Articles (Acronyms , Abbrevations)
devopediaAcronyms.v2248.json , devopediaArticles.v2248.json

(https://github.com/teja0508/AcronymLookup/blob/main/label-definitions/definitions_new.csv) .This definitions_new.csv file is combined Acronym data of Wikkipedia & Devopedia Articles


From there, you'll need to combine the files into one `data.csv` file. The rows will be automatically separated into train and test, but you'll need to shuffle the rows manually. In our case this was to ensure we put the lists of acronyms in the train set.

### Identify acronyms in text
Identification of acronyms in text is performed through a rule-based method. If you would like to use our code to perform the specific task of identifying acronyms in a body of text, please refer to the identifyAcronyms() method in `train.py`. 

### Extract definitions
Definitions are extracted from text using a constraint satisfaction problem (CSP) based method that we designed. Code to extract definitions can be viewed in the `label-definitions` folder, and further technical details are explained in the paper.


### Accessing the Database
If you are doing your own training, you'll need to set up Postgres. Once created, you can use the `setUpDB.sql` script in the `postgres-database` folder to set up the schema. If you need to reset the database or update the schema in the future, you can use `bash dbUtilities.bash updateSchema` to drop tables and reload schema from `setUpDB.sql`. To use the database, you can simply import `dbFunctions.py` and use the `AcronymDatabase` class.

## Training the Model
To train the model, simply run `python train.py`.

## Using the Model
To test against the testing set you have created, run `python test.py`. This will automatically evaluate the accuracy of the model. However, if you are just looking to run simple queries, pass a string to the `predict()` function in `serve.py`.

