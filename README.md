# Gaia DR3 Nearest Neighbor Analysis in Microsoft Fabric
This repository contains the assets for a Microsoft Fabric project that ingests astronomical data from Gaia DR3, processes it, and serves on-demand nearest neighbor analysis through a Power BI report.

# Project Overview
The project consists of two primary data pipelines designed to create and utilize a model for finding the nearest stars to a given point in 3D space.

# Ingestion & Training Pipeline: 
This pipeline periodically fetches new or updated data from the Gaia DR3 dataset. The data is staged in a Lakehouse, which triggers a streaming process to load it into a clean Delta table. Finally, a K-Nearest Neighbors (KNN) model is updated or retrained with this new data to ensure it remains current.

# Inference & Reporting Pipeline: 
This pipeline is designed for on-demand analysis. It accepts Galactic coordinates (X, Y, Z in parsecs) as parameters, uses the pre-trained model to find the top 5 nearest neighbors, and pushes the results to a Power BI dataset, which automatically refreshes the user-facing report.

Architecture Flow
Pipeline 1: Data Ingestion and Model Refresh
[Gaia DR3 Source] -> [Fabric Pipeline: Ingest Data] -> [Lakehouse Staging Folder]
                                                            |
                                                            v
                                                  [Alert/Trigger]
                                                            |
                                                            v
                         [Notebook: Stream Read/Write to Gold Delta Table]
                                                            |
                                                            v
                              [Notebook: Update/Retrain Nearest Neighbor Model]

Pipeline 2: On-Demand Nearest Neighbor Query
[User Input (X,Y,Z)] -> [Fabric Pipeline: Query Neighbors] -> [Notebook: Load Model & Find Neighbors]
                                                                        |
                                                                        v
                                                         [Power BI Dataset (Results)]
                                                                        |
                                                                        v
                                                          [Power BI Report (Visuals)]



# Technologies Used
Microsoft Fabric:

Data Factory Pipelines: For orchestrating the workflow.

Lakehouse: For storing raw data (staging) and curated Delta tables (gold).

Notebooks: For all data transformation, model training, and inference logic.

Power BI: For data visualization and user interaction.

Python Libraries:

pyspark: For handling large-scale data in notebooks.

pandas: For data manipulation.

scikit-learn: For building and using the K-Nearest Neighbors model.

Data Source: Gaia DR3 (direct download).

How to Set Up and Run
Prerequisites
A Microsoft Fabric workspace with necessary permissions.

A Lakehouse created to store the staging data and final Delta Table.

Credentials for the Gaia DR3 data source configured securely in Fabric.

Deployment Steps
Clone Repository: git clone <repository_url>

Upload Assets to Fabric:

Upload the notebooks from the /notebooks directory into your Fabric workspace.

Recreate the pipelines in the Fabric Data Factory UI, using the uploaded notebooks as activities. You can use the exported .json files as a reference.

Upload the .pbix file to your Fabric workspace and connect it to the final Delta table or the results dataset.

Configure Pipelines:

Ingestion Pipeline: Configure the trigger (e.g., schedule or event on staging folder).

Inference Pipeline: Ensure it's configured to accept the x, y, and z parameters.
