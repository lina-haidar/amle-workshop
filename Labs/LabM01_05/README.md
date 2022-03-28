# Azure Machine Learning Lab 05

## Prerequisites

An existing Azure Machine Learning workspace. Please refer to the [Lab 1](../LabM01_01/README.md) for guidance on how to create it if needed.

A datastore where to upload data. Please refer to the [Lab 3] for guidance on how to create it if needed

## Create and deploy a regression model using the Azure Machine Learning Studio Designer

The goal of this exercise is to deploy a regression model that will predict if a passanger airplane will have delayed arrival.
We will start creating using *Linear Regression* and then we will try a *Boosted Decion Tree Regression*. We will finally select the better performin model and we will deploy it as a web service

### Create a Linear Regression Model

#### Tasks

1. Sign in to the Azure Portal by using the credentials for your Azure Subscription
2. Search for **Machine Learning** in the search bar at the top of the page and select the corresponding service

    ![](img/select_machine_learning.png)

3. In the resulting click on the workspace you created and/or you want to use for the exercise (**aml-essentials-ws** in the example).

    ![](img/select_workspace.png)

4. In the Azure Machine Learning resource page click on **Launch studio**

    ![](img/aml_launch_studio.png)

5. In the Azure Machine Learning Studio Page click on **Designer** in the **Author** section of the left-side menu

    ![](img/select_pipeline_menu.png)

6. In the *Designer* session click on  **Easy-to-use prebuilt components**

    ![](img/built_new_pipeline.png)

7. In the automatically opened *Settings* window select the compute instance you created in [Lab 2](../LabM01_02/README.md)

    ![](img/select_compute_instance.png)

8. From the *Asset Library*  click on *Dataset* and drag and drop the two datasets **Airport Flight Delays**, **airport_codes** to the canvas

    ![](img/add_datasets.png)

9. From the **Asset Library** search *Join* 

    ![](img/search_join.png)

10. Drag and Drop the **Join Data** module to the canvas 
11. Connect the output of **Airport Flight Delays** to the left input of the **Join Data** module
12. Connect the output of **airport_codes** to the left input of the **Join Data** module

    ![](img/add_join.png)

13. Click the **Join Data** module to display the properties tab on the right

    ![](img/join_data_properties.png)

14.	Under **Join key columns for left dataset** click *Edit column* on the far right.
15.	In the window that opens, select By name and in the Available columns list find OriginAirportID and add it to the Selected columns list.

    ![](img/set_left_side.png)

16.	Click *Edit column* for Join key columns for right dataset.
17.	In the window that opens, select By name and add airport_id to the Selected columns list.

    ![](img/set_right_side.png)

18.	Set the remaining properties of the Join Data module as shown below:
    1. Match case: True
    2. Join type: Inner join
    3. Keep right key columns in joined table: False

    ![](img/join_data_final_settings.png)

19.	In the search box write  *remove duplicate rows*.

    ![](img/search_remove_duplicates.png)

20.	Drag and drop the **Remove Duplicate Rows** module to the canvas.
21.	Connect the output of the **Join Data** module to the input of the **Remove Duplicate Rows** module.

  ![](img/add_remove_duplicates.png)

22.	Click the **Remove Duplicate Rows** module to see its properties on the right side of the screen.
23.	For Key column selection filter expression field click Edit column.
24.	Add the following columns to the Selected columns list:
**Year**, **Month**, **DayofMonth**, **Carrier**, **OriginAirportID**, **DestAirportID**, **CRSDepTime**, **CRSArrTime**
*Note: In this case, two rows are considered duplicates of each other only if they have the same values in these eight columns.*

     ![](img/select_columns_deduplicate.png)

25.	In the search box write  *clean_missing_data*.

    ![](img/search_clean_missing_data.png)

26.	Drag and drop the **Clean Missing Data** module to the canvas.
27.	Connect the output of the **Remove Duplicate Rows** module to the input of the **Clean Missing Data** module.

  ![](img/add_clean_missing_data.png)

