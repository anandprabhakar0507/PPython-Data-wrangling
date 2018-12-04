
.. raw:: html

   <div class="alert alert-block alert-info" style="margin-top: 20px">

.. raw:: html

   <h1 align="center">

 Link

.. raw:: html

   </h1>

.. raw:: html

   <h1 align="center">

Data Analysis with Python

.. raw:: html

   </h1>

Module 2 : Data Wrangling
=========================

Welcome!
~~~~~~~~

By the end of this notebook, you will have learned the basics of Data
Wrangling!

Table of contents
-----------------

.. raw:: html

   <div class="alert alert-block alert-info" style="margin-top: 20px">

.. raw:: html

   <li>

Identify and handle missing values

.. raw:: html

   <ul>

.. raw:: html

   <div>

- Identify missing values

.. raw:: html

   </div>

.. raw:: html

   <div>

- Deal with missing values

.. raw:: html

   </div>

.. raw:: html

   <div>

- Correct data format

.. raw:: html

   </div>

.. raw:: html

   </ul>

.. raw:: html

   </li>

.. raw:: html

   <p>

.. raw:: html

   </p>

.. raw:: html

   <li>

Data standardization

.. raw:: html

   </li>

.. raw:: html

   <li>

Data Normalization (centring/scaling)

.. raw:: html

   </li>

.. raw:: html

   <li>

Binning

.. raw:: html

   </li>

.. raw:: html

   <li>

Indicator variable

.. raw:: html

   </li>

.. raw:: html

   <p>

.. raw:: html

   </p>

Estimated Time Needed: 30 min

.. raw:: html

   </div>

.. raw:: html

   <hr>

What is the purpose of Data Wrangling?
--------------------------------------

Data Wrangling is the process of converting data from the initial format
to a format that may be better for analysis.

What is the fuel consumption (L/100k) rate for the diesel car?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Import data
~~~~~~~~~~~

You can find the "Automobile Data Set" from the following link:
https://archive.ics.uci.edu/ml/machine-learning-databases/autos/imports-85.data.
We will be using this data set throughout this course.

Import pandas
^^^^^^^^^^^^^

.. code:: python

    import pandas as pd

Reading the data set from the URL and adding the related headers.
-----------------------------------------------------------------

URL of dataset
^^^^^^^^^^^^^^

.. code:: python

    filename = 'https://archive.ics.uci.edu/ml/machine-learning-databases/autos/imports-85.data'

Python list "headers" containing name of headers

.. code:: python

    headers = ["symboling","normalized-losses","make","fuel-type","aspiration", "num-of-doors","body-style",
             "drive-wheels","engine-location","wheel-base", "length","width","height","curb-weight","engine-type",
             "num-of-cylinders", "engine-size","fuel-system","bore","stroke","compression-ratio","horsepower",
             "peak-rpm","city-mpg","highway-mpg","price"]

Use the Pandas method **read\_csv()** to load the data from the web
address. Set the parameter "names" equal to the Python list "headers".

.. code:: python

    df = pd.read_csv(filename, names = headers)
    print("Done")

Use the method **head()** to display the first five rows of the
dataframe.

.. code:: python

    # To see what the data set looks like, we'll use the head() method.
    df.head()

As we can see, several question marks appeared in the dataframe; those
are missing values which may hinder our further analysis.

.. raw:: html

   <div>

So, how do we identify all those missing values and deal with them?

.. raw:: html

   </div>

**How to work with missing data:**

Steps for working with missing data: 1. Identify missing data 2. Deal
with missing data 3. Correct data format

 # 1. Identify and handle missing values

 ### Convert "?" to NaN In the car dataset, missing data comes with the
question mark "?". We replace "?" with NaN (Not a Number), which is
Python's default missing value marker, for reasons of computational
speed and convenience. Here we use the function:

.. raw:: html

   <pre>.replace(A, B, inplace = True) </pre>

to replace A by B

.. code:: python

    import numpy as np
    
    # replace "?" to NaN
    df.replace("?", np.nan, inplace = True)
    df.head(5)

Evaluating for Missing Data
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The missing values are converted to Python's default. We use Python's
built-in functions to identify these missing values. There are two
methods to detect missing data: 1. **.isnull()** 2. **.notnull()**

