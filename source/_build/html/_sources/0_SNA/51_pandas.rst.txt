^^^^^^^^^^^^
Pandas
^^^^^^^^^^^^
https://github.com/firmai/pandasvault#table-processing

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
    df_out = reduce_mem_usage(df); df_out


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

    df_out = list_shuff(["target","c","d"],df); df_out

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

    df = df_test.head()
    df["g"] = [",".join([str(a)+lista for a in range(4)]) for lista in ["a","b","c"]]
    df_out = df.assign(g = df["g"].str.split(",")).explode("g")

Table Exploration
**********************
Groupby Functionality
=====================================
.. code-block:: python
    :linenos: 

    df["gr"] = [1, 1 , 0]
    df_out = df.groupby('gr').agg([np.sum, np.mean, np.std])
    df_out.iloc[:,:]

Cross Correlation Series Without Duplicates (func)
======================================================
.. code-block:: python
    :linenos: 

    def corr_list(df):

        return  (df.corr()
                .unstack()
                .sort_values(kind="quicksort",ascending=False)
                .drop_duplicates().iloc[1:]); df_out
                
    corr_list(df)

Missing Data Report
======================================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df[df>df.mean()]  = None

    def missing_data(data):
        "Create a dataframe with a percentage and count of missing values"
        total = data.isnull().sum().sort_values(ascending = False)
        percent = (data.isnull().sum()/data.isnull().count()*100).sort_values(ascending = False)
        return pd.concat([total, percent], axis=1, keys=['Total', 'Percent'])

    df_out = missing_data(df)

Duplicated Rows Report
======================================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["a"].iloc[2] = df["a"].iloc[1]
    df["b"].iloc[2] = df["b"].iloc[1] 
    # Get a report of all duplicate records in a dataframe, based on specific columns
    df_out = df[df.duplicated(['a'], keep=False)]

Skewness (func)
======================================================
.. code-block:: python
    :linenos: 

    from scipy.stats import skew

    def display_skewness(data):
        '''show skewness information

            Parameters
            ----------
            data: pandas dataframe

            Return
            ------
            df: pandas dataframe
        '''
        numeric_cols = data.columns[data.dtypes != 'object'].tolist()
        skew_value = []

        for i in numeric_cols:
            skew_value += [skew(data[i])]
        df = pd.concat(
            [pd.Series(numeric_cols), pd.Series(data.dtypes[data.dtypes != 'object'].apply(lambda x: str(x)).values)
                , pd.Series(skew_value)], axis=1)
        df.columns = ['var_name', 'col_type', 'skew_value']

        return df

    display_skewness(df)



Feature Processing
*************************
Remove Correlated Pairs (func)
===================================

.. code-block:: python
    :linenos: 

    df= df_test.copy(); df
    def drop_corr(df, thresh=0.99,keep_cols=[]):
        df_corr = df.corr().abs()
        upper = df_corr.where(np.triu(np.ones(df_corr.shape), k=1).astype(np.bool))
        to_remove = [column for column in upper.columns if any(upper[column] > thresh)] ## Change to 99% for selection
        to_remove = [x for x in to_remove if x not in keep_cols]
        df_corr = df_corr.drop(columns = to_remove)
        return df.drop(to_remove,axis=1)

    df_out = drop_corr(df, thresh=0.1,keep_cols=["target"]); df_out

Replace Infrequently Occuring Categories
=============================================
替换频率比较小的类别

.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df = df.append([df]*2)
    df["cat"] = ["bat","bat","rat","mat","mat","mat","mat","mat","mat"]; df
    def replace_small_cat(df, columns, thresh=0.2, term="other"):
        for col in columns:
            # Step 1: count the frequencies
            frequencies = df[col].value_counts(normalize = True)
            # Step 2: establish your threshold and filter the smaller categories
            small_categories = frequencies[frequencies < thresh].index
            df[col] = df[col].replace(small_categories, "Other")
        return df
    df_out = replace_small_cat(df,["cat"])

