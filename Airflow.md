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

SCRIPTS_DIR = "~/Amin-MAG/cobbler/airflow_scripts"

with DAG(
        dag_id = 'cobbler_data_update',
        schedule_interval = None,
        start_date = datetime(2002, 6, 25),
        catchup = False,
        tags = ['mapCore'],
        default_args = {'owner': 'amin-mag'},
        params = {'date': ''}
        ) as dag:
    pass

    # Run a workflow or run scenarios mannually
    startup = BashOperator(
            task_id="startup",
            bash_command= "echo Updating Data...",
            #ssh_hook=sshHook
        )

    minio_sync = BashOperator(
            task_id="minio-synchronization",
            bash_command=f"source {SCRIPTS_DIR}/minio_sync.sh "
        )

    merge_feedbacks = BashOperator(
            task_id="merge-feedbacks",
            bash_command=f"source {SCRIPTS_DIR}/merge_feedback.sh "
        )

    merge_geofabrik = BashOperator(
            task_id="merge-geofabrik-osc",
            bash_command=f"source {SCRIPTS_DIR}/merge_geofabrik_osc.sh "
        )

    duplicate_remove = BashOperator(
            task_id="duplicate-remove",
            bash_command=f"source {SCRIPTS_DIR}/duplicate_remove.sh "
        )

    cleansing = BashOperator(
            task_id="cleansing",
            bash_command=f"source {SCRIPTS_DIR}/cleansing.sh "
        )

    correctors = BashOperator(
            task_id="correctors",
            bash_command=f"source {SCRIPTS_DIR}/corrector.sh "
        )

    generate_osm_pbf = BashOperator(
            task_id="generate-osm-pbf-file",
            bash_command=f"source {SCRIPTS_DIR}/generate_osm_pbf.sh "
        )

    deliver_data_file = BashOperator(
            task_id="deliver-data-file",
            bash_command=f"source {SCRIPTS_DIR}/deliver.sh "
        )

startup >> correctors
```