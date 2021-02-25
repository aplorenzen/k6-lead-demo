# k6 load testing demo

This is a demo on how to use a Postman request collection, as input for load testing with k6.

## Tools used
* k6 - https://k6.io/
Open-source load testing tool
* Postman - https://www.postman.com/
Collaboration Platform for API Development
* postman-to-k6 - https://www.npmjs.com/package/postman-to-k6
Converts a Postman collection to a k6 script
* InfluxDB - https://www.influxdata.com/
Time series database
* Grafana - https://grafana.com/
Self-managed observability stack tailored for enterprises
* Docker - https://www.docker.com/
Container platform for application

## Demo

1. Install the tools

* k6 - https://k6.io/
Open-source load testing tool
* Postman - https://www.postman.com/
Collaboration Platform for API Development
* postman-to-k6 - https://www.npmjs.com/package/postman-to-k6
Converts a Postman collection to a k6 script
* Docker - https://www.docker.com/
Container platform for application

2. Create a Postman collection
3. Export the Postman collection
4. Convert the Postman collection to a k6 test script
  ```bash
  postman-to-k6 neoprime.postman_collection.json --output scripts/k6-script.js
  ```
5. Start up an InfluxDB and Grafana instance
  ```bash
  docker-compose up influxdb grafana
  ```
  We can now follow the results of the tests  we'll be executing at http://localhost:3000

6. Execute the k6 script, outputting the results to InfluxDB
  ```bash
  docker-compose run k6 run /scripts/k6-script.js
  ```
7. Execute again, now with 50 virtual users and over 20 seconds
  ```bash
  docker-compose run k6 run --vus 50 --duration 20s /scripts/k6-script.js
  ```
8. Execute again, now ramping up from 0 users to 500 over 20 seconds, holding 500 users for 20 seconds and ramping down to 0 users over 20 seconds again
  ```bash
  docker-compose run k6 run --vus 0 --stage 20s:500 --stage 20s --stage 20s:0 /scripts/k6-script.js
  ```
9. Let’s add another dashboard template that gives us an even greater insight into the results. Copy the ID from the following dashboard into the dashboard import feature in grafana:

  https://grafana.com/grafana/dashboards/2587

10. When you want to shut down the stack:
```
docker-compose down
```