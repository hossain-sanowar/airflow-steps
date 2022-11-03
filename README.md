# airflow-steps
I am going to introduce Apache Airflow and describe how to install airflow locally in a Python environment so that airflow may operate on your local machine.

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
