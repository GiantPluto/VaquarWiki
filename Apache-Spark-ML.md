* DataFrame: Spark ML uses DataFrame from Spark SQL as an ML dataset, which can hold a variety of data types. E.g., a DataFrame could have different columns storing text, feature vectors, true labels, and predictions.

* Transformer: A Transformer is an algorithm which can transform one DataFrame into another DataFrame. E.g., an ML model is a Transformer which transforms DataFrame with features into a DataFrame with predictions.A transformer consists of learned models and feature transformers. It is an abstraction and is created by adding one or more columns. For instance, a feature transformer can take a dataset, read one of its columns, convert it into a new one, add this new column to the original dataset, and then finally output the updated dataset. Technically, a transformer uses the transform() method to convert one DataFrame to another. 

* Estimator: An Estimator is an algorithm which can be fit on a DataFrame to produce a Transformer. E.g., a learning algorithm is an Estimator which trains on a DataFrame and produces a model.
Estimator works by abstracting a learning algorithm concept or any other algorithm concept that trains or fits on data. From the technical standpoint, an estimator uses the fit () method to accept a DataFrame and produce a transformer.For instance, the LogisticRegression learning algorithm is an estimator that calls the fit () method and trains a LogisticRegressionModel, which is a transformer.

* Pipeline: A Pipeline chains multiple Transformers and Estimators together to specify an ML workflow.
 Pipeline running an algorithms sequence for processing and learning from data is common in machine learning. 

* Parameter: All Transformers and Estimators now share a common API for specifying parameters.













https://dzone.com/articles/distingish-pop-music-from-heavy-metal-using-apache