The output is a boolean value indicating whether the dataframe is
missing data.

.. code:: python

    missing_data = df.isnull()
    missing_data.head(5)

"True" stands for missing value, while "False" stands for not missing
value.

Count missing values in each column
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using a for loop in Python, we can quickly figure out the number of
missing values in each column. As mentioned above, "True" represents a
missing value, "False" means the value is present in the dataset. In the
body of the for loop the method ".value\_couts()" counts the number of
"True" values.

.. code:: python

    for column in missing_data.columns.values.tolist():
        print(column)
        print (missing_data[column].value_counts())
        print("")    

Based on the summary above, each column has 205 rows of data, with seven
columns containing missing data:

1. "normalized-losses": 41 missing data
2. "num-of-doors": 2 missing data
3. "bore": 4 missing data
4. "stroke" : 4 missing data
5. "horsepower": 2 missing data
6. "peak-rpm": 2 missing data
7. "price": 4 missing data

 ## Deal with missing data **How to deal with missing data:**

::

    1. Drop data 
        a. drop the whole row
        b. drop the whole column
    2. Replace data
        a. replace it by mean
        b. replace it by frequency
        c. replace it based on other functions

Whole columns should be dropped only if most entries in the column are
empty.

In our dataset, none of the columns are empty enough to drop entirely.

We have some freedom in choosing which method to replace data; however,
some methods may seem more reasonable than others. We will apply each
method to many different columns:

**Replace by mean:**

::

    "normalized-losses": 41 missing data, replace them with mean
    "stroke": 4 missing data, replace them with mean
    "bore": 4 missing data, replace them with mean
    "horsepower": 2 missing data, replace them with mean
    "peak-rpm": 2 missing data, replace them with mean

**Replace by frequency:**

::

    "num-of-doors": 2 missing data, replace them with "four". 
        * Reason: 84% sedans is four doors. Since four doors is most frequent, it is most likely to 

**Drop the whole row:**

::

    "price": 4 missing data, simply delete the whole row
        * Reason: price is what we want to predict. Any data entry without price data cannot be used for prediction; therefore they are not useful to us

Calculate the average of the column:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    avg_1 = df["normalized-losses"].astype("float").mean(axis = 0)

Replace "NaN" by mean value in "normalized-losses" column:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    df["normalized-losses"].replace(np.nan, avg_1, inplace = True)

Calculate the mean value for 'bore' column:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    avg_2=df['bore'].astype('float').mean(axis=0)

Replace NaN by mean value:
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    df['bore'].replace(np.nan, avg_2, inplace= True)

.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #1:

.. raw:: html

   </h1>

According to the example above, replace NaN in "stroke" column by mean:

.. raw:: html

   </div>


.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #1 Answer:

.. raw:: html

   </h1>

Run the code below! Did you get the right code?

.. raw:: html

   </div>

.. raw:: html

   <div align="right">

Click here for the solution

.. raw:: html

   </div>

.. raw:: html

   <div id="q1" class="collapse">

::

    # calculate the mean vaule for "stroke" column
    avg_3 = df["stroke"].astype("float").mean(axis = 0)

    # replace NaN by mean value in "stroke" column
    df["stroke"].replace(np.nan, avg_3, inplace = True)

.. raw:: html

   </div>

Calculate the mean value for the 'horsepower' column:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    avg_4=df['horsepower'].astype('float').mean(axis=0)

Replace "NaN" by mean value :
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    df['horsepower'].replace(np.nan, avg_4, inplace= True)

Calculate the mean value for 'peak-rpm' column:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    avg_5=df['peak-rpm'].astype('float').mean(axis=0)

Replace NaN by mean value:
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    df['peak-rpm'].replace(np.nan, avg_5, inplace= True)

To see which values are present in a particular column, we can use the
".value\_counts()" method:

.. code:: python

    df['num-of-doors'].value_counts()

We can see that four doors are the most common type. We can also use the
".idxmax()" method to automatically calculate the most common type:

.. code:: python

    df['num-of-doors'].value_counts().idxmax()

The replacement procedure is very similar to what we have seen
previously:

.. code:: python

    #replace the missing 'num-of-doors' values by the most frequent 
    df["num-of-doors"].replace(np.nan, "four", inplace = True)

