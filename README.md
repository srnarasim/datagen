# datagen
Synthetically generate datasets using Deep Learning
This story will explore how to synthetically generate related relational datasets using Synthetic Data Vault(SDV). Python notebook for this article is available on Google Colab here.
Introduction
More often than not Data-Driven Enterprises these days find themselves in a pickle. While there is a huge demand to innovate and create new Data products, at the same time they have to restrict/govern access to production data for access by data scientists and engineers. Data scientists need realistic datasets for validating their models and test engineers need high-quality data for system testing to deliver high-quality products.
Libraries like Faker are extremely handy for generating simple datasets that can be super useful for Unit level testing, they are much too limiting for system testing a reasonably complex modern Enterprise application. Enterprise apps involve datasets from multiple bounded contexts with complex inter-relationships. Also, Enterprise data are often multi-modal in nature involving relational, time series, and document-based data sets. Data generation for the enterprise data products will need a different strategy altogether. This is where tools like SDV come to the rescue.
SDV is the fruit of the hard work and several years of research done at the MIT Data to AI lab under the supervision of Kalyan Veeramachaneni.
SDV is a collection of Python libraries for generating Synthetic Data based on deep learning models for different modalities (time-series, relational, and tabular ). Under the hood, SDV uses several probabilistic modeling and deep learning-based techniques to recursively iterate through the tables (data and metadata) and generate a Model. The generated model can then be used to generate new datasets synthetically.
SDV has several other algorithms for different modalities like relational, time-series, and tabular. It also provides tools for benchmarking the generated data. In this article, we will explore using SDV for generating data synthetically for a multi-table, relational model.
We will use the dataset of a Kaggle competition - Dunnhumby - The Complete Journey. Using this dataset, we will use SDV to train a multi-table model. From the trained model we will generate a sample dataset, evaluate, and compare the generated dataset against the source dataset.
SDV workflow involves the following steps which will follow here. The complete Python notebook is available on Google Colab here.
The SDV workflow (Source: The Synthetic data vault )Organize
This step involves downloading the Kaggle dataset and preparing it for our experiment. The Dataset contains household level transactions from a group of 2500 households about the purchases made along with their demographics information. For this experiment, we will just involve three tables - Product, Transaction, and Demographics table
Used Pandas to load the three tables into Dataframes, also removed columns that are not part of the experiment.
Specify Structure
The next step is to identify the structure of the tables and the relationship between the tables. The structure is captured in JSON format following the SDV metadata specification.
Looking at the data, we can see that the Product and Transaction table are related by the PRODUCT_ID, Demographics and the Transaction table are linked by the household_key. The data types of the other fields and other relationships are identified as shown here.
The Metadata library has a cool visualize functionality that visually shows the tables and the relationships.
Now that the source dataset and metadata are ready, on to the next step to train a model from the dataset.
Learn Model
For this step, we will utilize the Hierarchical Modeling Algorithm implemented in SDV known as HMA1.
This algorithm recursively walks through the relational dataset and applies tabular models across all the tables and trains a model based on how all the fields from all the tables are related.
Training API is pretty straight-forward to use.
Now that the model is trained, we can start creating synthetic data from the model which should be statistically closely aligned with the source dataset.
The model can be saved into a pickle file which is all that is needed to generate synthetic datasets. The model can be loaded back from the pickle file without the need for the original dataset.
Synthesize Data
Now that all the hard work has been done by SDV, training a Model from our source datasets, we can simply create a sample from the Model as easily as this to generate a sample dataset.
Evaluate Generated Data
We will use the TableEvaluator library to compare the generated data statistically to the source dataset that we started with and show how real our generated fake data is.
Here are some insights on the comparison.
The Product table has two categorical columns, the distribution comparison for those two columns is shown here.
The transaction table has three Numeric values: Sales, Transaction Time, and Quantity. While Sales, Transaction Time columns were similar between the original and synthetic datasets, the Quantity was a bit off.
Synthetic data generated on the Demographics table closely resembled the distribution on the source table.
Conclusion
With GDPR, CCPA, and other privacy laws locking up and protecting customer data, how can organizations innovate and become data-driven and achieve customer-centric digital transformation? Synthetic data tools and frameworks make it possible to generate highly representative data that closely resembles actual data.
SDV provides a wide range of tools for data engineers and data scientists to generate real-like fake data. The tools are still evolving and improving but there is already a good set of them that could be put to use.