Quasi-Constant Features Detection (func)
===============================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["a"] = 3 
    def constant_feature_detect(data,threshold=0.98):
        data_copy = data.copy(deep=True)    #if False Any changes to the data of the original will be reflected in the shallow copy
        quasi_constant_feature = []
        for feature in data_copy.columns:
            predominant = (data_copy[feature].value_counts() / np.float(
                        len(data_copy))).sort_values(ascending=False).values[0]
            if predominant >= threshold:
                quasi_constant_feature.append(feature)   
        return quasi_constant_feature

    # the original dataset has no constant variable
    qconstant_col = constant_feature_detect(data=df,threshold=0.9)
    df_out = df.drop(qconstant_col, axis=1) ; df_out

Filling Missing Values Separately
===================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df[df>df.mean()]  = None 
    # Clean up missing values in multiple DataFrame columns
    dict_fill = {'a': 4,
                'b': 3,
                'c': 5,
                'd': 9999,
                'target': "False"}
    df = df.fillna(dict_fill) ;df

Conditioned Column Value Replacement
===================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    # Set DataFrame column values based on other column values
    df.loc[(df['a'] >1 ) & (df['c'] <0), ['target']] = np.nan

Remove Non-numeric Values in Data Frame
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy().assign(target=lambda row: row["a"].round(4).astype(str)+"SC"+row["b"].round(4).astype(str))
    df["a"] = "TI4560L" + df["a"].round(4).astype(str)
    df_out = df.replace('[^0-9]+', '', regex=True)

Feature Scaling, Normalisation, Standardisation (func)
===========================================================
.. code-block:: python
    :linenos: 

    from sklearn.preprocessing import StandardScaler
    from sklearn.preprocessing import MinMaxScaler

    def scaler(df,scaler=None,train=True, target=None, cols_ignore=None, type="Standard"):

        if cols_ignore:
            hold = df[cols_ignore].copy()
            df = df.drop(cols_ignore,axis=1)
        if target:
            x = df.drop([target],axis=1).values #returns a numpy array
        else:
            x = df.values
        if train:
            if type=="Standard":
            scal = StandardScaler()
            elif type=="MinMax":
            scal = MinMaxScaler()
            scal.fit(x)
            x_scaled = scal.transform(x)
        else:
            x_scaled = scaler.transform(x)
        
        if target:
            df_out = pd.DataFrame(x_scaled, index=df.index, columns=df.drop([target],axis=1).columns)
            df_out[target]= df[target]
        else:
            df_out = pd.DataFrame(x_scaled, index=df.index, columns=df.columns)
        
        df_out = pd.concat((hold,df_out),axis=1)
        if train:
            return df_out, scal
        else:
            return df_out

    df_out_train, scl = scaler(df,target="target",cols_ignore=["a"],type="MinMax")
    df_out_test = scaler(df_test,scaler=scl,train=False, target="target",cols_ignore=["a"])

Impute Null with Tail Distribution (func)
===========================================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df[df>df.mean()]  = None
    def impute_null_with_tail(df,cols=[]):
        """
        replacing the NA by values that are at the far end of the distribution of that variable
        calculated by mean + 3*std
        """
        
        df = df.copy(deep=True)
        for i in cols:
            if df[i].isnull().sum()>0:
                df[i] = df[i].fillna(df[i].mean()+3*df[i].std())
            else:
                warn("Column %s has no missing" % i)
        return df    
    
    df_out = impute_null_with_tail(df,cols=df.columns); df_out

