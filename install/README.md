# Installation

## Standalone

At the time of writing, the latest Python version supported by the Airflow's dependencies
[constraints](https://github.com/apache/airflow/tree/constraints-2.4.0) is 3.10.

See also [Airflow introduction and installation: Airflow Tutorial P1](https://youtu.be/z7xyNOF8tak?si=wNImX3Xn8Yu_ptx5)
for step-by-step instructions.

Execute:
```shell
chmod +x install/install_airflow_standalone.sh
```

### Initialization

1. Choose an AIRFLOW home directory, by default set to ```~/airflow```
```shell
export AIRFLOW_HOME=/Users/bertrandrigaldies/Projects/airflow-tutorial/local_airflow/
```
ATTENTION: Do not use "." for ```AIRFLOW_HOME```, as at the time of writing with AirFlow 2.8.3, this caused the following error:
```shell
-> % airflow db init
Traceback (most recent call last):
  File "/Users/bertrandrigaldies/Projects/airflow-tutorial/venv/bin/airflow", line 5, in <module>
    from airflow.__main__ import main
  File "/Users/bertrandrigaldies/Projects/airflow-tutorial/venv/lib/python3.10/site-packages/airflow/__init__.py", line 68, in <module>
    settings.initialize()
  File "/Users/bertrandrigaldies/Projects/airflow-tutorial/venv/lib/python3.10/site-packages/airflow/settings.py", line 559, in initialize
    configure_orm()
  File "/Users/bertrandrigaldies/Projects/airflow-tutorial/venv/lib/python3.10/site-packages/airflow/settings.py", line 237, in configure_orm
    raise AirflowConfigException(
airflow.exceptions.AirflowConfigException: Cannot use relative path: `sqlite:///./airflow.db` to connect to sqlite. Please use absolute path such as `sqlite:////tmp/airflow.db`.
```
2. Initialize the Airflow database
Execute the following command:
```shell
airflow db init
```

3. Create an admin user
Execute the following command to create a user (the command prompts you to enter a password):
```shell
airflow users create \
          --username admin \
          --firstname FIRST_NAME \
          --lastname LAST_NAME \
          --role Admin \
          --email admin@example.org
```

### Run The AirFlow Web Server

Execute the following command to start the AirFlow Web Server (select an unused port number):
```shell
airflow webserver -p 8080
```

### Run the AirFlow Scheduler
Execute the following command to run the AirFlow scheduler:
```shell
airflow scheduler
```

## Docker-based

### Docker Compose

See also the AirFlow Documentation on [Running Airflow in Docker](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#running-airflow-in-docker)

Fetch the AirFlow's docker-compose file by executing the following commamnd:
```shell
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.8.3/docker-compose.yaml'
```

### Initilization

1. The following commands are necessary on Linux. For other operating systems, the documentation notes that the commands
can be run too in order to eliminate a warning:

_For other operating systems, you may get a warning that AIRFLOW_UID is not set, but you can safely ignore it. 
You can also manually create an .env file in the same folder as docker-compose.yaml with this content to get rid of the warning_.

```shell
mkdir -p ./dags ./logs ./plugins ./config
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

2. AirFlow Database Initialization
```shell
docker compose up airflow-init
```

A successful database initialization will end with the following message:
```shell
airflow-init_1       | Upgrades done
airflow-init_1       | Admin user airflow created
airflow-init_1       | 2.8.3
start_airflow-init_1 exited with code 0
```

3. Start AirFlow
```shell
docker compose up
```