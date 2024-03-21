# Create and Run Notebooks in Oracle Machine Learning
## Introduction

This lab shows you how to create a notebook and run it in Oracle Machine Learning Notebooks.

Oracle Machine Learning Notebooks is a web-based interface for data analysis, data discovery, and data visualization. Whenever a notebook is created, it must be defined with a specific interpreter binding specifications. The notebook contains an internal list of bindings that determines the order of the interpreter bindings.

An Oracle Machine Learning notebook supports multiple languages interpreters. Each paragraph is associated with a specific interpreter. For example, to run SQL statements use the SQL interpreter. To run PL/SQL statements, use the "script" interpreter.

You identify which interpreter to use by specifying ``%`` followed by the interpreter to use. For example:

  * ``%sql`` — To call the SQL interpreter and run SQL statements
  * ``%script`` — To call PL/SQL interpreter and run PL/SQL scripts
  * ``%md`` — To call the Markdown interpreter and generate static html from Markdown plain text
  * ``%python`` — To call the Python interpreter and run Python scripts
  * ``%r`` — To call the R interpreter and run R scripts.
  * ``%conda`` — To connect to the Conda interpreter, and install third-party Python and R libraries inside a notebook session.

A notebook comprises paragraphs which is a notebook component where you can write and run SQL statements, PL/SQL scripts, run R and Python commands. A paragraph has an input section and an output section. In the input section, specify the interpreter to run along with the text. This information is sent to the interpreter to be run. In the output section, the results of the interpreter are provided.

### Estimated Time
This lab takes approximately 10 minutes to complete.

### Prerequisites

* Access to your Oracle Machine Learning Notebooks account
* A project created in your Oracle Machine Learning Notebooks account, where the notebook will reside



## Task 1: Create Your Notebook

To create a notebook:

1. Sign in to your Oracle Machine Learning Notebooks account and click **Notebooks** on the home page.

   ![OML homepage](images/oml-homepage.png "OML homepage")

2. On the Notebooks page, click **Create**. The Create Notebook dialog box opens.

   ![Create Notebook dialog](images/create-notebook.png "Create Notebook dialog")

3. In the **Name** field, provide a name for the notebook.

4. In the **Comments** field, enter comments if any.

5. The **Connection** field specifies the Global connection group.

6. Click **OK**. Your notebook is created and it opens in the notebook editor. You can now use it to run SQL statements, run PL/SQL scripts, and run Python commands. To do so, specify any one of the following directives in the input section of the paragraph:

    * `%sql` - To call the SQL interpreter and run SQL statements
    * `%script` - To call the PL/SQL interpreter and run PL/SQL scripts
    * `%r` - To call the R interpreter and run R commands
    * `%python` - To call the Python interpreter and run python statements
    * `%md` - To call the Markdown interpreter and generate static html from Markdown plain text


7. Click **Back** to return to the Notebooks page, and to save the changes in the notebook.

## Task 2: Create Your Notebook From Examples Templates

To create a notebook based on a template:

1. Sign in to your Oracle Machine Learning Notebooks account and click **Examples** on the home page.

    ![OML Homepage Examples](images/oml-homepage-examples.png "OML Homepage Examples")

2. On the Examples Template page, click the example template based on which you want to create your notebook and then click **Create Notebook**.  Note that clicking the notebook name opens a read-only notebook so that you can see the contents. Clicking anywhere else on the template box selects the template and then **Create Notebook** is enabled. In this example, you click **Anomaly Detection** example template. The Create Notebook dialog box opens.

    ![Example Templates](images/example-templates.png "Example Templates")

3. In the Create Notebook dialog, enter the following details:
     * **Name**: Enter a name for the notebook. In this example, the user Test Anomaly Detection
     * **Comment**: Enter comments, if any.
     * **Project**: Click the pencil icon to navigate and select the project in which you want to save the notebook. In this example, the notebook is saved in the default USER1 Project inside USER1 Workspace.
     * **Connection**: By default, the connection is set to Global.

     ![Create Example Notebook](images/create-example-notebook.png "Create Example Notebook")