Detect Outliers (func)
==============================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def outlier_detect(data,col,threshold=3,method="IQR"):
    
        if method == "IQR":
            IQR = data[col].quantile(0.75) - data[col].quantile(0.25)
            Lower_fence = data[col].quantile(0.25) - (IQR * threshold)
            Upper_fence = data[col].quantile(0.75) + (IQR * threshold)
        if method == "STD":
            Upper_fence = data[col].mean() + threshold * data[col].std()
            Lower_fence = data[col].mean() - threshold * data[col].std()   
        if method == "OWN":
            Upper_fence = data[col].mean() + threshold * data[col].std()
            Lower_fence = data[col].mean() - threshold * data[col].std() 
        if method =="MAD":
            median = data[col].median()
            median_absolute_deviation = np.median([np.abs(y - median) for y in data[col]])
            modified_z_scores = pd.Series([0.6745 * (y - median) / median_absolute_deviation for y in data[col]])
            outlier_index = np.abs(modified_z_scores) > threshold
            print('Num of outlier detected:',outlier_index.value_counts()[1])
            print('Proportion of outlier detected',outlier_index.value_counts()[1]/len(outlier_index))
            return outlier_index, (median_absolute_deviation, median_absolute_deviation)


        para = (Upper_fence, Lower_fence)
        tmp = pd.concat([data[col]>Upper_fence,data[col]<Lower_fence],axis=1)
        outlier_index = tmp.any(axis=1)
        print('Num of outlier detected:',outlier_index.value_counts()[1])
        print('Proportion of outlier detected',outlier_index.value_counts()[1]/len(outlier_index))
        
        return outlier_index, para

    index,para = outlier_detect(df,"a",threshold=0.5,method="IQR")
    print('Upper bound:',para[0],'\nLower bound:',para[1])

Windsorize Outliers (func)
==============================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def windsorization(data,col,para,strategy='both'):
        """
        top-coding & bottom coding (capping the maximum of a distribution at an arbitrarily set value,vice versa)
        """

        data_copy = data.copy(deep=True)  
        if strategy == 'both':
            data_copy.loc[data_copy[col]>para[0],col] = para[0]
            data_copy.loc[data_copy[col]<para[1],col] = para[1]
        elif strategy == 'top':
            data_copy.loc[data_copy[col]>para[0],col] = para[0]
        elif strategy == 'bottom':
            data_copy.loc[data_copy[col]<para[1],col] = para[1]  
        return data_copy


    df_out = windsorization(data=df,col='a',para=para,strategy='both')

Drop Outliers
==============================
.. code-block:: python
    :linenos: 

    ## run the top two examples
    df = df_test.copy()
    df_out = df[~index] 

Impute Outliers
==============================
.. code-block:: python
    :linenos: 

    def impute_outlier(data,col,outlier_index,strategy='mean'):
        """
        impute outlier with mean/median/most frequent values of that variable.
        """

        data_copy = data.copy(deep=True)
        if strategy=='mean':
            data_copy.loc[outlier_index,col] = data_copy[col].mean()
        elif strategy=='median':
            data_copy.loc[outlier_index,col] = data_copy[col].median()
        elif strategy=='mode':
            data_copy.loc[outlier_index,col] = data_copy[col].mode()[0]   
            
        return data_copy
    
    df_out = impute_outlier(data=df,col='a', outlier_index=index,strategy='mean')


Feature Engineering
***********************
Automated Dummy (one-hot) Encoding(func)
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["e"] = np.where(df["c"]> df["a"], 1,  2)
    def auto_dummy(df, unique=15):
        # Creating dummies for small object uniques
        if len(df)<unique:
            raise ValueError('unique is set higher than data lenght')
        list_dummies =[]
        for col in df.columns:
            if (len(df[col].unique()) < unique):
                list_dummies.append(col)
                print(col)
        df_edit = pd.get_dummies(df, columns = list_dummies) # Saves original dataframe
        #df_edit = pd.concat([df[["year","qtr"]],df_edit],axis=1)
        return df_edit

    df_out = auto_dummy(df, unique=3)

Binarise Empty Columns (func)
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df[df>df.mean()]  = None
    def binarise_empty(df, frac=80):
    # Binarise slightly empty columns
        this =[]
        for col in df.columns:
            if df[col].dtype != "object":
                is_null = df[col].isnull().astype(int).sum()
                if (is_null/df.shape[0]) >frac: # if more than 70% is null binarise
                    print(col)
                    this.append(col)
                    df[col] = df[col].astype(float)
                    df[col] = df[col].apply(lambda x: 0 if (np.isnan(x)) else 1)
        df = pd.get_dummies(df, columns = this) 
        return df

    df_out = binarise_empty(df, frac=0.6); df_out

