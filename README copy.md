# cheatsheet_Python_Stata_Mtalab

#### 介绍
Python、Stata、Mtalab小抄汇总

# Python、Stata、Mtalab小抄汇总

来源：https://cheatsheets.quantecon.org/

由计量经济学服务中心综合整理，转载请注明来源

本文由计量经济学服务中心由Markdown编辑整理，欢迎进入计量经济学仓库进行学习

[TOC]



# Statistics cheatsheet[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#statistics-cheatsheet)

- - [Basics](https://cheatsheets.quantecon.org/stats-cheatsheet.html#basics)
  - [Filtering data](https://cheatsheets.quantecon.org/stats-cheatsheet.html#filtering-data)
  - [Summarizing data](https://cheatsheets.quantecon.org/stats-cheatsheet.html#summarizing-data)
  - [Reshaping data](https://cheatsheets.quantecon.org/stats-cheatsheet.html#reshaping-data)
  - [Merging data](https://cheatsheets.quantecon.org/stats-cheatsheet.html#merging-data)
  - [Plotting](https://cheatsheets.quantecon.org/stats-cheatsheet.html#plotting)

In the Python code `import pandas as pd` has been run

## Basics[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#basics)

|                                  |                    STATA                    |                           PANDAS                           | BASE R                                    |
| :------------------------------: | :-----------------------------------------: | :--------------------------------------------------------: | ----------------------------------------- |
|  Create new dataset from values  |        `input a b 1 4 2 5 3 6 end `         | `d = {'a' : [1,2,3], 'b' : [4,5,6]} df = pd.DataFrame(d) ` | `df <- data.frame(a=1:3, b=4:6) `         |
| Create new dataset from csv file | `import delim mydata.csv, delimiters(",") ` |         `df = pd.read_csv('mydata.csv', sep=',') `         | `df <- read.csv('my_data.csv', sep=',') ` |
|        Print observations        |                   `list `                   |                           `df `                            | `df `                                     |
| Print observations of variable x |                  `list x `                  |                         `df['x'] `                         | `df$x `                                   |
|      Select only variable x      |                  `keep x `                  |                      `df = df['x'] `                       | `df <- df$x `                             |
|  Select only variables x and y   |                 `keep x y `                 |                   `df = df[['x', 'y']] `                   | `df <- df[c(‘x’, ‘y’)] `                  |
|         Drop variable x          |                  `drop x `                  |                `df = df.drop('x', axis=1) `                | `df$x <- NULL `                           |
|      Generate new variable       |              `gen z = x + y `               |               `df['z'] = df['x'] + df['y'] `               | `df$z <- df$x + df$y `                    |
|         Rename variable          |                `rename x y `                |            `df.rename(columns = {'x' : 'y'}) `             | `names(df)[names(df) == ‘x’] <- ‘y’ `     |
|         Sort by variable         |                  `sort x `                  |                   `df.sort_values('x') `                   | `df[order(df$x), ] `                      |

## Filtering data[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#filtering-data)

|                                                      |          STATA           |                PANDAS                | BASE R                        |
| :--------------------------------------------------: | :----------------------: | :----------------------------------: | ----------------------------- |
|           Conditionally print observations           |     `list if x > 1 `     |          `df[df['x'] > 1] `          | `subset(df, x == 1) `         |
| Conditionally print observations with ‘or’ operator  | `list if x > 1 | y < 0 ` | `df[(df['x'] > 1) | (df['y'] < 0)] ` | `subset(df, x == 1 | y < 0) ` |
| Conditionally print observations with ‘and’ operator | `list if x < 1 & y > 5 ` | `df[(df['x'] > 1) & (df['y'] < 0)] ` | `subset(df, x == 1 & y < 0) ` |
|    Print subset of observations based on location    |      `list in 1/3 `      |              `df[0:3] `              | `df[1:3, ] `                  |
|     Print observations with missing values in x      |  `list if missing(x) `   |       `df[df['x'].isnull()] `        | `subset(df, is.na(x)) `       |

## Summarizing data[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#summarizing-data)

|                                                       |             STATA              |               PANDAS               | BASE R                           |
| :---------------------------------------------------: | :----------------------------: | :--------------------------------: | -------------------------------- |
|               Print summary statistics                |          `summarize `          |          `df.describe() `          | `summary(df) `                   |
|   Print information about variables and data types    |          `describe `           |            `df.info() `            | `str(df) `                       |
|             Print aggregation of variable             |           `mean x `            |         `df['x'].mean() `          | `mean(df$x) `                    |
|         Group data by variable and summarize          |     `bysort x: summarize `     |   `df.groupby('x').describe() `    | `aggregate(. ~ x, df, summary) ` |
|                 Print frequency table                 |            `tab x `            |     `df['x'].value_counts() `      | `table(df$x) `                   |
|                Print cross-tabulation                 |           `tab x y `           |  `pd.crosstab(df['x'], df['y']) `  | `table(df$x, df$y) `             |
| Create bins based on values in x in new column ‘bins’ | `egen bins = cut x, group(3) ` | `df['bins'] = pd.cut(df['x'], 3) ` | `df$bins <- cut(df$x, 3) `       |

## Reshaping data[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#reshaping-data)

|                                      |            STATA             |                     PANDAS                     | BASE R                                                       |
| :----------------------------------: | :--------------------------: | :--------------------------------------------: | ------------------------------------------------------------ |
| Reshape data from wide to long panel | `reshape long x, i(i) j(j) ` |  `pd.wide_to_long(df, ['x'], i='i', j='j') `   | `reshape(df, direction='long', varying=grep('j', names(df), value=TRUE), sep='') ` |
| Reshape data from long to wide panel |       `reshape wide `        | `df.unstack() # returns hierarchical columns ` | `reshape(df, timevar='x', idvar='i', direction='wide') `     |

## Merging data[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#merging-data)

|              STATA              |          PANDAS          |                  BASE R                  |                                                              |
| :-----------------------------: | :----------------------: | :--------------------------------------: | ------------------------------------------------------------ |
| Vertically concatenate datasets |    `append using y `     |           `pd.concat([x, y]) `           | `rbind(x, y) # note that columns must be the same for each dataset ` |
|      Merge datasets on key      | `merge 1:1 key using y ` | `pd.merge(x, y, on='key', how='inner') ` | `merge(x, y, by='key') `                                     |

## Plotting[¶](https://cheatsheets.quantecon.org/stats-cheatsheet.html#plotting)

|    STATA     |     PANDAS     |            BASE R            |                      |
| :----------: | :------------: | :--------------------------: | -------------------- |
| Scatter plot |  `plot x y `   | `df.plot.scatter('x', 'y') ` | `plot(df$x, df$y) `  |
|  Line plot   |  `line x y `   |     `df.plot('x', 'y') `     | `lines(df$x, df$y) ` |
|  Histogram   |   `hist x `    |       `df.hist('x') `        | `hist(df$x) `        |
|   Boxplot    | `graph box x ` |      `df.boxplot('x') `      | `boxplot(df$x)`      |



------



# Python cheatsheet[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#python-cheatsheet)

- - [Operators](https://cheatsheets.quantecon.org/python-cheatsheet.html#operators)
  - [Data Types](https://cheatsheets.quantecon.org/python-cheatsheet.html#data-types)
  - [Built-In Functions](https://cheatsheets.quantecon.org/python-cheatsheet.html#built-in-functions)
  - [Iterating](https://cheatsheets.quantecon.org/python-cheatsheet.html#iterating)
  - [Comparisons and Logical Operators](https://cheatsheets.quantecon.org/python-cheatsheet.html#comparisons-and-logical-operators)
  - [User-Defined Functions](https://cheatsheets.quantecon.org/python-cheatsheet.html#user-defined-functions)
  - [Numpy](https://cheatsheets.quantecon.org/python-cheatsheet.html#numpy)
  - [numpy.linalg](https://cheatsheets.quantecon.org/python-cheatsheet.html#numpy-linalg)
  - [Pandas](https://cheatsheets.quantecon.org/python-cheatsheet.html#pandas)
  - [Plotting](https://cheatsheets.quantecon.org/python-cheatsheet.html#plotting)

## Operators[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#operators)

| Command | Description                                                  |
| :------ | :----------------------------------------------------------- |
| `*`     | multiplication operation: `2*3` returns `6`                  |
| `**`    | power operation: `2**3` returns `8`                          |
| `@`     | matrix multiplication:`import numpy as np A = np.array([[1, 2, 3]]) B = np.array([[3], [2], [1]]) A @ B `returns`array([[10]]) ` |

## Data Types[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#data-types)

| Command              | Description                                                  |
| :------------------- | :----------------------------------------------------------- |
| `l = [a1, a2,…, an]` | Constructs a list containing the objects a1,a2,...,ana1,a2,...,an. You can append to the list using `l.append()`. The ithith element of ll can be accessed using `l[i]` |
| `t =(a1, a2,…, an)`  | Constructs a tuple containing the objects a1,a2,...,ana1,a2,...,an. The ithith element of tt can be accessed using `t[i]` |

## Built-In Functions[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#built-in-functions)

| Command         | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| `len(iterable)` | `len` is a function that takes an iterable, such as a list, tuple or numpy array and returns the number of items in that object. For a numpy array, `len` returns the length of the outermost dimension`len(np.zeros((5, 4))) `returns `5`. |
| `zip`           | Make an iterator that aggregates elements from each of the iterables.`x = [1, 2, 3] y = [4, 5, 6] zipped = zip(x, y) list(zipped) `returns `[(1, 4), (2, 5), (3, 6)]` |

## Iterating[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#iterating)

| Command              | Description                                                  |
| :------------------- | :----------------------------------------------------------- |
| `for a in iterable:` | For loop used to perform a sequence of commands (denoted using tabs) for each element in an iterable object such as a list, tuple, or numpy array. An example code is`l  = [] for i in [1, 2, 3]:    l.append(i**2) print(l) `prints `[1, 4, 9]` |

## Comparisons and Logical Operators[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#comparisons-and-logical-operators)

| Command         | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| `if condition:` | Performs code if a condition is met (using tabs). For example`if x == 5:    x = x**2 else:    x = x**3 `squares xx if xx is 55, otherwise cubes it. |

## User-Defined Functions[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#user-defined-functions)

| Command  | Description                                                  |
| :------- | :----------------------------------------------------------- |
| `lambda` | Used for create anonymous one line functions of the form:`f = lambda x, y: 5*x+y `The code after the lambda but before variables specifies the parameters. The code after the colon tells python what object to return. |
| `def`    | The def command is used to create functions of more than one line:`def g(x, y):    """    Docstring    """    ret = sin(x)    return ret + y `The code immediately following `def` names the function, in this example `g` . The variables in the parenthesis are the parameters of the function. The remaining lines of the function are denoted by tab indents. The return statement specifies the object to be returned. |

## Numpy[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#numpy)

| Command                          | Description                                                  |
| :------------------------------- | :----------------------------------------------------------- |
| `np.array(object, dtype = None)` | `np.array` constructs a numpy array from an object, such as a list or a list of lists. `dtype`allows you to specify the type of object the array is holding. You will generally note need to specify the `dtype`. Examples:`np.array([1, 2, 3]) #creates 1 dim array of ints np.array( [1, 2, 3.0] )#creates 1 dim array of floats np.array( [ [1, 2], [3, 4] ]) #creates a 2 dim array ` |
| `A[i1, i2,…, in]`                | Access a the element in numpy array A in with index i1 in dimension 1, i2 in dimension 2, etc. Can use `:` to access a range of indices, where `imin:imax` represents all ii such that imin≤i<imaximin≤i<imax. Always returns an object of minimal dimension. For example,`A[:, 2]`returns the 2nd column (counting from 0) of A as a 1 dimensional array and`A[0:2, :]`returns the 0th and 1st rows in a 2 dimensional array. |
| `np.zeros(shape)`                | Constructs numpy array of shape shape. Here shape is an integer of sequence of integers. Such as 3, (1, 2), (2, 1), or (5, 5). Thus`np.zeros((5, 5))`Constructs an 5×55×5 array while`np.zeros(5, 5)`will throw an error. |
| `np.ones(shape)`                 | Same as `np.zeros` but produces an array of ones             |
| `np.linspace(a, b, n)`           | Returns a numpy array with nn linearly spaced points between aa and bb. For example`np.linspace(1, 2, 10)`returns`array([ 1.        , 1.11111111, 1.22222222, 1.33333333, 1.44444444, 1.55555556, 1.66666667, 1.77777778, 1.88888889, 2.        ]) ` |
| `np.eye(N)`                      | Constructs the identity matrix of size NN. For example`np.eye(3)`returns the 3×33×3 identity matrix:⎛⎝⎜100010001⎞⎠⎟(100010001) |
| `np.diag(a)`                     | `np.diag` has 2 uses. First if `a` is a 2 dimensional array then `np.diag` returns the principle diagonal of the matrix. Thus`np.diag( [ [1, 3], [5, 6] ])`returns `[1, 6]`.If aa is a 1 dimensional array then `np.diag` constructs an array with $a$ as the principle diagonal. Thus,`np.diag([1, 2])`returns(1002)(1002) |
| `np.random.rand(d0, d1,…, dn)`   | Constructs a numpy array of shape `(d0, d1,…, dn)` filled with random numbers drawn from a uniform distribution between :math`(0, 1)`. For example, `np.random.rand(2, 3)`returns`array([[ 0.69060674, 0.38943021, 0.19128955], [ 0.5419038 , 0.66963507, 0.78687237]]) ` |
| `np.random.randn(d0, d1,…, dn)`  | Same as `np.random.rand(d0, d1,…, dn)` except that it draws from the standard normal distribution N(0,1)N(0,1) rather than the uniform distribution. |
| `A.T`                            | Reverses the dimensions of an array (transpose). For example, if x=(1324)x=(1234) then `x.T`returns (1234)(1324) |
| `np.hstack(tuple)`               | Take a sequence of arrays and stack them horizontally to make a single array. For example`a = np.array( [1, 2, 3] ) b = np.array( [2, 3, 4] ) np.hstack( (a, b) ) `returns `[1, 2, 3, 2, 3, 4]` while`a = np.array( [[1], [2], [3]] ) b = np.array( [[2], [3], [4]] ) np.hstack((a, b)) `returns ⎛⎝⎜123234⎞⎠⎟(122334) |
| `np.vstack(tuple)`               | Like `np.hstack`. Takes a sequence of arrays and stack them vertically to make a single array. For example`a = np.array( [1, 2, 3] ) b = np.array( [2, 3, 4] ) np.hstack( (a, b) ) `returns`array( [ [1, 2, 3], [2, 3, 4] ] ) ` |
| `np.amax(a, axis = None)`        | By default `np.amax(a)` finds the maximum of all elements in the array aa. Can specify maximization along a particular dimension with axis. If`a = np.array( [ [2, 1], [3, 4] ]) #creates a 2 dim array`then`np.amax(a, axis = 0) #maximization along row (dim 0)`returns `array([3, 4])` and`np.amax(a, axis = 1) #maximization along column (dim 1)`returns `array([2, 4])` |
| `np.amin(a, axis = None)`        | Same as `np.amax` except returns minimum element.            |
| `np.argmax(a, axis = None)`      | Performs similar function to np.amax except returns index of maximal element. By default gives index of flattened array, otherwise can use axis to specify dimension. From the example for np.amax`np.amax(a, axis = 0) #maximization along row (dim 0) `returns `array([1, 1])` and`np.amax(a, axis = 1) #maximization along column (dim 1) `returns `array([0, 1])` |
| `np.argmin(a, axis =None)`       | Same as `np.argmax` except finds minimal index.              |
| `np.dot(a, b)` or `a.dot(b)`     | Returns an array equal to the dot product of aa and bb. For this operation to work the innermost dimension of aa must be equal to the outermost dimension of bb. If aa is a (3,2)(3,2)array and bb is a (2)(2) array then `np.dot(a, b)` is valid. If bb is a (1,2)(1,2) array then the operation will return an error. |

## numpy.linalg[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#numpy-linalg)

| Command                 | Description                                                  |
| :---------------------- | :----------------------------------------------------------- |
| `np.linalg.inv(A)`      | For a 2-dimensional array AA. `np.linalg.inv` returns the inverse of AA. For example, for a (2,2)(2,2) array AA`np.linalg.inv(A).dot(A) `returns`np.array( [ [1, 0], [0, 1] ]) ` |
| `np.linalg.eig(A)`      | Returns a 1-dimensional array with all the eigenvalues of $A$ as well as a 2-dimensional array with the eigenvectors as columns. For example,`eigvals, eigvecs = np.linalg.eig(A)`returns the eigenvalues in `eigvals` and the eigenvectors in `eigvecs`. `eigvecs[:, i]` is the eigenvector of AA with eigenvalue of `eigval[i]`. |
| `np.linalg.solve(A, b)` | Constructs array xx such that `A.dot(x)` is equal to bb. Theoretically should give the same answer as`Ainv = np.linalg.inv(A) x = Ainv.dot(b) `but numerically more stable. |

## Pandas[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#pandas)

| Command        | Description                                                  |
| :------------- | :----------------------------------------------------------- |
| pd.Series()    | Constructs a Pandas Series Object from some specified data and/or index`s1 = pd.Series([1, 2, 3]) s2 = pd.Series([1, 2, 3], index=['a', 'b', 'c']) ` |
| pd.DataFrame() | Constructs a Pandas DataFrame object from some specified data and/or index, column names etc.`d = {'a' : [1, 2, 3], 'b' : [4, 5, 6]} df = pd.DataFrame(d) `or alternatively,`a = [1, 2, 3] b = [4, 5, 6] df = pd.DataFrame(list(zip(a, b)), columns=['a', 'b']) ` |

## Plotting[¶](https://cheatsheets.quantecon.org/python-cheatsheet.html#plotting)

| Command                   | Description                                                  |
| :------------------------ | :----------------------------------------------------------- |
| `plt.plot(x, y, s =None)` | The plot command is included in `matplotlib.pyplot`. The plot command is used to plot xx versus yy where xx and yy are iterables of the same length. By default the plot command draws a line, using the ss argument you can specify type of line and color. For example ‘-‘, ‘- -‘, ‘:’, ‘o’, ‘x’, and ‘-o’ reprent line, dashed line, dotted line, circles, x’s, and circle with line through it respectively. Color can be changed by appending ‘b’, ‘k’, ‘g’ or ‘r’, to get a blue, black, green or red plot respectively. For example,`import numpy as np import matplotlib.pyplot as plt x=np.linspace(0, 10, 100) N=len(x) v= np.cos(x) plt.figure(1) plt.plot(x, v, '-og') plt.show() plt.savefig('tom_test.eps') `plots the cosine function on the domain (0, 10) with a green line with circles at the points x,v |

------



# MATLAB–Python–Julia cheatsheet[¶](https://cheatsheets.quantecon.org/index.html#matlab-python-julia-cheatsheet)

- - [Dependencies and Setup](https://cheatsheets.quantecon.org/index.html#dependencies-and-setup)
  - [Creating Vectors](https://cheatsheets.quantecon.org/index.html#creating-vectors)
  - [Creating Matrices](https://cheatsheets.quantecon.org/index.html#creating-matrices)
  - [Manipulating Vectors and Matrices](https://cheatsheets.quantecon.org/index.html#manipulating-vectors-and-matrices)
  - [Accessing Vector/Matrix Elements](https://cheatsheets.quantecon.org/index.html#accessing-vector-matrix-elements)
  - [Mathematical Operations](https://cheatsheets.quantecon.org/index.html#mathematical-operations)
  - [Sum / max / min](https://cheatsheets.quantecon.org/index.html#sum-max-min)
  - [Programming](https://cheatsheets.quantecon.org/index.html#programming)

## Dependencies and Setup[¶](https://cheatsheets.quantecon.org/index.html#dependencies-and-setup)

In the Python code we assume that you have already run `import numpy as np`

In the Julia, we assume you are using **v1.0.2 or later** with Compat **v1.3.0 or later** and have run `using LinearAlgebra, Statistics, Compat`

## Creating Vectors[¶](https://cheatsheets.quantecon.org/index.html#creating-vectors)

|                                       |          MATLAB          |                  PYTHON                  | JULIA                              |
| :-----------------------------------: | :----------------------: | :--------------------------------------: | ---------------------------------- |
|        Row vector: size (1, n)        |      `A = [1 2 3] `      | `A = np.array([1, 2, 3]).reshape(1, 3) ` | `A = [1 2 3] `                     |
|      Column vector: size (n, 1)       |     `A = [1; 2; 3] `     | `A = np.array([1, 2, 3]).reshape(3, 1) ` | `A = [1 2 3]' `                    |
|         1d array: size (n, )          |       Not possible       |        `A = np.array([1, 2, 3]) `        | `A = [1; 2; 3] `or`A = [1, 2, 3] ` |
| Integers from j to n with step size k |       `A = j:k:n `       |       `A = np.arange(j, n+1, k) `        | `A = j:k:n `                       |
|  Linearly spaced vector of k points   | `A = linspace(1, 5, k) ` |       `A = np.linspace(1, 5, k) `        | `A = range(1, 5, length = k) `     |

## Creating Matrices[¶](https://cheatsheets.quantecon.org/index.html#creating-matrices)

|                        |                            MATLAB                            |                            PYTHON                            | JULIA                                                        |
| :--------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|    Create a matrix     |                      `A = [1 2; 3 4] `                       |              `A = np.array([[1, 2], [3, 4]]) `               | `A = [1 2; 3 4] `                                            |
| 2 x 2 matrix of zeros  |                      `A = zeros(2, 2) `                      |                   `A = np.zeros((2, 2)) `                    | `A = zeros(2, 2) `                                           |
|  2 x 2 matrix of ones  |                      `A = ones(2, 2) `                       |                    `A = np.ones((2, 2)) `                    | `A = ones(2, 2) `                                            |
| 2 x 2 identity matrix  |                       `A = eye(2, 2) `                       |                       `A = np.eye(2) `                       | `A = I # will adopt # 2x2 dims if demanded by # neighboring matrices ` |
|    Diagonal matrix     |                     `A = diag([1 2 3]) `                     |                  `A = np.diag([1, 2, 3]) `                   | `A = Diagonal([1, 2,    3]) `                                |
| Uniform random numbers |                      `A = rand(2, 2) `                       |                 `A = np.random.rand(2, 2) `                  | `A = rand(2, 2) `                                            |
| Normal random numbers  |                      `A = randn(2, 2) `                      |                 `A = np.random.randn(2, 2) `                 | `A = randn(2, 2) `                                           |
|    Sparse Matrices     |         `A = sparse(2, 2) A(1, 2) = 4 A(2, 2) = 1 `          | `from scipy.sparse import coo_matrix A = coo_matrix(([4, 1],                ([0, 1], [1, 1])),                shape=(2, 2)) ` | `using SparseArrays A = spzeros(2, 2) A[1, 2] = 4 A[2, 2] = 1 ` |
|  Tridiagonal Matrices  | `A = [1 2 3 NaN;     4 5 6 7;     NaN 8 9 0] spdiags(A',[-1 0 1], 4, 4) ` | `import sp.sparse as sp diagonals = [[4, 5, 6, 7], [1, 2, 3], [8, 9, 10]] sp.diags(diagonals, [0, -1, 2]).toarray() ` | `x = [1, 2, 3] y = [4, 5, 6, 7] z = [8, 9, 10] Tridiagonal(x, y, z) ` |

## Manipulating Vectors and Matrices[¶](https://cheatsheets.quantecon.org/index.html#manipulating-vectors-and-matrices)

|                                                              |                            MATLAB                            |                            PYTHON                            | JULIA                                                        |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|                          Transpose                           |                            `A.' `                            |                            `A.T `                            | `transpose(A) `                                              |
|            Complex conjugate transpose (Adjoint)             |                            `A' `                             |                         `A.conj() `                          | `A' `                                                        |
|                   Concatenate horizontally                   |      `A = [[1 2] [1 2]] `or`A = horzcat([1 2], [1 2]) `      |        `B = np.array([1, 2]) A = np.hstack((B, B)) `         | `A = [[1 2] [1 2]] `or`A = hcat([1 2], [1 2]) `              |
|                    Concatenate vertically                    |     `A = [[1 2]; [1 2]] `or`A = vertcat([1 2], [1 2]) `      |        `B = np.array([1, 2]) A = np.vstack((B, B)) `         | `A = [[1 2]; [1 2]] `or`A = vcat([1 2], [1 2]) `             |
|                Reshape (to 5 rows, 2 columns)                |                  `A = reshape(1:10, 5, 2) `                  |                    `A = A.reshape(5, 2) `                    | `A = reshape(1:10, 5, 2) `                                   |
|                   Convert matrix to vector                   |                           `A(:) `                            |                      `A = A.flatten() `                      | `A[:] `                                                      |
|                       Flip left/right                        |                         `fliplr(A) `                         |                       `np.fliplr(A) `                        | `reverse(A, dims = 2) `                                      |
|                         Flip up/down                         |                         `flipud(A) `                         |                       `np.flipud(A) `                        | `reverse(A, dims = 1) `                                      |
| Repeat matrix (3 times in the row dimension, 4 times in the column dimension) |                      `repmat(A, 3, 4) `                      |                    `np.tile(A, (4, 3)) `                     | `repeat(A, 3, 4) `                                           |
|                    Preallocating/Similar                     | `x = rand(10) y = zeros(size(x, 1), size(x, 2)) `N/A similar type | `x = np.random.rand(3, 3) y = np.empty_like(x) # new dims y = np.empty((2, 3)) ` | `x = rand(3, 3) y = similar(x) # new dims y = similar(x, 2, 2) ` |
|     Broadcast a function over a collection/matrix/vector     | `f = @(x) x.^2 g = @(x, y) x + 2 + y.^2 x = 1:10 y = 2:11 f(x) g(x, y) `Functions broadcast directly | `def f(x):    return x**2 def g(x, y):    return x + 2 + y**2 x = np.arange(1, 10, 1) y = np.arange(2, 11, 1) f(x) g(x, y) `Functions broadcast directly | `f(x) = x^2 g(x, y) = x + 2 + y^2 x = 1:10 y = 2:11 f.(x) g.(x, y) ` |

## Accessing Vector/Matrix Elements[¶](https://cheatsheets.quantecon.org/index.html#accessing-vector-matrix-elements)

|                          |          MATLAB          |           PYTHON            | JULIA                   |
| :----------------------: | :----------------------: | :-------------------------: | ----------------------- |
|    Access one element    |        `A(2, 2) `        |         `A[1, 1] `          | `A[2, 2] `              |
|   Access specific rows   |       `A(1:4, :) `       |        `A[0:4, :] `         | `A[1:4, :] `            |
| Access specific columns  |       `A(:, 1:4) `       |        `A[:, 0:4] `         | `A[:, 1:4] `            |
|       Remove a row       |     `A([1 2 4], :) `     |     `A[[0, 1, 3], :] `      | `A[[1, 2, 4], :] `      |
|   Diagonals of matrix    |        `diag(A) `        |        `np.diag(A) `        | `diag(A) `              |
| Get dimensions of matrix | `[nrow ncol] = size(A) ` | `nrow, ncol = np.shape(A) ` | `nrow, ncol = size(A) ` |

## Mathematical Operations[¶](https://cheatsheets.quantecon.org/index.html#mathematical-operations)

|                            MATLAB                            |         PYTHON         |                            JULIA                             |                                                           |
| :----------------------------------------------------------: | :--------------------: | :----------------------------------------------------------: | --------------------------------------------------------- |
|                         Dot product                          |      `dot(A, B) `      |                   `np.dot(A, B) or A @ B `                   | `dot(A, B) A ⋅ B # \cdot `                                |
|                    Matrix multiplication                     |        `A * B `        |                           `A @ B `                           | `A * B `                                                  |
|                Inplace matrix multiplication                 |      Not possible      | `x = np.array([1, 2]).reshape(2, 1) A = np.array(([1, 2], [3, 4])) y = np.empty_like(x) np.matmul(A, x, y) ` | `x = [1, 2] A = [1 2; 3 4] y = similar(x) mul!(y, A, x) ` |
|                 Element-wise multiplication                  |       `A .* B `        |                           `A * B `                           | `A .* B `                                                 |
|                      Matrix to a power                       |         `A^2 `         |               `np.linalg.matrix_power(A, 2) `                | `A^2 `                                                    |
|                Matrix to a power, elementwise                |        `A.^2 `         |                           `A**2 `                            | `A.^2 `                                                   |
|                           Inverse                            |  `inv(A) `or`A^(-1) `  |                     `np.linalg.inv(A) `                      | `inv(A) `or`A^(-1) `                                      |
|                         Determinant                          |       `det(A) `        |                     `np.linalg.det(A) `                      | `det(A) `                                                 |
|                 Eigenvalues and eigenvectors                 | `[vec, val] = eig(A) ` |                `val, vec = np.linalg.eig(A) `                | `val, vec = eigen(A) `                                    |
|                        Euclidean norm                        |       `norm(A) `       |                     `np.linalg.norm(A) `                     | `norm(A) `                                                |
|       Solve linear system Ax=bAx=b (when AA is square)       |         `A\b `         |                   `np.linalg.solve(A, b) `                   | `A\b `                                                    |
| Solve least squares problem Ax=bAx=b (when AA is rectangular) |         `A\b `         |                   `np.linalg.lstsq(A, b) `                   | `A\b `                                                    |

## Sum / max / min[¶](https://cheatsheets.quantecon.org/index.html#sum-max-min)

|                                      |                  MATLAB                   |                            PYTHON                            | JULIA                                                        |
| :----------------------------------: | :---------------------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|    Sum / max / min of each column    | `sum(A, 1) max(A, [], 1) min(A, [], 1) `  |           `sum(A, 0) np.amax(A, 0) np.amin(A, 0) `           | `sum(A, dims = 1) maximum(A, dims = 1) minimum(A, dims = 1) ` |
|     Sum / max / min of each row      | `sum(A, 2) max(A, [], 2) min(A, [], 2) `  |           `sum(A, 1) np.amax(A, 1) np.amin(A, 1) `           | `sum(A, dims = 2) maximum(A, dims = 2) minimum(A, dims = 2) ` |
|   Sum / max / min of entire matrix   |     `sum(A(:)) max(A(:)) min(A(:)) `      |              `np.sum(A) np.amax(A) np.amin(A) `              | `sum(A) maximum(A) minimum(A) `                              |
|  Cumulative sum / max / min by row   | `cumsum(A, 1) cummax(A, 1) cummin(A, 1) ` | `np.cumsum(A, 0) np.maximum.accumulate(A, 0) np.minimum.accumulate(A, 0) ` | `cumsum(A, dims = 1) accumulate(max, A, dims = 1) accumulate(min, A, dims = 1) ` |
| Cumulative sum / max / min by column | `cumsum(A, 2) cummax(A, 2) cummin(A, 2) ` | `np.cumsum(A, 1) np.maximum.accumulate(A, 1) np.minimum.accumulate(A, 1) ` | `cumsum(A, dims = 2) accumulate(max, A, dims = 2) accumulate(min, A, dims = 2) ` |

## Programming[¶](https://cheatsheets.quantecon.org/index.html#programming)

|                                    |                            MATLAB                            |                            PYTHON                            | JULIA                                                        |
| :--------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|          Comment one line          |                    `% This is a comment `                    |                    `# This is a comment `                    | `# This is a comment `                                       |
|           Comment block            |                    `%{ Comment block %} `                    |            `# Block # comment # following PEP8 `             | `#= Comment block =# `                                       |
|              For loop              |             `for i = 1:N   % do something end `              |           `for i in range(n):    # do something `            | `for i in 1:N   # do something end `                         |
|             While loop             |             `while i <= N   % do something end `             |              `while i <= N:    # do something `              | `while i <= N   # do something end `                         |
|                 If                 |              `if i <= N   % do something end `               |                `if i <= N:   # do something `                | `if i <= N   # do something end `                            |
|             If / else              | `if i <= N   % do something else   % do something else end ` | `if i <= N:    # do something else:    # so something else ` | `if i <= N   # do something else   # do something else end ` |
|      Print text and variable       |              `x = 10 fprintf('x = %d \n', x) `               |                 `x = 10 print(f'x = {x}') `                  | `x = 10 println("x = $x") `                                  |
|        Function: anonymous         |                       `f = @(x) x^2 `                        |                    `f = lambda x: x**2 `                     | `f = x -> x^2 # can be rebound `                             |
|              Function              |           `function out  = f(x)   out = x^2 end `            |                 `def f(x):    return x**2 `                  | `function f(x)   return x^2 end f(x) = x^2 # not anon! `     |
|               Tuples               | `t = {1 2.0 "test"} t{1} `Can use cells but watch performance |                 `t = (1, 2.0, "test") t[0] `                 | `t = (1, 2.0, "test") t[1] `                                 |
| Named Tuples/ Anonymous Structures |                    `m.x = 1 m.y = 2 m.x `                    | `from collections import namedtuple mdef = namedtuple('m', 'x y') m = mdef(1, 2) m.x ` | `# vanilla m = (x = 1, y = 2) m.x # constructor using Parameters mdef = @with_kw (x=1, y=2) m = mdef() # same as above m = mdef(x = 3) ` |
|              Closures              |               `a = 2.0 f = @(x) a + x f(1.0) `               |         `a = 2.0 def f(x):    return a + x f(1.0) `          | `a = 2.0 f(x) = a + x f(1.0) `                               |
|        Inplace Modification        | `function f(out, x)     out = x.^2 end x = rand(10) y = zeros(length(x), 1) f(y, x) ` | `def f(x):    x **=2    return x = np.random.rand(10) f(x) ` | `function f!(out, x)    out .= x.^2 end x = rand(10) y = similar(x) f!(y, x)` |
