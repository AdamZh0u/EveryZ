^^^^^^^^^^^^
Pandas
^^^^^^^^^^^^
Genertate DF
******************

Create Data Frame
====================
.. code-block:: python
    :linenos: 

    np.random.seed(1)
    """quick way to create a data frame for testing""" 
    df_test = pd.DataFrame(np.random.randn(3, 4), columns=['a', 'b', 'c', 'd']) \
        .assign(target=lambda x: (x['b']+x['a']/x['d'])*x['c'])

Data Frames For Testing
===========================
.. code-block:: python
    :linenos: 

    import pandas.util.testing
    df1 = pd.util.testing.makeDataFrame()
    df2 = pd.util.testing.makeMissingDataframe() # contains missing values
    df3 = pd.util.testing.makeTimeDataFrame() # contains datetime values
    df4 = pd.util.testing.makeMixedDataFrame()

Table Processing
***********************
Configure Pandas
=====================
.. code-block:: python
    :linenos: 

    ##https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html
    def pd_config():
        options={
            "display":{
                'max_colwidth': 7, ## 每一格行宽度
                "max_columns":30,
                'expand_frame_repr': False,  # wrap to multiple pages
                'max_rows': 30,
                'max_seq_items': 30,         # Max length of printed sequence
                'precision': 3,               # 小数精度
                'show_dimensions': True,    #显示行列
                "large_repr":"truncate",#"info" show as summary of df 
                "unicode.east_asian_width":False, # true to show east word in same length but in a longer time 
                "date_dayfirst":True # 20/01/2005 false:2005/01/20
            },
            "mode":{
                'chained_assignment': None,
                "use_inf_as_na":False #True means treat None, NaN, -INF, INF as NA (old way), False means None and NaN are null, but INF, -INF are not NA (new way).
            }
        }
        for category, option in options.items():
            for op, value in option.items():
                pd.set_option(f'{category}.{op}', value)  # Python 3.6+
    ## pd.reset_option("^display")## 复原

Data Frame Formatting
============================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["number"] = [3,10,1]
    df_out = (
    df.style.format({"a":"${:.2f}", "target":"${:.5f}"})
    .hide_index()
    .highlight_min("a", color ="red")
    .highlight_max("a", color ="green")
    .background_gradient(subset = "target", cmap ="Blues")
    .bar("number", color = "lightblue", align = "zero")
    .set_caption("DF with different stylings")
    ) 
    df_out

.. image:: ./00_img/df_formatting.jpg

Lower Case Columns
========================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df.columns = ["A","BGs","c","dag","Target"]#df.columns.to_list() 
    df.columns = map(str.lower, df.columns)

Fast Data Frame Split
=====================================
.. code-block:: python
    :linenos: 

    test =  df.sample(frac=0.4) ## sample
    train = df[~df.isin(test)].dropna(); train

Create Features and Labels List
====================================
.. code-block:: python
    :linenos: 

    ## 选择除A之外的列名
    X = [name for name in df.columns if name not in ["target", 'd']]