Polynomials (func)
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def polynomials(df, feature_list):
        for feat in feature_list:
            for feat_two in feature_list:
                if feat==feat_two:
                    continue
                else:
                    df[feat+"/"+feat_two] = df[feat]/(df[feat_two]-df[feat_two].min()) #zero division guard
                    df[feat+"X"+feat_two] = df[feat]*(df[feat_two])
        return df

    df_out = polynomials(df, ["a","b"]) ; df_out

Transformations (func)
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def transformations(df,features):
        df_new = df[features]
        df_new = df_new - df_new.min()

        sqr_name = [str(fa)+"_POWER_2" for fa in df_new.columns]
        log_p_name = [str(fa)+"_LOG_p_one_abs" for fa in df_new.columns]
        rec_p_name = [str(fa)+"_RECIP_p_one" for fa in df_new.columns]
        sqrt_name = [str(fa)+"_SQRT_p_one" for fa in df_new.columns]

        df_sqr = pd.DataFrame(np.power(df_new.values, 2),columns=sqr_name, index=df.index)
        df_log = pd.DataFrame(np.log(df_new.add(1).abs().values),columns=log_p_name, index=df.index)
        df_rec = pd.DataFrame(np.reciprocal(df_new.add(1).values),columns=rec_p_name, index=df.index)
        df_sqrt = pd.DataFrame(np.sqrt(df_new.abs().add(1).values),columns=sqrt_name, index=df.index)
        
        dfs = [df, df_sqr, df_log, df_rec, df_sqrt]
        df=  pd.concat(dfs, axis=1)
        return df

    df_out = transformations(df,["a","b"]); df_out

Genetic Programming
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    from gplearn.genetic import SymbolicTransformer
    function_set = ['add', 'sub', 'mul', 'div',
                    'sqrt', 'log', 'abs', 'neg', 'inv','tan']

    gp = SymbolicTransformer(generations=800, population_size=200,
                            hall_of_fame=100, n_components=10,
                            function_set=function_set,
                            parsimony_coefficient=0.0005,
                            max_samples=0.9, verbose=1,
                            random_state=0, n_jobs=6)

    gen_feats = gp.fit_transform(df.drop("target", axis=1), df["target"]); df.iloc[:,:8]
    df_out = pd.concat((df,pd.DataFrame(gen_feats, columns=["gen_"+str(a) for a in range(gen_feats.shape[1])])),axis=1); df_out.iloc[:,:8]

Prinicipal Component Features (func)
=========================================
.. code-block:: python
    :linenos: 

    df =df_test.copy()
    from sklearn.decomposition import PCA, IncrementalPCA

    def pca_feature(df, memory_issues=False,mem_iss_component=False,variance_or_components=0.80,drop_cols=None):

        if memory_issues:
            if not mem_iss_component:
                raise ValueError("If you have memory issues, you have to preselect mem_iss_component")
        pca = IncrementalPCA(mem_iss_component)
        else:
            if variance_or_components>1:
                pca = PCA(n_components=variance_or_components) 
            else: # automted selection based on variance
                pca = PCA(n_components=variance_or_components,svd_solver="full") 
        X_pca = pca.fit_transform(df.drop(drop_cols,axis=1))
        df = pd.concat((df[drop_cols],pd.DataFrame(X_pca, columns=["PCA_"+str(i+1) for i in range(X_pca.shape[1])])),axis=1)
        return df

    df_out =pca_feature(df,variance_or_components=0.80,drop_cols=["target","a"]); df_out

Multiple Lags (func)
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def multiple_lags(df, start=1, end=3,columns=None):
        if not columns:
            columns = df.columns.to_list()
        lags = range(start, end+1)  # Just two lags for demonstration.

        df = df.assign(**{
        '{}_t_{}'.format(col, t): df[col].shift(t)
        for t in lags
        for col in columns
        })
        return df

    df_out = multiple_lags(df, start=1, end=2,columns=["a","target"]); df_out