28.	Click the **Clean Missing Data** module to see its properties on the right side of the screen.

![](img/clean_missing_data_properties.png)

29. Click *Edit column* for **Columns to be cleaned** field

30. In the Columns to be cleaned dialog box, select With rules, and in the Include field select All columns.
    
    ![](img/clean_missing_data_all_columns.png)

31. Click **Save**

32. Set the *Cleaning mode* to **Remove entire row** and leave the remaining options at their default values.

     ![](img/clean_missing_data_remove_entire_Row.png)

33.	In the search box write  *select_columns*.

    ![](img/search_select_columns.png)

34.	Drag and drop the **Select Column in Dataset** module to the canvas.
35.	Connect the left output of the **Clean Missing Data** module to the input of the **Select Column in Dataset** module.

  ![](img/add_select_column.png)

36.	Click the **Select Column in Dataset** module to see its properties on the right side of the screen.

![](img/select_columns_properties.png)

37.  Click *Edit column* for **Select Columns** field

38.	In the Select columns window add the following columns:
**Month**, **DayofMonth**, **DayOfWeek**, **Carrier**, **OriginAirportID**, **DestAirportID**, **CRSDepTime**, **CRSArrTime**, **DepDelay**, **ArrDelay**

    ![](img/insert_selected_columns.png)
    
39.  Click **Save**
40.	In the search box write  *Normalize Data*.

    ![](img/search_normalize_data.png)

41.	Drag and drop the ***Normalize Data** module to the canvas.
42.	Connect the output of the **Select Column in Dataset** module to the input of the **Normalize Data** module.

  ![](img/add_normalzed_data.png)

43.	Click the **Normalize Data** module to see its properties on the right side of the screen.

![](img/normalize_data_properties.png)

44. Click *Edit column* for **Columns to transform** field
45.	In the Select columns window add the following columns:
**CRSDepTime**, **DepDelay**, **CRSArrTime**

    ![](img/normalized_data_columns.png)

46. Click **Save**
47.	Set the remaining properties of the **Normalize Data Module** as shown below:
    1. Transformation method: ZScore
    2. Use 0 for constant columns when checked: True

    ![](img/normalize_data_other_settings.png)



<!--
Edit Metadata
-->

48.	In the search box write  *Edit Metadata*.

    ![](img/search_edit_metadata.png)

49.	Drag and drop the ***Edit Metadata** module to the canvas.
50.	Connect the left output of the **Normalized Data** module to the input of the **Edit Metadata** module.

  ![](img/add_metadata.png)

51.	Click the **Edit Metadata** module to see its properties on the right side of the screen.

    +++++++++++++++++++++++++++++++![](img/edit_metadata_properties.png)

52.  Click *Edit column* for **Column** field
53.	In the Select columns window add the following columns:
**Carrier**, **OriginAirportID**, **DestAirportID**

    ![](img/columns_edit_metadata.png)

54.  Click **Save**
55.	Set the remaining properties of the **Edit Metadata** as shown below:
    1. Data type: String
    2. Categorial: Categorical
    3. Fields: Unchanged
    4. New column name leave blank

    ![](img/edit_metadata_other_settings.png)

<!--
Split Data
-->

56.	In the search box write *Split Data*.

    ![](img/search_split_data.png)

57.	Drag and drop the ***Split Data** module to the canvas.
58.	Connect the  output of the **Edit Metadata** module to the input of the **Split Data** module.

    ![](img/add_split_data.png)

59.	Click the **Split Data** module to see its properties on the right side of the screen.
60.	Set the properties of the **Split Data** as shown below:
    1. Fraction of rows n the first output dataset: 0.7
    2. Random seed: 567

    ![](img/split_data_settings.png)

<!--
Train Model
-->

61.	In the search box write *Train Model*.

    ![](img/search_train_model.png)

