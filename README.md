# airflow-etl-aws-openweatherapi

This Apache Airflow DAG performs Extract, Transform, and Load (ETL) operations on weather data for the city of Kathmandu. The data is obtained from a weather API and then transformed before being loaded into an AWS S3 bucket.

**DAG Structure:**

DAG Name: weather_dag
Schedule: The DAG is scheduled to run daily.
Start Date: December 31, 2023 (configurable in default_args).

Tasks:

is_weather_api_ready: HttpSensor
Verifies the availability of the weather API endpoint before proceeding with the extraction.
Uses the weathermap_api connection to check if the API is ready.

extract_weather_data: SimpleHttpOperator
Calls the weather API to extract current weather data for Kathmandu.
Uses the weathermap_api connection.
Performs a GET request to the specified API endpoint.
Applies a response filter to parse the JSON response.
Logs the API response.

transform_load_weather_data: PythonOperator
Executes a Python function (transform_load_data) to transform and load the extracted weather data.
Converts temperature values from Kelvin to Fahrenheit.
Extracts relevant weather information such as city, weather description, temperature, humidity, etc.
Creates a Pandas DataFrame from the transformed data.
Uploads the DataFrame as a CSV file to an AWS S3 bucket (weatherapiairflowtuple).
Uses AWS credentials (key, secret, and token) stored in the script.

ETL Logic (transform_load_data function):
Extracts data from the XCom variable produced by the extract_weather_data task.
Performs temperature unit conversion and extracts relevant weather information.
Creates a DataFrame from the transformed data.
Uploads the DataFrame as a CSV file to the specified S3 bucket.

DAG Configuration:
Default Args:
Owner: airflow
Email notifications on failure are configured.
Retries: 1 with a retry delay of 1 minute.

Notes:
This DAG assumes that the weather API connection (weathermap_api) is properly configured in the Airflow Connections UI.
The AWS S3 bucket and credentials used for data storage should be valid.
The DAG is set to run daily, starting from December 31, 2023.
Replace placeholder values (e.g., API key, S3 bucket name, and email address) with your actual credentials and configurations before deploying it.
