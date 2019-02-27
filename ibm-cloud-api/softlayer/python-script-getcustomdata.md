---
description: Network Throughput을 가져오기 위한 Python Script
---

# Python Script - getCustomData

{% hint style="warning" %}
getVM\_NW\_PublicOut.py 조건 : 

* Server Type : 가상서버
* Public Network Outbound Interface
* Metric Type : Max\(평균, 합계도 가능\)
* 1달 기준\(2019-01-01 ~ 2019-01-31\)
* 10분 단위 측정\(unit : 600\)
* Input file : vs.csv
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="getVM\_NW\_PublicOut.py" %}
```python
import SoftLayer
from pprint import pprint as pp
import csv

class example():

    def __init__(self):
        self.client = SoftLayer.Client()

    def main(self, tracking_id):

        tracking_service = self.client['SoftLayer_Metric_Tracking_Object']
        #bw_result = tracking_service.getBandwidthData('2019-01-30', '2019-01-31', 'public', 600, #id=tracking_id)
        
            # In CPU case:
            # KeyName : CPU0,CPU1,...,CPUn
            # Name : cpu
            # summaryType: average/max/sum
            # In Memory case: KeyName : MEMORY_USAGE
            # Name : memory_usage
            # summaryType: average/max/sum
            # In Network case:
            # KeyName : PUBLICIN_NET_OCTET/PUBLICOUT_NET_OCTET/PRIVATEIN_NET_OCTET/PRIVATEOUT_NET_OCTET
            # Name : publicIn_net_octet/publicOut_net_octet/privateIn_net_octet/privateOut_net_octet
            # summaryType: average/max/sum

        object_type = [{'keyName':'PUBLICOUT_NET_OCTET','summaryType':'max','name':'publicOut_net_octet'}]
        summary_result = tracking_service.getSummaryData('2019-01-01','2019-01-31', object_type, 600, id=tracking_id)
        
        #args = {
        #    'startDateTime' : '2019-01-01',
        #    'endDateTime':'2019-01-31',
        #    'metrics': #[{'keyName':'PUBLICOUT_NET_OCTET','summaryType':'max','name':'publicOut_net_octet'}],
        #    'interval':600,
        #    'returnImage': False
        #}
        #custom_result = tracking_service.getCustomGraphData(args,id=tracking_id)
        pp(summary_result)

    # For use with a virtual Guest, just change
    # SoftLayer_Hardware_Server here with SoftLayer_Virtual_Guest
    def getTrackingId(self, server_id):
        tracking_id = self.client['SoftLayer_Virtual_Guest'].getMetricTrackingObjectId(id=server_id)
        return tracking_id

if __name__ == "__main__":
    main = example()
    
    f = open('vs.csv', 'r')
    rdr = csv.reader(f)
    a=0
    for line in rdr:
        my_server_id = line[0]
        tracking_id = main.getTrackingId(my_server_id)
        main.main(tracking_id)
        a+=1
    print(a)
    # CHANGE THIS
    #my_server_id = 50122710
    #tracking_id = main.getTrackingId(my_server_id)
    #main.main(tracking_id)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
getVM\_NW\_PublicIn.py 조건 : 

* Server Type : 가상서버
* Public Network Inbound Interface
* Metric Type : Max\(평균, 합계도 가능\)
* 1달 기준\(2019-01-01 ~ 2019-01-31\)
* 10분 단위 측정\(unit : 600\)
* Input file : vs.csv
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="getVM\_NW\_PublicIn.py" %}
```python
import SoftLayer
from pprint import pprint as pp
import csv

