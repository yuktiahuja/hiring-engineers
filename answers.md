Your answers to the questions go here.
Prerequisites - Setup the environment
I used amazon ec2 ubuntu instance to do this project. 

 

Install datadog agent

 
Collecting Metrics:
Add tags in the datadog.yaml file
#env_test:datadog_tags_test
#test:succeeded
 
And also added user tags 
#my_sql_integration_datadog, #yukti


https://app.datadoghq.com/infrastructure/map?host=2076341972&fillby=avg%3Acpuutilization&sizeby=avg%3Anometric&groupby=availability-zone&nameby=name&nometrichosts=false&tvMode=false&nogrouphosts=true&palette=hostmap_blues&paletteflip=true&node_type=host
 
https://app.datadoghq.com/dash/host/2076341972?from_ts=1583737644506&to_ts=1583741244506&live=true&tile_size=m
 


INSTALL MYSQL on the ubuntu Host Machine
 

Create a datadog user in mysql

Verify that the user was created 
 

 

 

 

Mysql Integration with DataDog Agent
mysql.d/conf.yaml

 


Snapshot of /etc/mysql/my.cnf

 


MySql integration is successfully installed. 

 


https://app.datadoghq.com/dash/integration/12/mysql---overview?from_ts=1583736733706&to_ts=1583740333706&live=true&tile_size=m

 

Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000.

cat /etc/datadog-agent/checks.d/my_metric.py

from datadog_checks.checks import AgentCheck
from random import randint

class Check(AgentCheck):
    def check(self, instance):
        self.gauge('my_metric', randint(0,1000))

 

cat /etc/datadog-agent/conf.d/my_metric.yaml

init_config:

instances:
  - min_collection_interval: 30

 

Change your check's collection interval so that it only submits the metric once every 45 seconds.

cat /etc/datadog-agent/conf.d/my_metric.yaml

init_config:

instances:
  - min_collection_interval: 45

Bonus Question Can you change the collection interval without modifying the Python check file you created?

Yes, just change the parameter min_collection_interval to the desired value in the my_metric.yaml file. 

https://app.datadoghq.com/metric/explorer?live=true&page=0&is_auto=false&from_ts=1583737571929&to_ts=1583741171929&tile_size=m&exp_metric=my_metric&exp_agg=avg&exp_row_type=metric
 
 
Visualizing Data:
List of dashboards

https://app.datadoghq.com/dashboard/lists
 
https://app.datadoghq.com/dashboard/d2r-ewr-arn/mymetric?from_ts=1583739229257&to_ts=1583740129257&live=true&tile_size=l
 
Take a snapshot of this graph and use the @ notation to send it to yourself.
https://app.datadoghq.com/event/stream?tags_execution=and&show_private=true&per_page=30&aggregate_up=true&use_date_happened=false&display_timeline=true&from_ts=1583737500000&priority=normal&is_zoomed=false&status=all&to_ts=1583741100000&is_auto=false&incident=true&only_discussed=false&no_user=false&page=0&live=true&bucket_size=60000

 
 
It is displaying that the metric values go above or below the expected range based on past trends.


Monitoring Data
 

@ahujayukti2@gmail.com 

{{#is_alert}} Critical Alert: **my_metric** has a value of {{value}} and it is above {{threshold}} on Host IP: {{host.ip}} {{/is_alert}}
{{#is_warning}} **my_metric** is now warn over value of {{value}} on host {{host}} {{/is_warning}}  
{{#is_no_data}} **my_metric**  received  no data for a period of 10m {{/is_no_data}} 
{{#is_recovery}} Recovery Message: **my_metric** has recovered from an alert with a value of {{ok_threshold}} {{/is_recovery}}


Email notification screen shot
 

Bonus question (scheduled downtime monitor, I changed the time to 11pm as I wanted to take the screenshot when I was working on the test.)  

 

Two Monitor recurring scheduled 

 

 

Collecting APM Data:
https://docs.datadoghq.com/getting_started/tracing/


 

 


 

 

APM and infrastructure metric dashboards

https://app.datadoghq.com/dashboard/wb7-dgh-pgb/yuktis-timeboard-11-mar-2020-0147?from_ts=1583902663334&live=true&tile_size=m&to_ts=1583906263334&tv_mode=false


 