62.	Drag and drop the ***Split Data** module to the canvas.
63.	Connect the  left output of the **Split Data** module to the right input of the **Train Model** module.

    ![](img/add_train_model.png)

64.	Click the **Train Model** module to see its properties on the right side of the screen.
65. Click *Edit column* for **Label column** field
66.	Select the column **ArrDelay**

    ![](img/select_train_model_column.png)

67. Click **Save**

<!-- 
Linear Regression
-->

68.  In the search box write *Linear Regression*.

    ![](img/search_linear_regression.png)

69.	Drag and drop the ***Linear Regression** module to the canvas.

    ![](img/add_linear_regression.png)

70. Connect the  output of the **Linear Regression** module to the left input of the **Train Model** module.
71.	Click the **Linear Regression** module to see its properties on the right side of the screen.

72.	Set the properties of the **Linear Regression** as shown below:
    1. Solution Method: **Ordinary Least Squares**
    2. L2 Regularization weight: **0.001**
    3. Include intercept term: **True**
    4. Random number seed: **567**

    ![](img/linear_regression_settings.png)

<!--
Score Model
-->

73. In the search box write *Score Model*.

    ![](img/search_score_model.png)  

74.	Drag and drop the ***Score Model** module to the canvas.
75. Connect the output of **Train Model** module to the left input of the **Score Model** module and the right output of the **Split Data** module to right input of the **Score Mode**

    ![](img/add_score_model.png)
<!--
Evaluate Model
-->

76. In the search box write *Evaluate Model*.

    ![](img/search_evaluate_model.png)  

77.	Drag and drop the ***Evaluate Model** module to the canvas.
78. Connect the output of **Score Model** module to the left input of the **Evaluate Model** 

    ![](img/add_evaluate_model.png)
  
79. Click on the **Submit** button on the top right of the interface
80. Set the properties of the **Pipeline Run** as shown below:
    1. Experiment: Create new
    2. New Experiment name: AirflightDelays
    3. Job Description: Example Regression for AML Workshop

    ![](img/set_pipeline_run.png)

81. Click **Submit**

82. Click on the **Evaluate Model** with the right button and select *Preview Data* -> *Evaluatiaon Results*

    ![](img/show_preview_data.png)

### Create a Boosted Decision Tree Regression Model

In this lab we are going to create a Boosted Decision Tree Regression Model and compare the Results with the Linear Regression model we built in the first step

#### Tasks

1. Double click on the **Train module** created during the previous Lab and type **Linear Regression Trained** in the comment box

    ![](img/comment_train_model_1.png)

2. Double click on the **Score Model** created during the previous Lab and type **Linear Regression Score** in the comment box

    ![](img/comment_score_model_1.png)

3. Copy and paste existing Train Model module and the Score Model module onto the canvas.  To do this, right-click the existing Train Model module, select Copy, then right-click on an open spot on the canvas and select Paste. Then click on the pasted module and move it to the desired location

4. Change the comments to **Boosted Decision Tree Regression Trained** and **Boosted Decision Tree Regression Score** respectively.

5. Connect the left input of **Split Data** module to the right input of the **Boosted Decision Tree Train Model** module

6. Connect the output of the **Boosted Decision Tree Train Model** module to the left input of the **Boosted Decision Tree Score Model** module

7. Connect the right output of **Split Data** module to right input of the  **Boosted Decision Tree Train Model** module

8. Connect the output of the **Boosted Decision Tree Score Model** module o the left input of the **Evaluate Model** module

   ![](img/add_boosted_train_and_score.png)

9. In the search box write *Boosted Decision Tree Regression *.

   ![](img/search_boosted_decision_tree_regression.png)  