Finally, let's drop all rows that do not have price data:

.. code:: python

    # simply drop whole row with NaN in "price" column
    df.dropna(subset=["price"], axis=0, inplace = True)
    
    # reset index, because we droped two rows
    df.reset_index(drop = True, inplace = True)

.. code:: python

    df.head()

**Good!** Now, we obtain the dataset with no missing values.

 ## Correct data format **We are almost there!**

.. raw:: html

   <div>

The last step in data cleaning is checking and making sure that all data
is in the correct format (int, float, text or other).

.. raw:: html

   </div>

In Pandas, we use

.. raw:: html

   <div>

**.dtype()** to check the data type

.. raw:: html

   </div>

.. raw:: html

   <div>

**.astype()** to change the data type

.. raw:: html

   </div>

Let's list the data types for each column:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    df.dtypes

As we can see above, some columns are not of the correct data type.
Numerical variables should have type 'float' or 'int', and variables
with strings such as categories should have type 'object'. For example,
'bore' and 'stroke' variables are numerical values that describe the
engines, so we should expect them to be of the type 'float' or 'int',
however, they are shown as type 'object'. We have to convert data types
into a proper format for each column using the "astype()" method.

Convert data types to proper format:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    
    df[["bore", "stroke"]] = df[["bore", "stroke"]].astype("float")
    df[["normalized-losses"]] = df[["normalized-losses"]].astype("int")
    df[["price"]] = df[["price"]].astype("float")
    df[["peak-rpm"]] = df[["peak-rpm"]].astype("float")
    print("Done")

Let us list the columns after the conversion:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    df.dtypes

**Wonderful!**

Now, we finally obtain the cleaned dataset with no missing values and
all data in its proper format.

 # Data Standardization Data is usually collected from different
