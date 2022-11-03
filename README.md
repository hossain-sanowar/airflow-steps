# airflow-steps
This project is going to create airflow using Apache airflow 
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
 5.1 alternative way
 ```
 mkdir airflow_new
 AIRFLOW_TUTORIAL={pwd}
 AIRFLOW_TUTORIAL=/Users/mdsanowarhossain/Documents/Project_Pro/airflow/airflow_tutorial/airflow_new
 sudo chmod -R 777 ../airflow_new
 airflow db init
 ```
 6. create airflow webserver
 ```
 airflow webserver -p 8080
 airflow standalone
 ```
