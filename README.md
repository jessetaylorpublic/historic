# historic
historical data collecting container for Interactive Brokers api to be used as an init container for a Kubernetes deployment


This repository is a container with config file built for a Kubernetes deployment . Used as an init container once introduced to the environment it will run automatically till completion. By selecting the date and tickers in ibapi-config.txt you can select what to collect from Interactive Brokers api. 

Example:
Adding 10 tickers in the list in the config.txt file and setting the day=01, month=01, and year=2015, will collect about 5 years of minutely data for the 10 assets. This is done by saving the data locally in csv format. The file is structured  /data/<ticker>/<year>/<month>/<day>. Also to further improve load times the data is all combined and saved as full datasets in the /data/full/<ticker> directory. 

Limitations:
Data is pulled in one day increments due to api restrictions when loading minutely data. local directories will be checked first to confirm the directory does not exist. This solves the issue with waiting for api restrictions to collect data that is already present. This is helpful if the container is restarted. Only 60 request can be made in a 10 minute period so roughly one year of data can be collected per hour for one asset.  In the example above with a request of 5 years and 10 assets, its expected to take about 50 hours or just over 2 days. 

Deploying:
Update the ibapi-config.txt
Upload the container to a repository
Deploy the config file ibapi-config.txt to a cluster
Deploy as an init container to a Kubernetes deployment that collects live data 
