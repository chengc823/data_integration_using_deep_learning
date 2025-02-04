
Data Integration using Deep Learning
==============================

This github-repository provides the code for our team project in the fall semester 2021 at the University of Mannheim with the topic of Data Integration using Deep Learning. We are investigating the performance of table transformer frameworks on the tasks of matching entities (entity matching) and schemes (schema matching) across tables. The task will be presented as a multi-class classification problem of characterising if a row (entity) or a column (schema) belongs to a predefined cluster/has a specific label. Table transformer models use whole table representations as input to enrich the classification problem with metadata and surrounding information. We investigate whether these enriched models, in particular TURL and Tabbie, will perform better than our two baselines: RandomForest based on tf-idf and word count representations and RoBERTa/TinyBert as embedding based transformer models. The project is based on data from the `WDC Schema.org Table Corpus <http://webdatacommons.org/structureddata/schemaorgtables/>`__ with special focus on two of the largest categories product and localbusiness for the entity matching task and with predefined restrictions for the schema matching task. The schema matching task entails the determination of > 200 column types such as name, title, duration etc. from categories such as Music Recoding or Recipe.

We make most of our code availabe that was used for this project. Since the repositories for the different algorithms are too huge for github on top of our code, they can be requested by contacting the authors.

Table of Contents
==============================
.. contents::

Description Entity - Product
==============================

In the following, we will shortly describe the project setup and our approach. For the product data, the corpus already provided a clustering that we could make use of as labels. By first cleaning the corpus data via a tld-based approach (filter on english internet domain endings) and using fast-text we made sure to focus on tables that are mainly english. Via keyword search on brands with the strongest revenue in different product categories, we created our dataset from the corpus and made sure that enough entities are included that are hard to distinguish (Doc2Vec and Jaccard similarity). Using a multilabel stratified shuffle approach we split our remaining data into train-validation-test sets by using 3/8 for training, 2/8 for validation and 3/8 for testing. After a manual check of the test data and then discarding noise (~10%), we remain with 1,345 train, 855 validation and 1,331 test tables. As baselines we use on the one hand a tf-idf and tf based random forest algorithm and one the other hand two bert based approaches with TinyBert and RoBERTa. The results can be seen below.
As we are trying to beat those baselines with the mentioned table transformers, we will provide in the following the results of TURL, tabbie and constrative learning for comparison with best settings respectively.


Results: 

* Random Forest: 0.8684 F1  
* TinyBert: 0.8329 F1
* RoBERTa: 0.8958 F1
* TURL: 0.7556 F1
* Tabbie: -
* Contrastive Learning: 0.9379 F1


You can find the code for each part in the following table: 

*  `Data set generation <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/Product/Preprocessing>`__
*  `Train test split <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/Product/train_test_split>`__
*  `Baselines <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/Product/Baseline>`__
*  `TURL Experiments <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/Product/TURL>`__
*  `Tabbie Experiments <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/Product/tabbie>`__
*  `Visualizations <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/visualizations>`__

All Experiments done were written in Jupyter Notebooks, which can be found in this  `folder <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity>`__.

All of our datasets can be requested by mail at ralph@informatik.uni-mannheim.de, but are available for research purposes only.


Description Entity - LocalBusiness
==============================

We used the same approach as above for preparing a dataset for LocalBusiness. Unfortunately the baselines were already in the range of 95%-99% F1. So, we discarded the dataset as it did not include difficult enough examples to evaluate the approaches.


You can find the code for each part in the following table: 

*  `Data set generation <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/LocalBusiness/Preprocessing>`__
*  `Train test split <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/LocalBusiness/train_test_split>`__
*  `Baselines <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity/LocalBusiness/Baseline>`__


All Experiments done were written in Jupyter Notebooks, which can be found in this  `folder <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Entity>`__.

Description Schema
==============================

