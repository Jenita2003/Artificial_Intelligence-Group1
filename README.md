MARKET BASKET ANALYSIS PROGRAM EXPLANATION:

import pandas as pd

• This line imports the Pandas library and gives it the alias "pd," making it 
easier to reference Pandas functions in the code.

data=pd.read_csv('assignment1_data.csv',on_bad_lines='skip')

• This line reads a CSV file named 'assignment1_data.csv' into a Pandas 
DataFrame called 'data.' The 'on_bad_lines' parameter is set to 'skip,' which 
means that any lines with parsing errors in the CSV file will be skipped.

print(data)
• This line prints the content of the 'data' DataFrame to the console, displaying 
the data from the CSV file.

data.describe()

• This line generates summary statistics for the numerical columns in the 
DataFrame using the describe() method. It will display statistics such as 
count, mean, standard deviation, minimum, and maximum for each 
numerical column.

pd.set_option('display.max_columns', None)

pd.set_option('display.expand_frame_repr', False)

• These lines set display options for Pandas. The first line sets the maximum 
number of columns to be displayed in the output to 'None,' which means all 
columns will be shown. The second line prevents Pandas from wrapping 
long lines when displaying DataFrames.

print(data)

• This line prints the 'data' DataFrame again, but this time with the updated 
display options applied.

data = pd.read_csv('assignment1_data.csv')

• This line re-reads the 'assignment1_data.csv' file into the 'data' DataFrame 
without specifying the 'on_bad_lines' parameter. This means it will read the 
file without skipping lines with parsing errors.

null_values = data.isnull().any().any()

if null_values:

 print("There are null values in the dataset.")
 
else:

 print("There are no null values in the dataset.")
 
• These lines check for the presence of null (missing) values in the 
DataFrame. If any null values are found, it prints that there are null values in 
the dataset; otherwise, it prints that there are no null values.

data.info()

• This line displays information about the DataFrame, including the number of 
non-null entries in each column, data types, and memory usage.

print(data.dropna())

• This line drops rows with any null values from the DataFrame and prints the 
resulting DataFrame.

print(data.notnull().sum())

• This line counts and prints the number of non-null values in each column of 
the DataFrame.

data.isnull().sum()

• This line calculates and prints the number of null values in each column of 
the DataFrame.

data.dropna(subset=['Itemname', 'CustomerID'], inplace=True)

• This line drops rows with null values in the 'Itemname' and 'CustomerID' 
columns and modifies the 'data' DataFrame in place.

The code following this point is a separate section that performs association 
rule mining using the MLxtend library:

import pandas as pd

from mlxtend.frequent_patterns import apriori

from mlxtend.frequent_patterns import association_rules

• These lines import Pandas and the necessary functions from the MLxtend 
library for association rule mining.

data = pd.read_csv('assignment1_data.csv')

• This line reads the CSV file 'assignment1_data.csv' into a new DataFrame 
called 'data.'

basket = (data.groupby(['CustomerID', 'Itemname'])['Itemname']

.count().unstack().reset_index().fillna(0)

.set_index('CustomerID'))

• This code prepares the data for association rule mining by creating a 
"basket" of items purchased by each customer. It counts the occurrences of 
each item for each customer and constructs a binary matrix where 1 
indicates the item was purchased, and 0 indicates it wasn't.

def encode_units(x):

 if x <= 0:
 
 return 0
 
 if x >= 1:
 
 return 1
 
• This is a function definition that converts item counts to binary values (0 or 
1) based on whether the count is greater than 1 or not.
basket_sets = basket.applymap(encode_units)
• This line applies the encode_units function to the 'basket' data, converting 
item counts to binary values.

frequent_itemsets = apriori(basket_sets, min_support=0.05, 
use_colnames=True)

• This line uses the Apriori algorithm to find frequent itemsets in the binary 
'basket' data. The min_support parameter sets the minimum support 
threshold for an itemset to be considered frequent.

association_rules = association_rules(frequent_itemsets, metric="lift", 
min_threshold=1.0)

• This line generates association rules from the frequent itemsets using the 
"lift" metric with a minimum threshold of 1.0.

print("Association Rules:")

print(association_rules)

• These lines print the resulting association rules to the console.

The code following this point filters and displays meaningful association rules:

confidence_threshold = 0.7

lift_threshold = 1.0

meaningful_rules = association_rules[
(association_rules['confidence'] > confidence_threshold) &
(association_rules['lift'] > lift_threshold)
]

• These lines filter the association rules based on confidence and lift 
thresholds to identify meaningful rules.

print("Meaningful Association Rules:")

print(meaningful_rules)

• These lines print the meaningful association rules to the console.
The code following this point creates a histogram:

import pandas as pd

import plotly.express as px

import plotly.io as pio

import plotly.graph_objects as go

pio.templates.default = "plotly_white"

• These lines import Pandas and the Plotly libraries and set the Plotly template 
to 'plotly_white.'
data = pd.read_csv("assignment1_data.csv")
• This line reads the 'assignment1_data.csv' file into a new DataFrame called 
'data.'

print(data.head())

• This line prints the first few rows of the DataFrame to the console.
fig = px.histogram(data, x='Itemname', title='Item description')
• This line creates a histogram using Plotly Express with the 'Itemname' 
column as the x-axis and sets a title.

fig.show()

• This line displays the histogram
