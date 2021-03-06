# caps
Customer Acquisition Prioritization System (Recommendation System)

By: Liu Yue (modeling) & Zhu Yilin (demo)

In the perspective of a marketing consultingcompany, we recommend companies with potential new customers with high customer lifetime values. We developed our Customer Acquisition Prioritization System (CAPS) mainly have 2 sets of recommender algorithms - CF and popularity-based rules. More detailed results can be found in our report, presentation sides and the demo to promote CAPS our client.

To access the demo, please visit the public link:  https://plot.ly/dashboard/baojinjiji:113/view

As we use MongoDB to store the raw data file (about 2GB), you may need MongoDB.
As we use Plotly for visulization, you may need to register an account on plot.ly and need to have Jupyer Notebook to run the demo.pybn script.

In addition, to run all the python files, you need to install several Python libraries via:

	$ pip intall numpy pandas plotly matplotlib pymongo scikit-surprise
	
The installation of scikit-surprise requires Visual Studio, whose version depends on your Python version.

To allow for re-constructing the models and demo, we have the following parts in this readme file:
Part 1: Data Files

Part 2: Python Scripts

___________________________________________________________________________________
Part 1: Data Files

All datafiles are placed under the './1.Data' folder, which contains several subfolders.
All files titled 'summary_*' are the high level summarization of recomender results. Their respective source files contain actual HH_ID recommended.

In './1.Data/1.SourceData', we have the 3 source files that have been placed into MongoDB and 2 small descriptive files for key-value matching for the ProductArea and MajorCategory. 
	'DMEFLines3Dataset2.csv'	- line orders for each product in orders (DB)
	'DMEFOrders3Dataset2.csv'	- orders (DB)
	'4-DMEF3YrBase.csv'			- ZIP code & household id matching (DB)
	'area.csv'     				- product ProductArea code & descripton matching
	'caregory.csv' 				- product MajorCategory code & descripton matching

In './1.Data/2.DescriptiveAnalytics', we have the small output files from 'BT4221_G7_1_Descriptive_Statistics.ipybn' 
	'company_area_percentage.csv'		- percentage revenue for all companies by ProductArea
	'company_category_percentage.csv'	- ... by MajorCategory
    'company_sales_revenue_36.csv'		- revenue for Company 36 in 36 months in 10 areas
    'company_sales_hh_num_36.csv'		- cumulative customer base for ...
	
In './1.Data/3.CFInput', we have the medium-size (30-150MB) files from 'BT4221_G7_2_Recommeder_CF_Data_Preprocessing.ipybn', which will be required for Python Suprise library.
	'train_dol.txt'				- training data in the format of "CompanyID","HH_ID",score constructer
	'train_dol_log.txt'			- ...
	'train_num.txt'				- ...
	'train_num_log.txt'			- ...
	'scale_train_dol.txt'		- ...
	'scale_train_dol_log.txt'	- ...
	'scale_train_num.txt'		- ...
	'scale_train_num_log.txt'	- ...
	'test_dol.txt'				- testing data in the format of "CompanyID","HH_ID",score constructer
	'test_dol_log.txt'			- ...
	'test_num.txt'				- ...
	'test_num_log.txt'			- ...
	'scale_test_dol.txt'		- ...
	'scale_test_dol_log.txt'	- ...
	'scale_test_num.txt'		- ...
	'scale_test_num_log.txt'	- ...

In './1.Data/4.CFResults', we have some small- and medium-size output files from 'BT4221_G7_3_Recommeder_CF_Basic.ipybn'.
	'with_mean_cf_basics_cocluster_dol.csv'								- results for predicted ratings
	'with_mean_cf_basics_knn_dol.csv'									- ...
	'with_mean_cf_basics_svd_dol.csv'									- ...
	'actual_with_mean_cf_basics_cocluster_dol_log.csv'					- ...
	'actual_with_mean_cf_basics_cocluster_dol_logscale_.csv'			- ...
	'actual_with_mean_cf_basics_cocluster_dolscale_.csv'				- ...
	'actual_with_mean_cf_basics_knn_dol_log.csv'						- ...
	'actual_with_mean_cf_basics_knn_dol_logscale_.csv'					- ...
	'actual_with_mean_cf_basics_knn_dolscale_.csv'						- ...
	'actual_with_mean_cf_basics_svd_dol_log.csv'						- ...
	'actual_with_mean_cf_basics_svd_dol_logscale_.csv'					- ...
	'actual_with_mean_cf_basics_svd_dolscale_.csv'						- ...
	'summary_actual_with_mean_cf_basics_cocluster_dol_log.csv'			- summary as decile mean actual testing revenue ranked by predicted score
	'summary_actual_with_mean_cf_basics_cocluster_dol_logscale_.csv'	- ...
	'summary_actual_with_mean_cf_basics_cocluster_dolscale_.csv'		- ...
	'summary_actual_with_mean_cf_basics_knn_dol_log.csv'				- ...
	'summary_actual_with_mean_cf_basics_knn_dol_logscale_.csv'			- ...
	'summary_actual_with_mean_cf_basics_knn_dolscale_.csv'				- ...
	'summary_actual_with_mean_cf_basics_svd_dol_log.csv'				- ...
	'summary_actual_with_mean_cf_basics_svd_dol_logscale_.csv'			- ...
	'summary_actual_with_mean_cf_basics_svd_dolscale_.csv'				- ...
	'summary_with_mean_cf_basics_cocluster_dol.csv'						- ...
	'summary_with_mean_cf_basics_knn_dol.csv'							- ...
	'summary_with_mean_cf_basics_svd_dol.csv'							- ...	

