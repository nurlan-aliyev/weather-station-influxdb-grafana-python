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
<img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/98dde8c92aad9d71b436c468fed7a12ac5cc1d0f/asset/1..png" alt="Figure 1">
  <p><em>Figure 1</em></p>
</div>

 - Have Python installed, set up InfluxDB and Grafana on the server

1. Data Fetching

   We will be using ≪urlopen≫ and ≪json≫ modules
  
  Here as the first stage, we are opening the URL address of the API point to get the returned JSON and assign it in a variable. Then we treat the data as JSON using the `json.loads()` method (Figure 2.).
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/7e741cfe56c0a0bb52d665dbb2dc451843a05e6c/asset/2..png" alt="Figure 2">
   <p><em>Figure 2</em></p>
  </div>
  
  Next, we are destructing the JSON response and picking the readings to store them into variables and get them into a list (Figure 3.).
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/7e741cfe56c0a0bb52d665dbb2dc451843a05e6c/asset/3.png" alt="Figure 3">
   <p><em>Figure 3</em></p>
  </div>

  As there can be “None” values in the readings because of faulty sensors, we have an extra safety phase to turn values to 0. (This part is optional, Figure 4.)
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/7e741cfe56c0a0bb52d665dbb2dc451843a05e6c/asset/4.png" alt="Figure 4">
   <p><em>Figure 4</em></p>
  </div>
  
2.  Data Parsing

  This phase requires some boilerplate code as InfluxDB requires Flux annotation.
  
  Here we are configuring the headers with required annotations (Figure 5.). In the following phase, we destruct the list into variables and then write read data as rows into the main list.
  
  
  <div align="center">
   <img src="https://github.com/nurlan-aliyev/weather-station-influxdb-grafana-python/blob/8a8daf941fa71516892589dacc234d764e92c219/asset/carbon%20(3).png" alt="Figure 5">
   <p><em>Figure 5</em></p>
  </div>
  
