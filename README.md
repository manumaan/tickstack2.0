# Custom TICK stack Sandbox 2.0
This is a custom TICK stack sandbox with:  

TELEGRAF 1.2
INFLUXDB 2.1
CHRONOGRAF 1.9
KAPACITOR 1.6
GRAFANA 7.1.0 

### Running

To run the `sandbox`, simply use the convenient cli:

```bash
$ ./sandbox
sandbox commands:
  up           -> spin up the sandbox environment (add -nightly to grab the latest nightly builds of InfluxDB and Chronograf)
  down         -> tear down the sandbox environment
  restart      -> restart the sandbox
  influxdb     -> attach to the influx cli
  flux         -> attach to the flux REPL

  enter (influxdb||kapacitor||chronograf||telegraf) -> enter the specified container
  logs  (influxdb||kapacitor||chronograf||telegraf) -> stream logs for the specified container

  delete-data  -> delete all data created by the TICK Stack
  docker-clean -> stop and remove all running docker containers
  rebuild-docs -> rebuild the documentation container to see updates
```

To get started just run `./sandbox up`. You browser will open two tabs:

- `localhost:8888` - Chronograf's address. You will use this as a management UI for the full stack
- `localhost:3010` - Documentation server. This contains a simple markdown server for tutorials and documentation.

### Grafana
Grafana can be accessed at localhost:3000 
Default credentials for Grafana: admin/admin 
Enter these and change the default password. 

Configuration:
Left side menu (+ icon)  -> create -> Dashboard -> Create a sample dashboard. Save. 
Left side menu (gear icon) -> configuration - Data Source 
 HTTP url:  http://influxdb:8086 
 Database: telegraf
Click Save & Test

> NOTE: Make sure to stop any existing installations of `influxdb`, `kapacitor` or `chronograf`. If you have them running the Sandbox will run into port conflicts and fail to properly start. In this case stop the existing processes and run `./sandbox restart`. Also make sure you are **not** using _Docker Toolbox_.

Once the Sandbox launches, you should see your dashboard appear in your browser:

### Configuration Steps 
Once sandbox is up, login to the influx db admin page: http://localhost:8086/
Enter these details:
Org: org
User: admin 
Password: admin123
Bucket: bucket1
Ensure the database initialization is completed. 

Copy the Admin's token from Data-Tokens screen and paste in telegraf.conf and kapacitor.conf. 

Restart telegraf docker from docker desktop and check status. 
Once telegraf is running, you can check the system metrics coming inside the bucket1 database. 

Restart kapacitor docker container from docker desktop and check status. 

From Chronograf you can add the database into the config as given here:
https://docs.influxdata.com/chronograf/v1.9/administration/creating-connections/#manage-influxdb-connections-using-the-chronograf-ui

TODO: 
Kapacitor is not getting added in Chronograf. It is working fine on its own
Telegraf is not getting added in Chronograf, but it is collecting system metrics fine and inserting into influx db. 

To map the bucket as per the v1.0 requirement: 
influx v1 dbrp create \
>   --org=org \
>   --token=qUWVacO43dVF4x9Mea4OtEmIycsBt6Ie82TWHMz-uiZ5RqM0cBhux5vws8TSpvm2XhMzbgrK7mY-FurT6BvLRA== \
>   --db bucket-db \
>   --rp bucket-rp \
>   --bucket-id 6f077cf310faf6e5 \ (ID can be see in influxdb - data -buckets ) 
>   --default

Reference: https://www.influxdata.com/blog/running-influxdb-2-0-and-telegraf-using-docker/
