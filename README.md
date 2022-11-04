# airflow-steps
I am going to introduce Apache Airflow and describe how to install airflow locally in a Python environment so that airflow may operate on your local machine.

https://airflow.apache.org/docs/apache-airflow/stable/installation/installing-from-pypi.html

 - Introduction
- Create the Airflow project folder
- Create a local python environment
- Install Airflow and its dependencies via pip
- Export Airflow Home and init the DB
- Launch the airflow webserver
- Create a username and password
- Start the airflow scheduler 

# Steps:
1. Create folder
2. Basic command for check directory
```
ls
pwd
path= /Users/mdsanowarhossain/Documents/Project_Pro/airflow/airflow_tutorial
python3 --version
```
3. create environment
```
python3 -m venv py_env
source py_env/bin/activate
```
4. install apache-airflow
https://github.com/apache/airflow#installing-from-pypi
```
pip install 'apache-airflow==2.4.2' \
 --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.4.2/constraints-3.10.txt"
 ```
 5. install sqlight database
 ```
 #create path
 #this is for linux
 export AIRFLOW_HOME=.
 #mac user
 export AIRFLOW_HOME=path
 export AIRFLOW_HOME=/Users/mdsanowarhossain/Documents/Project_Pro/airflow/airflow_tutorial
 ```
 5.1 another option
 ```
 mkdir airflow_new
 AIRFLOW_TUTORIAL={pwd}
 AIRFLOW_TUTORIAL=/Users/mdsanowarhossain/Documents/Project_Pro/airflow/airflow_tutorial/airflow_new
 sudo chmod -R 777 ../airflow_new
 airflow db init
 ```
 6. create airflow webserver
 ```
  airflow db init #create log folder, sqlite database, some configuration file
 airflow webserver -p 8080
 or 
 airflow standalone
 ```
7. create user name and password
```
airflow users create --help
airflow users create \
          --username admin \
          --firstname FIRST_NAME \
          --lastname LAST_NAME \
          --role Admin \
          --email admin@example.org
```
```
airflow users create --username admin --firstname firstname --lastname lastname --role Admin --email admin@domain.com
```
8. Open new terminal for creating schedular
```
export AIRFLOW_HOME=/Users/mdsanowarhossain/Documents/Project_Pro/airflow/airflow_tutorial
source py_env/bin/activate
airflow scheduler
```
#------------------------------------------------------------------------------------------------------------------------------
#Docker

# Running Airflow 2.0 in Docker

https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#running-airflow-in-docker

Today I will demonstrate how to install Apache Airflow 2.0 on Docker step by step.

👉 What is Docker and Docker Compose
👉 How to run Airflow in Docker

- What is Docker
- Install Docker and Docker Compose
- Create a local python environment
- Download and customize the docker-compose.yaml file
- Initialize the Airflow DB
- Launch the Airflow in Docker
#1. check current path
```
ls
pwd
path
```
2. check docker status
```
 docker --version
 docker-compose --version
 ```
3. install docker-compose.yaml
```
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.4.2/docker-compose.yaml'
```
4. remove some unnecessary file
5. create dags
```
mkdir -p ./dags ./logs ./plugins
```
6. Initialize the database
```
docker-compose up airflow-init
```
7. run docker compose 
```
docker-compose up
```
8. check docker status
```
docker ps
```
9. user name and password

# BashOperator
#========================================BashOperator======================================
I will demonstrate how to build our first dag by use the bash operator
👉 How to create a dag and set parameters as your need
👉 Define tasks using BashOperator
👉 How to build the task dependencies

- Launch airflow using docker
- Remove all the airflow example dags
- Create our first dag and set the parameters
- Create dag tasks using bash operator
- Trigger our first dag run 
- Define multiple tasks for the dag
- Three different ways to build tasks dependencies

1. Remove all airflow 
```
docker-compose down -v
```
2. change parameter and create new dag; reload the page 
```
AIRFLOW__CORE__LOAD_EXAMPLES: 'false'
docker-compose up airflow-init
docker-compose up -d 
```
3. bashOperator
```
from datetime import datetime, timedelta

from airflow import DAG
from airflow.operators.bash import BashOperator

default_args={
    'owner':'Sanowar',
    'retries':5,
    'retry_delay': timedelta(minutes=2)
}

with DAG(
    dag_id='our_first_dag_v5',
    default_args=default_args,
    description='This is our first dag that we write',
    start_date=datetime(2022, 11,2,2),
    schedule_interval='@daily'
) as dag:
    task1=BashOperator(
        task_id='first_task',
        bash_command='echo hello world, this is the first task!'
    )
    task2=BashOperator(
        task_id='second_task',
        bash_command="echo hey, I am task2 and will be running after task1"
    )
    task3=BashOperator(
        task_id='third_task',
        bash_command="echo hey, I am task3 and will be running after task2"
    )
    #task dependency method 1
    # task1.set_downstream(task2)
    # task1.set_downstream(task3)

    #task dependency method 2
    # task1>>task2
    # task1>>task3

    #task dependency method 3
    task1>>[task2,task3]
    ```
 # Airflow Python Operator and XCom
#===================================Python xcon====================
https://airflow.apache.org/docs/apache-airflow/stable/index.html

I am going to show you how to use Airflow PythonOperator and XComs.
👉 How to run a python function as a task using the python operator
👉 How to pass parameters to the python function 
👉 How to share values between different tasks using Xcoms

- Introduction and launch airflow using docker
- Create a new DAG file
- Using the python operator to run a python function
- Pass parameters using op_kwargs
- How XCom works
- Push and pull values using XCom
- Push multiple values using key and value
- Only share small values via XCom

```
from airflow import DAG
from datetime import datetime, timedelta
from airflow.operators.python import PythonOperator

default_args={
    'owner':'Sanowar',
    'retries':5,
    'retry_delay':timedelta(minutes=5)
}

def greet(ti):
    first_name=ti.xcom_pull(task_ids='get_name', key='first_name')
    last_name=ti.xcom_pull(task_ids='get_name', key='last_name')
    age=ti.xcom_pull(task_ids='get_age',key='age')
    print(f"Hello World! My name is {first_name}{last_name},"
          f"and I am {age} years old!")

def get_name(ti):
    ti.xcom_push(key='first_name',value='Jerry')
    ti.xcom_push(key='last_name',value='Fridman')

def get_age(ti):
    ti.xcom_push(key='age',value=19)

with DAG(
default_args=default_args,
dag_id='our_dag_with_python_operator_v05',
description='our first dag using python operator',
start_date=datetime(2022, 11,2),
schedule_interval='@daily'
) as dag:
    task1=PythonOperator(
        task_id='greet',
        python_callable=greet
        # op_kwargs={'age':30}
        )
    task2 =PythonOperator(
        task_id='get_name',
        python_callable=get_name
    )
    task3 =PythonOperator(
        task_id='get_age',
        python_callable=get_age
    )
    [task2, task3] >> task1
```
