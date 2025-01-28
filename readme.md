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

To run the script, follow these steps:

1. Open a terminal or shell in your local environment.
2. Run the following script:
   ```bash
   export PROJECT_ID=$(gcloud info --format='value(config.project)')
   export BUCKET_NAME=$PROJECT_ID-traindays

## Setup
1. **Create a Dataproc Cluster**: Use the Google Cloud Console or `gcloud` CLI to set up a Dataproc cluster.
2. ```bash
   nano create_cluster.sh
   ```
   ***Copy the below content to create_cluster.sh***
   ```bash
   gcloud dataproc clusters create ch6cluster \
     --enable-component-gateway \
     --region ${REGION} \
     --master-machine-type n1-standard-4 \
     --master-boot-disk-size 500 --num-workers 2 \
     --worker-machine-type n1-standard-4 \
     --worker-boot-disk-size 500 \
     --optional-components JUPYTER --project $PROJECT \
     --initialization-actions=$INSTALL \
     --scopes https://www.googleapis.com/auth/cloud-platform \
     --public-ip-address
   ```

   ***Run this file by executing below command***
   ```bash
   ./create_cluster.sh $BUCKET_NAME  us-east4
   ```
   ## Troubleshooting

***Note***: If you encounter an error indicating that there are no available resources in the `us-east1-a` region, follow these steps:

   1. Open the `create_cluster.sh` script.
   2. Locate line 3, which contains the region setting:
      ```bash
      --region ${REGION}
      ```
   3. Modify it to specify a different zone within the us-east1 region
      ```bash
      --region ${REGION} --zone us-east1-b
      ```
   4. Save the changes and run the script again.


4. **Upload Data**: Place your training dataset in a Google Cloud Storage bucket.

5. **Prepare the Spark Job**: Look into [trainday_prediction_using_dataproc_spark.py](https://github.com/meghanabaradhya/Machine-Learning-with-Spark-on-Google-Cloud-Dataproc/blob/main/trainday_prediction_using_dataproc_spark.ipynb) to run the code


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