Multiple Rolling (func)
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    def multiple_rolling(df, windows = [1,2], functions=["mean","std"], columns=None):
        windows = [1+a for a in windows]
        if not columns:
            columns = df.columns.to_list()
        rolling_dfs = (df[columns].rolling(i)                                    # 1. Create window
                        .agg(functions)                                # 1. Aggregate
                        .rename({col: '{0}_{1:d}'.format(col, i)
                                    for col in columns}, axis=1)  # 2. Rename columns
                    for i in windows)                                # For each window
        df_out = pd.concat((df, *rolling_dfs), axis=1)
        da = df_out.iloc[:,len(df.columns):]
        da = [col[0] + "_" + col[1] for col in  da.columns.to_list()]
        df_out.columns = df.columns.to_list() + da 

        return  df_out                      # 3. Concatenate dataframes

    df_out = multiple_rolling(df, columns=["a"])
    df_out

Date Features
=========================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["date_fake"] = pd.date_range(start="2019-01-03", end="2019-01-06", periods=len(df))
    def date_features(df, date="date"):
        df[date] = pd.to_datetime(df[date])
        df[date+"_month"] = df[date].dt.month.astype(int)
        df[date+"_year"]  = df[date].dt.year.astype(int)
        df[date+"_week"]  = df[date].dt.week.astype(int)
        df[date+"_day"]   = df[date].dt.day.astype(int)
        df[date+"_dayofweek"]= df[date].dt.dayofweek.astype(int)
        df[date+"_dayofyear"]= df[date].dt.dayofyear.astype(int)
        df[date+"_hour"] = df[date].dt.hour.astype(int)
        df[date+"_int"] = pd.to_datetime(df[date]).astype(int)
        return df

    df_out = date_features(df, date="date_fake"); df_out.iloc[:,:8]

Haversine Distance (Location Feature) (func)
=============================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["latitude"] = [39, 35 , 20]
    df["longitude"]=  [-77, -40 , -10 ]
    from math import sin, cos, sqrt, atan2, radians
    def haversine_distance(row, lon="latitude", lat="longitude"):
        c_lat,c_long = radians(52.5200), radians(13.4050)
        R = 6373.0
        long = radians(row['longitude'])
        lat = radians(row['latitude'])
        
        dlon = long - c_long
        dlat = lat - c_lat
        a = sin(dlat / 2)**2 + cos(lat) * cos(c_lat) * sin(dlon / 2)**2
        c = 2 * atan2(sqrt(a), sqrt(1 - a))
        
        return R * c

    df['distance_central'] = df.apply(haversine_distance,axis=1); df.iloc[:,4:]

Parse Address
=============================================
.. code-block:: python
    :linenos: 

    df = df_test.copy()
    df["addr"] = pd.Series([
                'Washington, D.C. 20003',
                'Brooklyn, NY 11211-1755',
                'Omaha, NE 68154' ])
    regex = (r'(?P<city>[A-Za-z ]+), (?P<state>[A-Z]{2}) (?P<zip>\d{5}(?:-\d{4})?)')  

    df.addr.str.replace('.', '').str.extract(regex)

Processing Strings in Pandas
=============================================
.. code-block:: python
    :linenos: 

    df = pd.util.testing.makeMixedDataFrame()
    df["C"] = df["C"] + " " + df["C"]


    """convert column to UPPERCASE"""

    col_name = "C"
    df[col_name].str.upper()

    """count string occurence in each row"""
    df[col_name].str.count(r'\d') # counts number of digits

    """count # o chars in each row"""
    df[col_name].str.count('o') # counts number of digits

    """split rows"""
    s = pd.Series(["this is a regular sentence", "https://docs.p.org", np.nan])
    s.str.split()

    """this creates new columns with the different split values (instead of lists)"""
    s.str.split(expand=True)  

    """limit the number of splits to 1, and start spliting from the rights side"""
    s.str.rsplit("/", n=1, expand=True) 

