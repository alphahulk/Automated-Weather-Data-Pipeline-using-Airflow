# Automated-Weather-Data-Pipeline-using-Airflow

Checking Weather API Availability:

We start by ensuring that the weather API we're using is available. This is important because if the API isn't ready to respond, attempting to fetch data from it would be futile. So, we have a task called is_weather_api_ready that uses an HttpSensor to make a lightweight HTTP request to the weather API's endpoint specifically designed to check its availability.
Extracting Weather Data:

Once we confirm that the weather API is available, we proceed to fetch the weather data for Portland. We have a task named extract_weather_data that employs a SimpleHttpOperator to make an HTTP GET request to the API's endpoint dedicated to retrieving weather data for Portland. This task retrieves the raw weather data in JSON format.
Transforming and Loading Weather Data:

After fetching the raw weather data, we need to process it into a structured format that's suitable for analysis and storage. We achieve this through the transform_load_weather_data task, which is a PythonOperator. This task executes a Python function called transform_load_data. Inside this function, we extract relevant information from the raw weather data, such as temperature, pressure, humidity, and wind speed. We then convert temperature values from Kelvin to Fahrenheit and format the data into a tabular structure using Pandas DataFrame. Finally, we upload this transformed data into a CSV file stored in an Amazon S3 bucket. We include a timestamp in the filename to indicate when the data was recorded.
Schedule and Error Handling:

This DAG is scheduled to run daily (schedule_interval = '@daily') and starts from January 8, 2023 (start_date=datetime(2023, 1, 8)). We have configured email notifications for failures, but they are currently disabled ('email_on_failure': False). Additionally, we've set the DAG to not catch up on any missed runs (catchup=False), ensuring that it runs strictly according to the schedule without retroactively processing past dates.
In summary, this DAG automates the entire process of fetching, processing, and storing weather data for Portland on a daily basis, ensuring that the data is readily available for analysis 