We make all our code availabe that was used for this project. It contains the data preprocessing for all use cases, the baseline generation, experiments for both TURL and Tabbie and the consequent error analysis. At first, the data from the named web page was downloaded and explored. Hereby, statistics and technical characteristics of the tables were recorded to choose a subsample of the data. Also, similar to the Entity task, only English data is considered by filtering out non-English data. By comparing the frequency and density of column labels such as name, duration, price etc. about 200 labels are selected as the database from about 13 categories such as Book, Creative Work etc. The biggest categories, i.e. comprising the most tables, were chosen and respective tables were selected based on the number of relevant selected columns, a minium amount of rows and a maximum amount of NAs that they comprise of. Hereby, large, midsize and small tables are considered. Additionally, it was important to keep a variety of datatypes including string, date, integer, float and geolocation. Hereby, the inclusion of hard cases is possible. A hard case would be for example different types of gtin numbers of a product or best rating vs worst rating vs average rating. Furthermore, while not possible to represent all categories evenly distributed, every category has enough representatives to be trained and tested on. Overall, there are three training sets and one test built. The large training set contains 44,345 tables, the mid-size training set contains 9,776 training set and the small training set contains 2,444 tables. Hereby the small set is included in the mid and large and the mid-size set is contained in the large while the proportion of columns was held equal. The test set contains 8,912 tables. 

Both bert-based models and regular models serve as a baseline for the final table transformer models. To prepare the data for the bert-based models, the entries of the selected target columns are concatenated. Hereby, the context and the structure within the data is lost and not fully comparable with the to be tested table transformer models.

The final tests are done on the models TURL and Tabbie.


Results: 

* Random Forest: 0.35 F1
* TinyBert: 0.76 F1
* Bert: 0.8 F1
* Distilbert: 0.8 F1
* RoBERTa: 0.8 F1
* TURL: 0.86 F1
* Tabbie: - 
* Contrastive Learning: 0.76 F1

You can find the code for each part in the following table: 

*  `Data set generation <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Schema/Preprocessing>`__
*  `Train test split <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Schema/Train_Test_Split>`__
*  `Baselines <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Schema/Baseline>`__
*  `TURL Experiments <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Schema/TURL>`__
*  `Tabbie Experiments <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Schema/tabbie>`__
*  `Visualizations <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/visualizations>`__


All Experiments done were written in Jupyter Notebooks, which can be found in this  `folder <https://github.com/NiklasSabel/data_integration_using_deep_learning/tree/main/notebooks/Schema>`__.



How to Install
==============================

To use this code you have to follow these steps:

1. Start by cloning this Git repository:

.. code-block::

    $  git clone https://github.com/NiklasSabel/data_integration_using_deep_learning.git
    $  cd data_integration_using_deep_learning

2. Continue by creating a new conda environment (Python 3.8):

.. code-block::

    $  conda create -n data_integration_using_deep_learning python=3.8
    $  conda activate data_integration_using_deep_learning

3. Install the dependencies:

.. code-block::

    $ pip install -r requirements.txt
    
4. For running TURL please use the provided turl.yml file.

Credits
==============================

The project started in October 2021 as a team project at the University of Mannheim and ended in March 2022. The project team consists of:

* `Cheng Chen <https://github.com/chengc823>`__
* `Jennifer Hahn <https://github.com/JenniferHahn>`__
* `Kim-Carolin Lindner <https://github.com/kimlindner>`__
* `Jannik Reißfelder <https://github.com/jannik-reissfelder>`__
* `Marvin Rösel <https://github.com/maroesel>`__
* `Niklas Sabel <https://github.com/NiklasSabel/>`__
* `Luisa Theobald <https://github.com/LuThe17>`__
* `Estelle Weinstock <https://github.com/estelleweinstock>`__

Feel free to raise an issue in this github repository if you have questions to the project team.


License
==============================

This repository is licenced under the MIT License. If you have any enquiries concerning the use of our code, do not hesitate to contact us.

Project based on the  `cookiecutter data science project template <https://drivendata.github.io/cookiecutter-data-science/>`__ #cookiecutterdatascience

`TURL repository <https://github.com/sunlab-osu/TURL>`__

`Tabbie repository <https://github.com/SFIG611/tabbie>`__