agencies with different formats. (Data Standardization is also a term
for a particular type of data normalization, where we subtract the mean
and divide by the standard deviation.

**What is Standardization?**

.. raw:: html

   <div>

Standardization is the process of transforming data into a common format
which allows the researcher to make the meaningful comparison.

.. raw:: html

   </div>

**Example**

.. raw:: html

   <div>

Transform mpg to L/100km:

.. raw:: html

   </div>

.. raw:: html

   <div>

In our dataset, the fuel consumption columns "city-mpg" and
"highway-mpg" are represented by mpg (miles per gallon) unit. Assume we
are developing an application in a country that accepts the fuel
consumption with L/100km standard

.. raw:: html

   </div>

.. raw:: html

   <div>

We will need to apply **data transformation** to transform mpg into
L/100km

.. raw:: html

   </div>

The formula for unit conversion is L/100km = 235 / mpg

.. raw:: html

   <div>

We can do many mathematical operations directly in Pandas.

.. raw:: html

   </div>

.. code:: python

    df.head()

.. code:: python

    # transform mpg to L/100km by mathematical operation (235 divided by mpg)
    df['city-L/100km'] = 235/df["city-mpg"]
    
    # check your transformed data 
    df.head()

.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #2:

.. raw:: html

   </h1>

According to the example above, transform mpg to L/100km in the column
of "highway-mpg", and change the name of column to "highway-L/100km":

.. raw:: html

   </div>


.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #2 Answer:

.. raw:: html

   </h1>

Run the code below! Did you get the right code?

.. raw:: html

   </div>

.. raw:: html

   <div align="right">

Click here for the solution

.. raw:: html

   </div>

.. raw:: html

   <div id="q2" class="collapse">

::

    # transform mpg to L/100km by mathematical operation (235 divided by mpg)
    df["highway-mpg"] = 235/df["highway-mpg"]

    # rename column name from "highway-mpg" to "highway-L/100km"
    df.rename(columns={'"highway-mpg"':'highway-L/100km'}, inplace=True)

    # check your transformed data 
    df.head()

.. raw:: html

   </div>

 # Data Normalization

**Why normalization?**

.. raw:: html

   <div>

Normalization is the process of transforming values of several variables
into a similar range. Typical normalizations include scaling the
variable so the variable average is 0, scaling the variable so the
variable variance is 1, or scaling the variable so the variable values
range from 0 to 1.

.. raw:: html

   </div>

**Example**

.. raw:: html

   <div>

To demonstrate normalization, let's say we want to scale the columns
"length", "width" and "height"

.. raw:: html

   </div>

.. raw:: html

   <div>

**Target:** We would like to Normalize those variables so their value
ranges from 0 to 1.

.. raw:: html

   </div>

.. raw:: html

   <div>

**Approach:** Replace origianl value by (original value)/(maximum value)

.. raw:: html

   </div>

.. code:: python

    # replace (origianl value) by (original value)/(maximum value)
    df['length'] = df['length']/df['length'].max()
    df['width'] = df['width']/df['width'].max()

.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Questiont #3:

.. raw:: html

   </h1>

According to the example above, normalize the column "height":

.. raw:: html

   </div>


.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #3 Answer:

.. raw:: html

   </h1>

Run the code below! Did you get the right code?

.. raw:: html

   </div>

.. raw:: html

   <div align="right">

Click here for the solution

.. raw:: html

   </div>

.. raw:: html

   <div id="q3" class="collapse">

::

    df['height'] = df['height']/df['height'].max() 
    # show the scaled columns
    df[["length","width","height"]].head()

.. raw:: html

   </div>

Here we can see we've normalized "length", "width", and "height" in the
range of [0,1].

 # Binning **Why binning?**

.. raw:: html

   <div>

Binning is a process of transforming continuous numerical variables into
discrete categorical 'bins' for grouped analysis.

.. raw:: html

   </div>

**Example: **

.. raw:: html

   <div>

In our dataset, "horsepower" is a real valued variable ranging from 48
to 288, and it has 57 unique values. What if we only care about the
price difference between cars with high horsepower, medium horsepower,
and little horsepower (3 types)? Can we rearrange them into three ‘bins'
to simplify analysis?

.. raw:: html

   </div>

.. raw:: html

   <div>

We will use the Pandas method 'cut' to segment the 'horsepower' column
into 3 bins.

.. raw:: html

   <div>

Example of Binning Data In Pandas
---------------------------------

Convert data to correct format:

.. code:: python

    df["horsepower"]=df["horsepower"].astype(float, copy=True)

We would like four bins of equal size bandwidth. The fourth is because
the function "cut" includes the rightmost value:

.. code:: python

    binwidth = (max(df["horsepower"])-min(df["horsepower"]))/4

We build a bin array with a minimum value to a maximum value, with
bandwidth calculated above. The bins will be values used to determine
when one bin ends and another begins:

.. code:: python

    bins = np.arange(min(df["horsepower"]), max(df["horsepower"]), binwidth)
    bins

We set group names:

.. code:: python

    group_names = ['Low', 'Medium', 'High']

We apply the function "cut" to determine what each value of
"df['horsepower']" belongs to.

.. code:: python

    df['horsepower-binned'] = pd.cut(df['horsepower'], bins, labels=group_names,include_lowest=True )
    df[['horsepower','horsepower-binned']].head(20)

Check the dataframe above carefully, you will find the last column
provides the bins for "horsepower" with 3 categories ("Low","Medium" and
"High").

.. raw:: html

   <div>

We successfully narrow the intervals from 57 to 3!

.. raw:: html

   </div>

Bins visualization
------------------

Normally, a histogram is used to visualize the distribution of bins we
created above.

.. code:: python

    %matplotlib inline
    import matplotlib as plt
    from matplotlib import pyplot
    
    a = (0,1,2)
    
    # draw historgram of attribute "horsepower" with bins = 3
    plt.pyplot.hist(df["horsepower"], bins = 3)
    
    # set x/y labels and plot title
    plt.pyplot.xlabel("horsepower")
    plt.pyplot.ylabel("count")
    plt.pyplot.title("horsepower bins")

The plot above shows the binning result for attribute "horsepower".

 # Indicator variable (or dummy variable) **What is an indicator
variable?**

.. raw:: html

   <div>

An indicator variable (or dummy variable) is a numerical variable used
to label categories. They are called 'dummies' because the numbers
themselves don't have inherent meaning.

.. raw:: html

   </div>

**Why do we use indicator variables?**

.. raw:: html

   <div>

So that we can use categorical variables for regression analysis in the
later modules.

.. raw:: html

   </div>

**Example**

.. raw:: html

   <div>

We see the column "fuel-type" has two unique values, "gas" or "diesel".
Regression doesn't understand words, only numbers. To use this attribute
in regression analysis, we convert "fuel-type" into indicator variables.

.. raw:: html

   </div>

.. raw:: html

   <div>

We will use the panda's method 'get\_dummies' to assign numerical values
to different categories of fuel type.

.. raw:: html

   <div>

.. code:: python

    df.columns

Get indicator variables and assign it to data frame
"dummy\_variable\_1":

.. code:: python

    dummy_variable_1 = pd.get_dummies(df["fuel-type"])
    dummy_variable_1.head()

Change column names for clarity:

.. code:: python

    dummy_variable_1.rename(columns={'fuel-type-diesel':'gas', 'fuel-type-diesel':'diesel'}, inplace=True)
    dummy_variable_1.head()

We now have the value 0 to represent "gas" and 1 to represent "diesel"
in the column "fuel-type". We will now insert this column back into our
original dataset.

.. code:: python

    # merge data frame "df" and "dummy_variable_1" 
    df = pd.concat([df, dummy_variable_1], axis=1)
    
    # drop original column "fuel-type" from "df"
    df.drop("fuel-type", axis = 1, inplace=True)

.. code:: python

    df.head()

The last two columns are now the indicator variable representation of
the fuel-type variable. It's all 0s and 1s now.

.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #4:

.. raw:: html

   </h1>

As above, create indicator variable to the column of "aspiration": "std"
to 0, while "turbo" to 1:

.. raw:: html

   </div>


.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #4 Answer:

.. raw:: html

   </h1>

Run the code below! Did you get the right code?

.. raw:: html

   </div>

.. raw:: html

   <div align="right">

Click here for the solution

.. raw:: html

   </div>

.. raw:: html

   <div id="q4" class="collapse">

::

    # get indicator variables of aspiration and assign it to data frame "dummy_variable_2"
    dummy_variable_2 = pd.get_dummies(df['aspiration'])

    # change column names for clarity
    dummy_variable_2.rename(columns={'std':'aspiration-std', 'turbo': 'aspiration-turbo'}, inplace=True)

    # show first 5 instances of data frame "dummy_variable_1"
    dummy_variable_2.head()

.. raw:: html

   </div>

.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #5:

.. raw:: html

   </h1>

Merge the new dataframe to the original dataframe then drop the column
'aspiration':

.. raw:: html

   </div>


.. raw:: html

   <div class="alert alert-danger alertdanger" style="margin-top: 20px">

.. raw:: html

   <h1>

Question #5 Answer:

.. raw:: html

   </h1>

Run the code below! Did you get the right code?

.. raw:: html

   </div>

.. raw:: html

   <div align="right">

Click here for the solution

.. raw:: html

   </div>

.. raw:: html

   <div id="q5" class="collapse">

::

    #merge the new dataframe to the original datafram
    df = pd.concat([df, dummy_variable_2], axis=1)

    # drop original column "aspiration" from "df"
    df.drop('aspiration', axis = 1, inplace=True)

.. raw:: html

   </div>

save the new csv

.. code:: python

    df.to_csv('clean_df.csv')

About the Authors:
==================

This notebook written by `Mahdi Noorian
PhD <https://www.linkedin.com/in/mahdi-noorian-58219234/>`__ ,\ `Joseph
Santarcangelo PhD <https://www.linkedin.com/in/joseph-s-50398b136/>`__,
Bahare Talayian, Eric Xiao, Steven Dong, Parizad , Hima Vsudevan and
`Fiorella Wenver <https://www.linkedin.com/in/fiorellawever/>`__.
Copyright © 2017
`cognitiveclass.ai <cognitiveclass.ai?utm_source=bducopyrightlink&utm_medium=dswb&utm_campaign=bdu>`__.
This notebook and its source code are released under the terms of the
`MIT License <https://bigdatauniversity.com/mit-license/>`__.

.. raw:: html

   <div class="alert alert-block alert-info" style="margin-top: 20px">

.. raw:: html

   <h1 align="center">

 Link

.. raw:: html

   </h1>
