# Databricks

Spark is a fast execution engine and easy API to use, but to use Spark we have to 

- **Setup Clusters**
- **Manage Security**
- **Use Third-Party Products** to write Programs

At Its core, Databricks contains Apache [Spark](Spark.md), which is widely used in the industry for developing big data projects. For instance, Azure provides Azure Databricks which is the Databricks software on the Azure Cloud platform. Databricks has some level of management level to interact with Apache Spark.

- **Spin up Cluster and Install the software**
- **Workspace and Notebook**
- **Administration of Controls**
- **Optimized Spark**
- **Create Databases and Table**
- **Data Lake**
- **SQL Analytics**
- **MLFlow**

## Cluster

Databricks Cluster consist of several virtual machines (Driver and workers) for Workloads.

> Driver Node Distributes the tasks in parallel

The set of Workloads on these nodes are

- ETL for data Engineering
- Data Science
- Machine Learning

### Types of Cluster

- **All Purpose**
	- Created Manually
	- Persistent
	- Suitable for interactive workload
	- Shared among many users
	- Expensive to run
- **Job Cluster**
	- Created by Jobs
	- Terminated at the end of the job
	- Suitable for automated workloads
	- Isolated just for the job
	- Cheaper to run

### Access Mode

- **Single User**: Will only access only on user access that support Python, SQL, Scala, R
- **Shared**: Multiple User Access supporting Python, SQL
- **No Isolation Shared**: Multiple User Access supporting Python, SQL, Scala, R

### Runtime

Databricks Runtime is set of libraries that run on Databricks Clusters.

- **Databricks Runtime**: Spark, Scala, Java, Python, R, Ubuntu, GPU Libraries, Delta Lake
- **Databricks Runtime ML**: Databricks Runtime + PyTorch, Keras, TensorFlow
- **Photon Runtime**: Databricks Runtime + Photon Engine
- **Databricks Runtime Light**: For only jobs
# Azure Databricks Architecture

There are two pieces in the Azure Databricks.

## Control Plane

- **Cluster Manager**
- **Databricks UX**.
- **DBFS**

## Data Plane

- **Virtual Network** and **Network Security Group**
- **Azure Blob Storage**
- **Databricks Workspace**
