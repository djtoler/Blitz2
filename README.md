## URL Shortener Load Testing Summary

Initial Load Test:

Our application initially had its available CPUs handling stress from "sudo nice -n -20 stress-ng --cpu 2 &". 

Our QA engineer simulated user load using JMeter. They sent 14,000 GET requests to our application server at `http://52.90.119.24/5000`

Before Remediation

* Successful Requests: 13,933
* Failed Requests: 67
* Server Resource Utilization: 100%

During Remediation

* Stopped our EC2 instance
* Detached the EBS volume from our current instance 
* Vertically scaled our infrastructure to a instance with double rhe available CPUs
* Restarted our EC2 instance

After Remediation (projected)

* Successful Requests: 14000
* Failed Requests: 0
* Server Resource Utilization: 50%

Diagnosis & Solution:

We identified resource constraints on the t2.medium instance.

<p align="center">
<img src="https://github.com/djtoler/Blitz2/blob/main/medium_cpu_user_blitz2.PNG">
</p>

The image above shows our 2 available CPUs working at 100%. 


We switched to a t2.xlarge instance to double the CPU count.


Projected Results For Next Load Test:

We anticipate being able to successfully handle all 14,000 users on t2.xlarge without errors.


