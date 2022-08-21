# Apache Airflow

An open-source platform for programmatically Authoring, Scheduling, and Monitoring workflows.

## Operators

Each operator is for running a single task in the workflow. The operator shouldn't share anything with each other. (They can share with XCom, but it's not recommended)

We have different kinds of operators.

## Basic Usage

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.operators.bash import BashOperator

from datetime import datetime

SCRIPTS_DIR = "~/Amin-MAG/project/airflow_scripts"

with DAG(
        dag_id = 'update',
        schedule_interval = None,
        start_date = datetime(2002, 6, 25),
        catchup = False,
        tags = ['test'],
        default_args = {'owner': 'amin-mag'},
        params = {'date': ''}
        ) as dag:
    pass

    # Run a workflow or run scenarios mannually
    a = BashOperator(
            task_id="a",
            bash_command= "echo Updating...",
        )

    b = BashOperator(
            task_id="minio-synchronization",
            bash_command=f"source {SCRIPTS_DIR}/minio_sync.sh "
        )


a >> b
```