4. Click **OK**. Once the notebook is created successfully, a message appears stating that the notebook is created in the project. The newly created notebook is listed in the Notebooks page.

     ![message](images/message.png "message")

5. To open the notebook, click the Cloud menu on the top to open the left navigation menu. On the left navigation menu, click **Notebooks.**

     ![Navigation Menu](images/navigation-menu.png "Navigation Menu")

6. In the Notebooks page, click **Test Anomaly Detection**. The notebook opens in the notebook editor. You are now ready to edit and run the notebook. This completes the task of creating a notebook based on an Example Template.

     ![Test Anomaly Detection](images/test-anomaly-detection.png "Test Anomaly Detection notebook")

## Task 3: Run your Notebook using the Conda Interpreter

Conda is an open-source package and environment management system that allows the use of environments containing third-party Python and R libraries. Oracle Machine Learning Notebooks provide the conda interpreter to install third-party Python and R libraries inside a notebook session.
Third-party libraries installed in Oracle Machine Learning Notebooks can be used in:

* Standard Python
* Standard R
* Oracle Machine Learning for Python embedded Python execution from the Python, SQL and REST APIs
* Oracle Machine Learning for R embedded R execution from the R, SQL, and REST APIs

This topic shows how to start working in the Conda environment:

* Connect to the Conda interpreter
* Download and activate the Conda environment
* View the list of packages in the Conda environment
* Run a Python function to import the Iris dataset, and use the seaborn package for visualization

1. Type %conda at the beginning of the paragraph to connect to the Conda interpreter, and press Enter.

    ```
    <copy>
    %conda
    </copy>
    ```

2. Next, download and activate the Conda environment. Type:

    ```
    <copy>
    download sbenv
    activate sbenv
    </copy>
    ```
    In this example, the Conda environment is downloaded and activated. The name of the Conda environment in this example is `sbenv`.
    ![Download and Activate Conda environment](images/download-activate-conda.png "Download and Activate Conda environment")

3. You can view all the packages that are present in the Conda environment. To view the list of packages, type `list`.

    ![List Packages](images/list-packages.png "List Packages")


4. Here's an example that demonstrates how to use the seaborn library package for visualization. Seaborn is a Python visualization library based on matplotlib. It provides a high-level interface for drawing attractive statistical graphics. This example

* Imports Pandas and seaborn
* Loads the Iris dataset
* Plots the datapoints, that is, the three different species of the Iris flower - Setosa, Virginica, and Versicolor based on its dimensions. It creates a scatter plot

Type:

  ```
  <copy>
    %python

    def sb_plot():
        import pandas as pd
        import seaborn as sb
        from matplotlib import pyplot as plt
        df = sb.load_dataset('iris')
        sb.set_style("ticks")
        sb.pairplot(df,hue = 'species',diag_kind = "kde",kind = "scatter",palette = "husl")
        plt.show()
  </copy>      
  ```
  ![Seaborn commands](images/seaborn-commands.png "Seaborn commands")

5. To run the function in a Python paragraph, type:

    ```
    <copy>
    %python
    sb_plot()
    </copy>
    ```

    ![Seaborn Visualization](images/seaborn-visualization.png "Seaborn Visualization")



## Task 4: Run Your Notebook using the R Interpreter

To run R functions in an Oracle Machine Learning notebook, you must first connect to the R interpreter.
An Oracle Machine Learning notebook supports multiple languages. For this, you must create a notebook with some paragraphs to run SQL queries, and other paragraphs to run R and Python scripts. To run a notebook in different scripting languages, you must first connect the notebook paragraphs with the respective interpreters such as R, Python, or SQL. This example shows how to:
  * Connect to the R interpreter to run R commands in a notebook.
  * Verify the connection to Oracle Autonomous Database, and
  * Load the ORE libraries