Gallery
========================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["category"] = np.where( df["target"]>1, "1",  "0") 
    df["k"] = df["category"].astype(str) +": " + df["d"].round(1).astype(str) 
    df = df.append(df, ignore_index=True)

    """set display width, col_width etc for interactive pandas session""" 
    pd.set_option('display.width', 200)
    pd.set_option('display.max_colwidth', 20)
    pd.set_option('display.max_rows', 100)
            
    """when you have an excel sheet with spaces in column names"""
    df.columns = [c.lower().replace(' ', '_') for c in df.columns]

    """Add prefix to all columns"""
    df.add_prefix("1_")

    """Add suffix to all columns"""
    df.add_suffix("_Z")

    """Droping column where missing values are above a threshold"""
    df.dropna(thresh = len(df)*0.95, axis = "columns") 

    """Given a dataframe df to filter by a series ["a","b"]:""" 
    df[df['category'].isin(["1","0"])]

    """filter by multiple conditions in a dataframe df"""
    df[(df['a'] >1) & (df['b'] <1)]

    """filter by conditions and the condition on row labels(index)"""
    df[(df.a > 0) & (df.index.isin([0, 1]))]

    """regexp filters on strings (vectorized), use .* instead of *"""
    df[df.category.str.contains(r'.*[0-9].*')]

    """logical NOT is like this"""
    df[~df.category.str.contains(r'.*[0-9].*')]

    """creating complex filters using functions on rows"""
    df[df.apply(lambda x: x['b'] > x['c'], axis=1)]

    """Pandas replace operation"""
    df["a"].round(2).replace(0.87, 17, inplace=True)
    df["a"][df["a"] < 4] = 19

    """Conditionals and selectors"""
    df.loc[df["a"] > 1, ["a","b","target"]]

    """Selecting multiple column slices"""
    df.iloc[:, np.r_[0:2, 4:5]] 

    """apply and map examples"""
    df[["a","b","c"]].applymap(lambda x: x+1)

    """add 2 to row 3 and return the series"""
    df[["a","b","c"]].apply(lambda x: x[0]+2,axis=0)

    """add 3 to col A and return the series"""
    df.apply(lambda x: x['a']+1,axis=1)

    """ Split delimited values in a DataFrame column into two new columns """
    df['new1'], df['new2'] = zip(*df['k'].apply(lambda x: x.split(': ', 1)))

    """ Doing calculations with DataFrame columns that have missing values
    In example below, swap in 0 for df['col1'] cells that contain null """ 
    df['new3'] = np.where(pd.isnull(df['b']),0,df['a']) + df['c']

    """ Exclude certain data type or include certain data types """
    df.select_dtypes(exclude=['O','float'])
    df.select_dtypes(include=['int'])

    """one liner to normalize a data frame""" 
    (df[["a","b"]] - df[["a","b"]].mean()) / (df[["a","b"]].max() - df[["a","b"]].min())

    """groupby used like a histogram to obtain counts on sub-ranges of a variable, pretty handy""" 
    df.groupby(pd.cut(df.a, range(0, 1, 2))).size()

    """use a local variable use inside a query of pandas using @"""
    mean = df["a"].mean()
    df.query("a > @mean")

    """Calculate the % of missing values in each column"""
    df.isna().mean() 

    """Calculate the % of missing values in each row"""
    rows = df.isna().mean(axis=1) ; df.head()

Read Commands
===================
.. code-block:: python
    :linenos: 

    """To avoid Unnamed: 0 when loading a previously saved csv with index"""
    """To parse dates"""
    """To set data types"""

    df_out = pd.read_csv("data.csv", index_col=0,
                    parse_dates=['D'],
                    dtype={"c":"category", "B":"int64"}).set_index("D")

    """Copy data to clipboard; like an excel copy and paste
    df = pd.read_clipboard()
    """

    """Read table from website
    df = pd.read_html(url, match="table_name")
    """

    """ Read pdf into dataframe ()
    !pip install tabula
    from tabula import read_pdf
    df = read_pdf('test.pdf', pages='all')
    """
    df_out.head()

Create Ordered Categories
================================
.. code-block:: python
    :linenos: 

    df["cats"] = ["bad","good","excellent"]
    from pandas.api.types import CategoricalDtype

    ## Let's create our own categorical order.
    cat_type = CategoricalDtype(["bad", "good", "excellent"], ordered = True)
    df["cats"] = df["cats"].astype(cat_type)

    ## Now we can use logical sorting.
    df = df.sort_values("cats", ascending = True)

    ## We can also filter this as if they are numbers.
    df[df["cats"] > "bad"]

Select Columns Based on Regex
=====================================
.. code-block:: python
    :linenos: 

    # https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.filter.html
    df_out = df.filter(regex="_l",axis=1) 
    # items : Keep labels from axis which are in items.
    # like ：Keep labels from axis for which “like in label == True”.
    # regex : 
    # axis : 0 rows 1 columns

Accessing Group of Groupby Object
=====================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df = df.append(df, ignore_index=True)
    df["groupie"] = ["falcon","hawk","hawk","eagle","falcon","hawk"]
    gbdf = df.groupby("groupie")
    hawk = gbdf.get_group("hawk").mean();

Multiple External Selection Criteria
========================================
.. code-block:: python
    :linenos: 

    cr1 = df["a"] > 0
    cr2 = df["b"] < 0
    cr3 = df["c"] > 0
    cr4 = df["d"] >-1
    df[cr1 & cr2 & cr3 & cr4]

