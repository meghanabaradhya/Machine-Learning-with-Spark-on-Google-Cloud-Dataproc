# Machine Learning with Spark on Google Cloud Dataproc

This project demonstrates how to use **Apache Spark** on **Google Cloud Dataproc** to perform scalable machine learning. By leveraging Spark's distributed computing capabilities, we process large datasets, train machine learning models, and deploy them efficiently.

## Overview
In this project, you will:
- Set up a Dataproc cluster on Google Cloud.
- Preprocess and analyze large datasets using Apache Spark.
- Train a machine learning model with Spark MLlib.
- Evaluate the trained model and explore deployment options.

## Prerequisites
Before starting, ensure you have:
1. A Google Cloud account with billing enabled.
2. Basic knowledge of Apache Spark and machine learning concepts.
3. The Google Cloud SDK installed and authenticated.
4. Access to a dataset for training and testing (e.g., from BigQuery or Cloud Storage).

## Setup
1. **Create a Dataproc Cluster**: Use the Google Cloud Console or `gcloud` CLI to set up a Dataproc cluster.
   ```bash
   gcloud dataproc clusters create my-cluster \
       --region=<region> \
       --zone=<zone> \
       --master-machine-type=n1-standard-4 \
       --worker-machine-type=n1-standard-4 \
       --num-workers=2
   ```

2. **Upload Data**: Place your training dataset in a Google Cloud Storage bucket.

3. **Prepare the Spark Job**: Write a Python or Scala script that uses Spark MLlib for data preprocessing and model training.

## Key Steps
1. **Data Ingestion**: Load the dataset from Cloud Storage or BigQuery into Spark.
   ```python
   from pyspark.sql import SparkSession

   spark = SparkSession.builder.appName("ML with Dataproc").getOrCreate()
   data = spark.read.csv("gs://<bucket-name>/<file-name>.csv", header=True, inferSchema=True)
   ```

2. **Data Preprocessing**: Use Spark transformations to clean and prepare the data.
   ```python
   from pyspark.ml.feature import VectorAssembler

   assembler = VectorAssembler(inputCols=["feature1", "feature2"], outputCol="features")
   prepared_data = assembler.transform(data)
   ```

3. **Model Training**: Train a model using Spark MLlib.
   ```python
   from pyspark.ml.classification import LogisticRegression

   lr = LogisticRegression(featuresCol="features", labelCol="label")
   model = lr.fit(prepared_data)
   ```

4. **Model Evaluation**: Test the model's accuracy using Spark's evaluation metrics.
   ```python
   from pyspark.ml.evaluation import BinaryClassificationEvaluator

   evaluator = BinaryClassificationEvaluator()
   accuracy = evaluator.evaluate(model.transform(prepared_data))
   print(f"Model Accuracy: {accuracy}")
   ```

## Deployment
Once the model is trained and evaluated, you can:
- Save it to Cloud Storage for future use:
  ```python
  model.save("gs://<bucket-name>/spark-model")
  ```
- Deploy it to a real-time prediction service like AI Platform or use it in batch jobs on Dataproc.

## Cleanup
To avoid unnecessary charges, delete the resources:
1. Delete the Dataproc cluster:
   ```bash
   gcloud dataproc clusters delete my-cluster --region=<region>
   ```
2. Remove unused Cloud Storage files.

## Resources
- [Google Cloud Dataproc Documentation](https://cloud.google.com/dataproc/docs)
- [Apache Spark MLlib](https://spark.apache.org/mllib/)
- [Google Cloud SDK](https://cloud.google.com/sdk)

## License
This project is licensed under the MIT License. See the LICENSE file for details.