1. To connect to the R interpreter, type the following directive at the beginning of the notebook paragraph, and press Enter:

    ```
    <copy>
    %r
    </copy>
    ```

2. To verify the database connection, type the following command and press Enter:

    ```
    <copy>
    ore.is.connected()
    </copy>
    ```
  ![ore connected command](images/ore-connected.png "ore connected command")  

  Once your notebook is connected, the command returns TRUE, as shown in the screenshot here. The notebook is now connected to the R interpreter, and you are ready to run R commands in your notebook.

3. To import R Libraries, run the following commands:

    ```
    <copy>
    library(ORE)
    library(OREdplyr)
    </copy>
    ```

  ![load r packages](images/load-r-packages.png "load r packages")

  Once the packages are loaded successfully, the list of ORE packages are displayed as shown in the screenshot here. Scroll down to view the entire list.


## Task 5: Run Your Notebook using the Python Interpreter

Oracle Machine Learning for Python (OML4Py) enables you to run Python commands and scripts for data transformations and for statistical, machine learning, and graphical analysis on data accessible as tables and views.  

This example assumes that you have a notebook called Py Note notebook created. To run a Python script:

1. Open the Py Note notebook.

2. You must specify the Python interpreter to execute Python scripts in notebooks. Type %python and press enter. This specification, indicates that the paragraph should be executed by the Python interpreter.

   ![py connect interpreter](images/py-connect-interpreter.png "py connect interpreter")

3. To use OML4Py, you must first import the `oml` module. `oml` is the OML4Py module that allows you to manipulate Oracle Database objects such as tables and views, invoke user-defined Python functions using embedded execution, and use the database machine learning algorithms. Type the following commands and click the **Run** icon. Alternatively, you can press **Shift+Enter** keys to run the paragraph.   

    ```
    <copy>
    import oml
    oml.isconnected()
    </copy>
    ```


   ![connect py](images/connect-py.png "connect python")

   In this example, the commands:

     * `import oml`- Imports the OML4Py module
     * `oml.isconnected()` - Returns the following values:

        * TRUE - Indicates that the `oml` module is connected to the Oracle Database
        * FALSE - Indicates that the `oml` module is not connected to the Oracle Database

4. Once the `oml` module is connected to the Oracle Database, the command returns `TRUE`. On Oracle Autonomous Database, if the interpreter bindings are properly specified, this should always return `TRUE` as the database connection is established by the OML Notebook environment automatically. You are now ready to run python commands in your notebook.

   ![connect py true](images/connect-py-true.png "connect py true")

   Click the run icon. Alternatively, you can press **Shift+Enter** keys to run the notebook.

5. Type the following Python code and click the run icon.   

    ```
    <copy>
    import matplotlib.pyplot as plt
    import numpy as np

    list1 = np.random.rand(10)*2.1
    list2 = np.random.rand(10)*3.0

    plt.subplot(1,2,1) # 1 line, 2 rows, index nr 1 (first position in subplot)
    plt.hist(list1)
    plt.subplot(1, 2, 2) # 1 line, 2 rows, index nr 2 (second position in subplot)
    plt.hist(list2)
    plt.show()

    </copy>
    ```

    In this example, the commands import two python packages to compute and render the data in two histograms for list1 and list2. The Python packages are:

    * `Matplotlib` - Python package to render graphs.
    * `Numpy` - Python package for computations.

     ![python commands](images/py-commands.png "python commands")

6. The graphs for list1 and list 2 are generated by the python engine.     

     ![py commands histogram](images/py-commands-histogram.png "py commands histogram")  

7. Click **Back** to return to the Notebooks page.

## Task 6: Run Your Notebook using the SQL and PL/SQL Interpreter

To display and visualize data using SQL in a notebook paragraph, that data must be fetched from the database.