class example():

    def __init__(self):
        self.client = SoftLayer.Client()

    def main(self, tracking_id):

        tracking_service = self.client['SoftLayer_Metric_Tracking_Object']
        #bw_result = tracking_service.getBandwidthData('2019-01-30', '2019-01-31', 'public', 600, #id=tracking_id)
        
        object_type = [{'keyName':'PUBLICIN_NET_OCTET','summaryType':'max','name':'publicIn_net_octet'}]
        summary_result = tracking_service.getSummaryData('2019-01-01','2019-01-31', object_type, 600, id=tracking_id)
        
        # In CPU case:
        # KeyName : CPU0,CPU1,...,CPUn
        # Name : cpu
        # summaryType: average/max/sum
        # In Memory case: KeyName : MEMORY_USAGE
        # Name : memory_usage
        # summaryType: average/max/sum
        # In Network case:
        # KeyName : PUBLICIN_NET_OCTET/PUBLICOUT_NET_OCTET/PRIVATEIN_NET_OCTET/PRIVATEOUT_NET_OCTET
        # Name : publicIn_net_octet/publicOut_net_octet/privateIn_net_octet/privateOut_net_octet
        # summaryType: average/max/sum
        
        #args = {
        #    'startDateTime' : '2019-01-01',
        #    'endDateTime':'2019-01-31',
        #    'metrics': #[{'keyName':'PUBLICOUT_NET_OCTET','summaryType':'max','name':'publicOut_net_octet'}],
        #    'interval':600,
        #    'returnImage': False
        #}
        #custom_result = tracking_service.getCustomGraphData(args,id=tracking_id)
        pp(summary_result)

    # For use with a virtual Guest, just change
    # SoftLayer_Hardware_Server here with SoftLayer_Virtual_Guest
    def getTrackingId(self, server_id):
        tracking_id = self.client['SoftLayer_Virtual_Guest'].getMetricTrackingObjectId(id=server_id)
        return tracking_id

if __name__ == "__main__":
    main = example()
    
    f = open('vs.csv', 'r')
    rdr = csv.reader(f)
    a=0
    for line in rdr:
        my_server_id = line[0]
        tracking_id = main.getTrackingId(my_server_id)
        main.main(tracking_id)
        a+=1
    print(a)
    # CHANGE THIS
    #my_server_id = 50122710
    #tracking_id = main.getTrackingId(my_server_id)
    #main.main(tracking_id)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
getBM\_NW\_PublicOut.py 조건 : 

* Server Type : 물리서버\(베어메탈\)
* Public Network Outbound Interface
* Metric Type : Max\(평균, 합계도 가능\)
* 1달 기준\(2019-01-01 ~ 2019-01-31\)
* 10분 단위 측정\(unit : 600\)
* Input file : bm.csv
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="getBM\_NW\_PublicOut.py" %}
```python
import SoftLayer
from pprint import pprint as pp
import csv

class example():

    def __init__(self):
        self.client = SoftLayer.Client()

    def main(self, tracking_id):

        tracking_service = self.client['SoftLayer_Metric_Tracking_Object']
        #bw_result = tracking_service.getBandwidthData('2019-01-30', '2019-01-31', 'public', 600, #id=tracking_id)
        
            # In CPU case:
            # KeyName : CPU0,CPU1,...,CPUn
            # Name : cpu
            # summaryType: average/max/sum
            # In Memory case: KeyName : MEMORY_USAGE
            # Name : memory_usage
            # summaryType: average/max/sum
            # In Network case:
            # KeyName : PUBLICIN_NET_OCTET/PUBLICOUT_NET_OCTET/PRIVATEIN_NET_OCTET/PRIVATEOUT_NET_OCTET
            # Name : publicIn_net_octet/publicOut_net_octet/privateIn_net_octet/privateOut_net_octet
            # summaryType: average/max/sum

        object_type = [{'keyName':'PUBLICOUT','summaryType':'max','name':'vif_1_tx'}]
        summary_result = tracking_service.getSummaryData('2019-01-01','2019-01-31', object_type, 600, id=tracking_id)
        
        #args = {
        #    'startDateTime' : '2019-01-01',
        #    'endDateTime':'2019-01-31',
        #    'metrics': #[{'keyName':'PUBLICOUT_NET_OCTET','summaryType':'max','name':'publicOut_net_octet'}],
        #    'interval':600,
        #    'returnImage': False
        #}
        #custom_result = tracking_service.getCustomGraphData(args,id=tracking_id)
        pp(summary_result)

    # For use with a virtual Guest, just change
    # SoftLayer_Hardware_Server here with SoftLayer_Virtual_Guest
    def getTrackingId(self, server_id):
        tracking_id = self.client['SoftLayer_Hardware_Server'].getMetricTrackingObjectId(id=server_id)
        return tracking_id

if __name__ == "__main__":
    main = example()
    
    f = open('bm.csv', 'r')
    rdr = csv.reader(f)
    a=0
    for line in rdr:
        my_server_id = line[0]
        tracking_id = main.getTrackingId(my_server_id)
        main.main(tracking_id)
        a+=1
    print(a)
    # CHANGE THIS
    #my_server_id = 50122710
    #tracking_id = main.getTrackingId(my_server_id)
    #main.main(tracking_id)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
getBM\_NW\_PublicIn.py 조건 : 

* Server Type : 물리서버\(베어메탈\)
* Public Network Inbound Interface
* Metric Type : Max\(평균, 합계도 가능\)
* 1달 기준\(2019-01-01 ~ 2019-01-31\)
* 10분 단위 측정\(unit : 600\)
* Input file : bm.csv
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="getBM\_NW\_PublicIn.py" %}
```python
import SoftLayer
from pprint import pprint as pp
import csv