Filtering Strings in Pandas
=============================================
.. code-block:: python
    :linenos: 

    df = pd.util.testing.makeMixedDataFrame()
    df["C"] = df["C"] + " " + df["C"]
    col_name = "C"

    """check if a certain word/pattern occurs in each row"""
    df[col_name].str.contains('oo')  # returns True/False for each row

    """find occurences"""
    df[col_name].str.findall(r'[ABC]\d') # returns a list of the found occurences of the specified pattern for each row

    """replace Weekdays by abbrevations (e.g. Monday --> Mon)"""
    df[col_name].str.replace(r'(\w+day\b)', lambda x: x.groups[0][:3]) # () in r'' creates a group with one element, which we acces with x.groups[0]

    """create dataframe from regex groups (str.extract() uses first match of the pattern only)"""
    df[col_name].str.extract(r'(\d?\d):(\d\d)')
    df[col_name].str.extract(r'(?P<hours>\d?\d):(?P<minutes>\d\d)')
    df[col_name].str.extract(r'(?P<time>(?P<hours>\d?\d):(?P<minutes>\d\d))')

    """if you want to take into account ALL matches in a row (not only first one):"""
    df[col_name].str.extractall(r'(\d?\d):(\d\d)') # this generates a multiindex with level 1 = 'match', indicating the order of the match

    df[col_name].replace('\n', '', regex=True, inplace=True)

    """remove all the characters after &# (including &#) for column - col_1"""
    df[col_name].replace(' &#.*', '', regex=True, inplace=True)

    """remove white space at the beginning of string"""
    df[col_name] = df[col_name].str.lstrip()

Model Validation
*************************
Classification Metrics (func)
==============================
.. code-block:: python
    :linenos: 

    y_test = [0, 1, 1, 1, 0]
    y_predict = [0, 0, 1, 1, 1]
    y_prob = [0.2,0.6,0.7,0.7,0.9]
    from sklearn.metrics import roc_auc_score, average_precision_score, confusion_matrix
    from sklearn.metrics import log_loss, brier_score_loss, accuracy_score

    def classification_scores(y_test, y_predict, y_prob):

        confusion_mat = confusion_matrix(y_test,y_predict)

        TN = confusion_mat[0][0]
        FP = confusion_mat[0][1]
        TP = confusion_mat[1][1]
        FN = confusion_mat[1][0]

        TPR = TP/(TP+FN)
        # Specificity or true negative rate
        TNR = TN/(TN+FP) 
        # Precision or positive predictive value
        PPV = TP/(TP+FP)
        # Negative predictive value
        NPV = TN/(TN+FN)
        # Fall out or false positive rate
        FPR = FP/(FP+TN)
        # False negative rate
        FNR = FN/(TP+FN)
        # False discovery rate
        FDR = FP/(TP+FP)

        ll = log_loss(y_test, y_prob) # Its low but means nothing to me. 
        br = brier_score_loss(y_test, y_prob) # Its low but means nothing to me. 
        acc = accuracy_score(y_test, y_predict)
        print(acc)
        auc = roc_auc_score(y_test, y_prob)
        print(auc)
        prc = average_precision_score(y_test, y_prob) 

        data = np.array([np.arange(1)]*1).T

        df_exec = pd.DataFrame(data)

        df_exec["Average Log Likelihood"] = ll
        df_exec["Brier Score Loss"] = br
        df_exec["Accuracy Score"] = acc
        df_exec["ROC AUC Sore"] = auc
        df_exec["Average Precision Score"] = prc
        df_exec["Precision - Bankrupt Firms"] = PPV
        df_exec["False Positive Rate (p-value)"] = FPR
        df_exec["Precision - Healthy Firms"] = NPV
        df_exec["False Negative Rate (recall error)"] = FNR
        df_exec["False Discovery Rate "] = FDR
        df_exec["All Observations"] = TN + TP + FN + FP
        df_exec["Bankruptcy Sample"] = TP + FN
        df_exec["Healthy Sample"] = TN + FP
        df_exec["Recalled Bankruptcy"] = TP + FP
        df_exec["Correct (True Positives)"] = TP
        df_exec["Incorrect (False Positives)"] = FP
        df_exec["Recalled Healthy"] = TN + FN
        df_exec["Correct (True Negatives)"] = TN
        df_exec["Incorrect (False Negatives)"] = FN

        df_exec = df_exec.T[1:]
        df_exec.columns = ["Metrics"]
        return df_exec


    met = classification_scores(y_test, y_predict, y_prob); met