Paragraphs using the SQL `%sql` and PL/SQL `%script` interpreters allow users to call Oracle SQL and PL/SQL statements, respectively. You call the OML4SQL machine learning functionality in such paragraphs as well. The notebook offers the functionality to perform charting on the SQL interpreter output that is returned to the notebook. The options in the chart settings to perform groupings, summation, and other operations are done in the notebook server, and not in the database server. For example, if you want to run a `Group By` on all your data, then it is recommended to use SQL scripts to do the grouping in the database, and return the summary information for charting in the notebook. Grouping at the notebook level works well for small sets of data. If you pull a lot of data into the notebook, then you may encounter memory limitations. You can set the row limit for your notebook by using the option **Render Row Limit** to control how many rows are allowed to be returned from the database, with the default being the first 1000 rows in the Connection Group page.

To run a notebook:

1. Click the notebook that you want to run. The notebook opens in the Notebook editor.

2. Type the SQL statement to fetch data from an Oracle Database. For example, type `SELECT * from SH.SALES;` where `SH` is the schema name and `SALES` is the table name as shown in the screenshot.

   ![SH sales](images/sh-sales.png "Sales table in SH schema")

   Click the run icon. Alternatively, you can press **Shift+Enter** keys to run the notebook.

3. After you run the notebook, it fetches the data in the notebook in the next paragraph, as shown in the screenshot.

   ![sh sales data](images/sh-sales-data.png "Sales data")    

   The output section of the paragraph has a charting component that displays the results in graphical output. The chart interface allows you to interact with the output in the notebook paragraph. You have the option to run and edit single a paragraph or all paragraphs in a notebook. In this screenshot, you can see the data from the `SALES` table in a scatter plot.

   ![sh sales scatterplot](images/sh-sales-scatterplot.png "Sales table in a scatter plot")    

   You can visualize the data by clicking the respective icons for each graphical representation, as shown here:

      * ![histogram icon](images/histogram.png "histogram icon")Click the histogram icon to visualize your data in a histogram.
      * ![pie chart icon](images/pie-chart.png "pie chart icon")Click the pie chart icon to visualize your data in a pie chart.
      * ![cumulative gain chart icon](images/cumulative-gain-chart.png "cumulative gain chart icon")Click the cumulative gain chart icon to visualize your data in a cumulative gain chart.
      * ![line diagram icon](images/line-diagram.png "line diagram icon")Click the line diagram icon to visualize your data in a line diagram.
      * ![scatter plot icon](images/scatter-plot-icon.png "scatter plot icon")Click the scatter plot icon to visualize your data in a scatter plot.

4. Click **Back** to return to the Notebooks page.

## Task 7: Run your Notebook to generate static html from Markdown plain text

To call the Markdown interpreter and generate static html from Markdown plain text:


1. Type ``%md`` and press Enter.

2. Type ``"Hello World!"`` and click **Run**. The static html text is generated, as seen in the screenshot below.

	![Markdown tags for plain text](images/md-text.png "Markdown tags for plain text")

3. You can format the text in bold and italics. To display the text in bold, write the same text inside the tag **Hello World** and click Run.

	![Markdown tags for bold](images/md-bold.png "Markdown tags for bold")

4. To display the text in italics, write the same text inside an asterisk pair or underscore pair as shown in the screenshot, and click Run.

	![Markdown tags for italics](images/md-italics.png "Markdown tags for italics")

5. To display the text in a bulleted list, prefix * (asterisk) to the text, as shown in the screenshot below:

	![Markdown tags for bulleted points](images/md-bullets.png "Markdown tags for bulleted points")

6. To display the text in heading1, heading 2 and heading 2, prefix # (hash) to the text and click Run. For H1, H2, and H3, you must prefix 1, 2, and 3 hashes respectively.

	![Markdown tags for headings](images/md-heading-tags.png "Markdown tags for headings")


You may now **proceed to the next lab.**



## Acknowledgements

* **Author** : Mark Hornick, Sr. Director, Data Science / Machine Learning PM; Moitreyee Hazarika, Principal User Assistance Developer, Database User Assistance Development

* **Last Updated By/Date**: Moitreyee Hazarika, March 2024