Memory Reduction Script (func)
===================================
.. code-block:: python
    :linenos: 

    import gc
    def reduce_mem_usage(df):
        """ iterate through all the columns of a dataframe and modify the data type
            to reduce memory usage.        
        """
        start_mem = df.memory_usage().sum() / 1024**2
        print('Memory usage of dataframe is {:.2f} MB'.format(start_mem))
        
        for col in df.columns:
            col_type = df[col].dtype
            gc.collect()
            if col_type != object:
                c_min = df[col].min()
                c_max = df[col].max()
                if str(col_type)[:3] == 'int':
                    if c_min > np.iinfo(np.int8).min and c_max < np.iinfo(np.int8).max:
                        df[col] = df[col].astype(np.int8)
                    elif c_min > np.iinfo(np.int16).min and c_max < np.iinfo(np.int16).max:
                        df[col] = df[col].astype(np.int16)
                    elif c_min > np.iinfo(np.int32).min and c_max < np.iinfo(np.int32).max:
                        df[col] = df[col].astype(np.int32)
                    elif c_min > np.iinfo(np.int64).min and c_max < np.iinfo(np.int64).max:
                        df[col] = df[col].astype(np.int64)  
                else:
                    if c_min > np.finfo(np.float16).min and c_max < np.finfo(np.float16).max:
                        df[col] = df[col].astype(np.float16)
                    elif c_min > np.finfo(np.float32).min and c_max < np.finfo(np.float32).max:
                        df[col] = df[col].astype(np.float32)
                    else:
                        df[col] = df[col].astype(np.float64)
            else:
                df[col] = df[col].astype('category')

        end_mem = df.memory_usage().sum() / 1024**2
        print('Memory usage after optimization is: {:.2f} MB'.format(end_mem))
        print('Decreased by {:.1f}%'.format(100 * (start_mem - end_mem) / start_mem))
        
        return df
    df_out = pv.reduce_mem_usage(df); df_out


Verify Primary Key (func)
==============================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["first_d"] = [0,1,2]
    df["second_d"] = [4,1,9]
    def verify_primary_key(df, column_list):
        return df.shape[0] == df.groupby(column_list).size().reset_index().shape[0]

    verify_primary_key(df, ["first_d","second_d"])

Shift Columns to Front (func)
================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def list_shuff(items, df):
        "Bring a list of columns to the front"
        cols = list(df)
        for i in range(len(items)):
            cols.insert(i, cols.pop(cols.index(items[i])))
        df = df.loc[:, cols]
        df.reset_index(drop=True, inplace=True)
        return df

    df_out = pv.list_shuff(["target","c","d"],df); df_out

Multiple Column Assignments
================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df_out = (df.assign(stringed = df["a"].astype(str),
            ounces = df["b"]*12,#    this will allow yo set a title
            galons = lambda df: df["a"]/128)
           .query("b > -1")
           .style.set_caption("Average consumption")) 

Method Chaining Technique
================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df[df>df.mean()]  = None

    # with line continuation character
    df_out = df.dropna(subset=["b","c"],how="all") \
    .loc[df["a"]>0] \
    .round(2) \
    .groupby(["target","b"]).max() \
    .unstack() \
    .fillna(0) \
    .rolling(1).sum() \
    .reset_index() \
    .stack() \
    .ffill().bfill() 
    df_out

Load Multiple Files
=======================
.. code-block:: python
    :linenos: 

    import os
    os.makedirs("folder",exist_ok=True,); df_test.to_csv("folder/first.csv",index=False) ; df_test.to_csv("folder/last.csv",index=False)

    import glob
    files = glob.glob('folder/*.csv')
    dfs = [pd.read_csv(fp) for fp in files]
    df_out = pd.concat(dfs)

Drop Rows with Column Substring
=====================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["string_feature"] = ["1xZoo", "Safe7x", "bat4"]
    substring = ["xZ","7z", "tab4"]
    df_out = df[~df.string_feature.str.contains('|'.join(substring))]
    df_out

Unnest (Explode) a Column
=====================================
.. code-block:: python
    :linenos: 

    df = df_test.head()
    df["g"] = [[str(a)+lista for a in range(4)] for lista in ["a","b","c"]]
    df_out = df.explode("g"); df_out.iloc[:5,:]

Nest List Back into Column
=====================================
.. code-block:: python
    :linenos: 

    ### Run above example first 
    df = df_out.copy()
    df_out['g'] = df_out.groupby(df_out.index)['g'].agg(list)
    df_out.head()

Split Cells With Lists
=====================================
.. code-block:: python
    :linenos: 



Table Exploration
**********************
Groupby Functionality
=====================================
.. code-block:: python
    :linenos: 






Feature Processing
*************************

Feature Engineering
***********************
Model Validation
*************************