10.	Drag and drop the ***Boosted Decision Tree Regression** module to the canvas.
11. Connect the output of **Boosted Decision Tree Regression** module to the left input of the **Boosted Decision Tree Regression Trained** 
12.	Click the ***Boosted Decision Tree Regression** module to see its properties on the right side of the screen.
13.	Set the properties of the ***Boosted Decision Tree Regression** as shown below:
    1. Create trainer mode: **SingleParameter**
    2. Maximum number of leaves per tree: **20**
    3. Minimum number of samples per leaf node: **10**
    4. Learning Rate: **0.2**
    5. Total number of trees constructed: **100**
    6. Random number seed: **567**

    ![](img/boosted_decision_tree_regression_settings.png)  
    
14. Click on **Submit**. Leave **Select Existing** experiment option checked. Wait for the run to complete

15. Click on the **Evaluate Model** with the right button and select *Preview Data* -> *Evaluatiaon Results*

    ![](img/show_preview_data.png)

16. From the comparison we can see that the new model is slighly better of the previous one

    ![](img/result_comparison.png)


17. Delete the **Linear Regression** module
18. Delete the **Linear Regression Train Model** module 
19. Delete the **Linear Regression Score Model** module 
20. Connect the output of the **Boosted Decision Tree Score Model* module to the left input of the *Evaluate Model* module

    ![](img/final_pipeline.png)

### Deploy the model as Web Service

#### Tasks

1. Click **Create Inference Pipeline** button in the upper right area of the screen and select **Real-time Inference Pipeline**
   
    ![](img/create_real_time_inference.png)

   After setting up the web service you will see another tab **Real-time Inference Pipeline**

    ![](img/real_time_pipeline.png)

2. Remove the left **Web service input**
3. Remove the connection of the right **Web Service Input** to the **Join Data** Module
4. Connect the **Web Service Input** to the **Score Model** module

    ![](img/moved_web_service_input.png)

5. Remove the connection between the **Edit Metadata** module and the **Score Model** module

    ![](img/remove_connection_to_score_model.png)

6. Search and drag a **Select Columns in Dataset** module onto the canvas
7. Connect the output of the **Edit Metadata** to the input of **Selected Columns in Dataset**
8. Connect the output of the **Select Columns in Dataset** module to the right input of the **Score Model** model

     ![](img/insert_selected_columns_dataset.png)

9.	Click the **Select Column in Dataset** module to see its properties on the right side of the screen.

    ![](img/select_columns_properties.png)

10.  Click *Edit column* for **Select Columns** field

11.	In the Select columns window add the following columns:
**Month**, **DayofMonth**, **DayOfWeek**, **Carrier**, **OriginAirportID**, **DestAirportID**, **CRSDepTime**, **CRSArrTime**, **DepDelay**

    ![](img/select_columns_for_inference.png)

12. Remove the connection between the **Score Model** module and the **Web service output**

    ![](img/remove_score_to_weboutput.png)

13. Search and drag a **Select Columns in Dataset** module onto the canvas
14. Connect the output of the **Score Model** to the input of **Selected Columns in Dataset**
15. Connect the output of the **Selected Columns in Dataset** to the input of **Web service output**

    [](img/reconnect_weboutput.png)

16.	Click the **Select Column in Dataset** module to see its properties on the right side of the screen.

    ![](img/select_columns_properties.png)

17. Click *Edit column* for **Select Columns** field
18.	In the Select columns window add the following columns: **Scored Labels**
19. Remove the **Evalute model** module
20. Click on **Submit** in the upper right of the screen
21. Set the properties of the **Pipeline Run** as shown below:
    1. Experiment: Create new
    2. New Experiment name: RegressionInferenceWebService
    
    ![](img/inference_pipeline_settings.png)

22. When complete, click **Deploy** in the upper right area of the Screen
    
    ![](img/click_deploy_details.png)

23. Enter a name for the deployment, for example, **delayregression-deployment**. In the *Compute type field*, select **Azure Container Instance.**

    ![](img/real_time_endpoint_settings.png)

24. Click **Deploy**

The lab is complete!