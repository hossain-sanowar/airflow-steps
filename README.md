# airflow-steps
This project is going to create airflow using Apache airflow 
# Steps:
1. Create folder
2. Basic command for check directory
```
ls
pwd
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
 export AIRFLOW_HOME=.
 airflow db init
 ```
 6. create airflow webserver
 ```
 airflow webserver -p 8082
 ```