In './1.Data/5.CFResultsFurther', we have some small- and medium-size output files from 'BT4221_G7_3_Recommeder_CF_Advanced.ipybn'.
    'cf_further_svd_dol.csv',									- results for predicted ratings (new svd)
    'cf_further_svd_dol_log.csv',								- ...	
    'cf_further_svd_num.csv',									- ...	
    'cf_further_svd_num_log.csv',								- ...	
    'summary_cf_further_svd_dol.csv',							- summary as decile mean actual testing revenue ranked by predicted score	
    'summary_cf_further_svd_dol_log.csv',						- ...	
    'summary_cf_further_svd_num.csv',							- ...	
    'summary_cf_further_svd_num_log.csv'						- ...	
    'knn_company_36_dol_log_sim_cosine.csv',					- results for predicted ratings (new knn)	
    'knn_company_36_dol_log_sim_msd.csv',						- ...	
    'knn_company_36_dol_log_sim_pearson_baseline.csv',			- ...	
    'knn_company_36_dol_sim_cosine.csv',						- ...	
    'knn_company_36_dol_sim_msd.csv',							- ...	
    'knn_company_36_dol_sim_pearson_baseline.csv',				- ...	
    'knn_company_36_num_log_sim_cosine.csv',					- ...
    'knn_company_36_num_log_sim_msd.csv',						- ...	
    'knn_company_36_num_log_sim_pearson.csv',					- ...	
    'knn_company_36_num_log_sim_pearson_baseline.csv',			- ...	
    'knn_company_36_num_sim_cosine.csv',						- ...	
    'knn_company_36_num_sim_msd.csv',							- ...	
    'knn_company_36_num_sim_pearson_baseline.csv'				- ...	
    'summary_knn_company_36_dol_log_sim_cosine.csv',			- summary as decile mean actual testing revenue ranked by predicted score	
    'summary_knn_company_36_dol_log_sim_msd.csv',				- ...	
    'summary_knn_company_36_dol_log_sim_pearson_baseline.csv',	- ...	
    'summary_knn_company_36_dol_sim_cosine.csv',				- ...	
    'summary_knn_company_36_dol_sim_msd.csv',					- ...	
    'summary_knn_company_36_dol_sim_pearson_baseline.csv',		- ...
    'summary_knn_company_36_num_log_sim_cosine.csv',			- ...	
    'summary_knn_company_36_num_log_sim_msd.csv',				- ...	
    'summary_knn_company_36_num_log_sim_pearson.csv',			- ...	
    'summary_knn_company_36_num_log_sim_pearson_baseline.csv',	- ...	
    'summary_knn_company_36_num_sim_cosine.csv',				- ...	
    'summary_knn_company_36_num_sim_msd.csv',					- ...	
    'summary_knn_company_36_num_sim_pearson_baseline.csv'    	- ...
	
In './1.Data/6.PopularityResults', we have the small- and medium-size output files from 'BT4221_G7_4_Recommder_Popularity_Based_Model.ipybn' 
	'popularity_company_36.csv'              		- customers recommended based on overall customer spending
	'popularity_company_36_area.csv'         		- ...based on weighted customer spending in the company's top 3 areas
	'popularity_company_36_category.csv'     		- ...based on weighted customer spending in the company's top 10 categoris
	'summary_popularity_company_36.csv'        		- summary: percentile-mean of actual revenue by recommended customers
	'summary_popularity_company_36_area.csv'  		- ...
	'summary_popularity_company_36_category.csv'	- ...
____________________________________________________________________________________
	
Part 2: Python Scripts
All Python scripts (".pynb") are placed under the './2.Model' folder and the './3.Demo' folder with respective html files for the ease of display.

In './2.Model', we have the following files:
	'BT4221_G7_1_Descriptive_Statistics.ipybn' 			- describing how to put the raw file into MongoDB, the genreal dataset, and nature of the company selected.
	'BT4221_G7_2_Recommeder_CF_Data_Preprocessing.ipybn'- obtaining "ratings" to be inputed into CF algorithms 
	'BT4221_G7_3_Recommeder_CF_Basic.ipybn' 			- CF recommenders and simple graphs, for 4 dol-related scores standard knn, svd, co-clustering
	'BT4221_G7_3.1_Recommeder_CF_Advanced.ipybn' 		- CF recommenders and simple graphs, for more scores/similarities for svd and knn
	'BT4221_G7_4_Recommder_Popularity_Based_Model.ipybn'- recommenders with popularity-based rules and simple graphs 

In './3.Demo', we have the following file:
	'BT4221_G7_4_Visualization.ipybn'	- describing how plotly figures are ploted, which are embeded in the demo dashboard  


	
	
