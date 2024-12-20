# ASE 2024: TSDB Lab


___
## Prerequisites and dependencies

- Ubuntu 18 or higher (For a different OS, please refer to the [InfluxDB installation portal](https://portal.influxdata.com/downloads) and install InfluxDB version 2.7.1). 
- Docker version 20.10 (or higher)
- Clone this repository, you will need the data files. 

___
## Preparation
```bash
sudo apt-get update
```

Docker installation (skip if Docker is already installed)
```bash

sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce -y
```


___
## InfluxDB Installation
Use the following command to download and run the InfluxDB v2.0 Docker image. Expose port `8086`, which InfluxDB uses for client-server communication over the `InfluxDB HTTP API`.
```bash
sudo docker run --name influxdb -p 8086:8086 influxdb:2.7.10
```
Or if it is already installed: 
```bash
sudo docker start influxdb
```
You should now be able to access the web interface by opening your web browser and navigating to [http://localhost:8086/](http://localhost:8086/). 
- Create an account and organization, set up your first bucket, and save the provided API token.
- Upload the dataset bitcoin-historical-annotated.csv into your bucket under sources (second option on the left) and select "upload a CSV".
- Inspect the data with the Data Explorer (third option on the left) by selecting your bucket and the measurement 'coindesk', to see the data you need to select a custom time range starting from 2022-10-11 11:00:00.

___
## Telegraf Installation
In a new terminal, Install Telegraf from the InfluxData repository with the following commands:
```bash
wget -q https://repos.influxdata.com/influxdata-archive_compat.key
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list

sudo apt-get update && sudo apt-get install telegraf
```
Check your installation 

```bash
telegraf version
```

## CLI Setup
Install Influx CLI (for other OS follow this [link](https://portal.influxdata.com/downloads/)):
```bash
wget -q https://repos.influxdata.com/influxdata-archive_compat.key
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list

sudo apt-get update && sudo apt-get install influxdb2-cli
```
To avoid having to pass your InfluxDB API token with each influx command, set up a configuration profile to store your credentials–for example, enter the following code in your terminal:

```bash
# Set up a configuration profile
influx config create -n default \
  -u http://localhost:8086 \
  -o INFLUX_ORG \
  -t INFLUX_API_TOKEN \
  -a
```

Test the shell: 
```bash
influx v1 shell
> use "your_bucket"
>
> select * from "coindesk";
```

___
## Data Querying

Examples of data queries in Flux can be found on the following [page](https://docs.influxdata.com/influxdb/cloud/query-data/flux/).

___
## Dashboard templates: 

InfluxDB Community Templates can be found [here](https://github.com/influxdata/community-templates). 