class example():

    def __init__(self):
        self.client = SoftLayer.Client()

    def main(self, tracking_id):

        tracking_service = self.client['SoftLayer_Metric_Tracking_Object']
        #bw_result = tracking_service.getBandwidthData('2019-01-30', '2019-01-31', 'public', 600, #id=tracking_id)
        
        object_type = [{'keyName':'PUBLICIN','summaryType':'max','name':'vif_1_rx'}]
        summary_result = tracking_service.getSummaryData('2019-01-01','2019-01-31', object_type, 600, id=tracking_id)
        
        # In CPU case:
        # KeyName : CPU0,CPU1,...,CPUn
        # Name : cpu
        # summaryType: average/max/sum
        # In Memory case: KeyName : MEMORY_USAGE
        # Name : memory_usage
        # summaryType: average/max/sum
        # In Network case:
        # KeyName : PUBLICIN_NET_OCTET/PUBLICOUT_NET_OCTET/PRIVATEIN_NET_OCTET/PRIVATEOUT_NET_OCTET
        # Name : publicIn_net_octet/publicOut_net_octet/privateIn_net_octet/privateOut_net_octet
        # summaryType: average/max/sum
        
        #args = {
        #    'startDateTime' : '2019-01-01',
        #    'endDateTime':'2019-01-31',
        #    'metrics': #[{'keyName':'PUBLICOUT_NET_OCTET','summaryType':'max','name':'publicOut_net_octet'}],
        #    'interval':600,
        #    'returnImage': False
        #}
        #custom_result = tracking_service.getCustomGraphData(args,id=tracking_id)
        pp(summary_result)

    # For use with a virtual Guest, just change
    # SoftLayer_Hardware_Server here with SoftLayer_Virtual_Guest
    def getTrackingId(self, server_id):
        tracking_id = self.client['SoftLayer_Hardware_Server'].getMetricTrackingObjectId(id=server_id)
        return tracking_id

if __name__ == "__main__":
    main = example()
    
    f = open('bm.csv', 'r')
    rdr = csv.reader(f)
    a=0
    for line in rdr:
        my_server_id = line[0]
        tracking_id = main.getTrackingId(my_server_id)
        main.main(tracking_id)
        a+=1
    print(a)
    # CHANGE THIS
    #my_server_id = 50122710
    #tracking_id = main.getTrackingId(my_server_id)
    #main.main(tracking_id)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

