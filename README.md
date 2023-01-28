<div align="center">
<b>Real-time data fetching and visualizing</b>

Aliyev Nurlan

2022.12.15
</div>

It is required to fetch real-time readings from a weather station located in Budapest and visualize the data using Grafana software. There are various ways to tackle this problem but one of the efficient ways is to automate this task using Python. Fortunately, we can access API point, which returns current readings as JSON, then we can handle the data later on.

These events have their unique timestamps, thus we’ll be using InfluxDB, a timeseries based database, as the main database system. In this case, Python is preferred programming language to automate the tasks of fetching and uploading the data to InfluxDB. As the final stage, we’ll be using Grafana to visualize the data in the ways we want to see.

Below is the general workflow:

 - Fetch data from the API point and store the response,

 - Parse the returned JSON file to Flux annotated CSV file,
 
- Write the saved CSV file to InfluxDB
   
- Automate running the Python script

Prerequisites:

- Install influx-cli from [here](https://docs.influxdata.com/influxdb/cloud/tools/influx-cli/),
- Configure it as in *Figure 1* (Erased places include private tokens and server address)

<div align="center">
<img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/502a6ddc0768dedb2dfafb0f2e78ce8fba6de8cc/asset/influx_config.png" alt="Figure 1">
  <p><em>Figure 1</em></p>
</div>

 - Have Python installed, set up InfluxDB and Grafana on the server

1. Data Fetching

   We will be using ≪urlopen≫ and ≪json≫ modules
  
  Here as the first stage, we are opening the URL address of the API point to get the returned JSON and assign it in a variable. Then we treat the data as JSON using the `json.loads()` method (Figure 2.).
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/a5b84ef8fc4f54aec28b5a6b6c4950be262e0b87/asset/1.1e.png" alt="Figure 2">
   <p><em>Figure 2</em></p>
  </div>
  
  Next, we are destructing the JSON response and picking the readings to store them into variables and get them into a list (Figure 3.).
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/3d924d7c32609b680ade6f56493fee2f118fd709/asset/carbon%20(1).png" alt="Figure 3">
   <p><em>Figure 3</em></p>
  </div>

  As there can be “None” values in the readings because of faulty sensors, we have an extra safety phase to turn values to 0. (This part is optional, Figure 4.)
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/62089c50edec1ab62dea4b85b17843a116a97fb1/asset/carbon%20(2).png" alt="Figure 4">
   <p><em>Figure 4</em></p>
  </div>
  
2.  Data Parsing

  This phase requires some boilerplate code as InfluxDB requires Flux annotation.
  
  Here we are configuring the headers with required annotations (Figure 5.). In the following phase, we destruct the list into variables and then write read data as rows into the main list.
  
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/8a8daf941fa71516892589dacc234d764e92c219/asset/carbon%20(3).png" alt="Figure 5">
   <p><em>Figure 5</em></p>
  </div>
  
  This operation needs to be done for all of the readings (Figure 6.). After this phase we create the CSV file, write the headers and the data list, then save the file in the directory (Figure 7.).
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/e9e22a52db4eec8aaafe265b60ece9a29360bd91/asset/carbon%20(4).png" alt="Figure 6">
   <p><em>Figure 6</em></p>
  </div>
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/e9e22a52db4eec8aaafe265b60ece9a29360bd91/asset/carbon%20(5).png" alt="Figure 7">
   <p><em>Figure 7</em></p>
  </div>
  
 
3. 	Writing CSV file to InfluxDB  

 Finally, we have an annotated CSV file to write to our database. There are couple of things to pay attention:

- The path to “influx-cli” which is installed and configured is required 
-	The file path of CSV file we have created, which needs to be set once is required
-	It is preferred to use “/” (forward slash) instead of “\” (backward slash) as the directory separator of the paths
-	OS module needs to be imported
-	Bucket name must be set
-	This solution is to be used for Windows OS only

 The function which handles data writing is fairly simple, we inject CMD command into python file and let it run automatically (Figure 8.).

  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/0f5db117421ea1670d2fff8a271fdccd8e7425cc/asset/carbon%20(6).png" alt="Figure 8">
   <p><em>Figure 8</em></p>
  </div>
  
  At this point, we have finished the main logic of the application, if we run the script with the rest of the Python code added we get the result (Figure 9.).
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/246d888160aaa3f3187e1213d7ea861216b25aca/asset/final_cmd.png" alt="Figure 9">
   <p><em>Figure 9</em></p>
  </div>
  
