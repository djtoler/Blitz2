# URL Shortener Load Testing Summary

## Objective
> #### _Implement a solution that will ensure that our URL shortener application infrastructure can successfully handle 14,000 GET requests at any given time._

## Initial Load Test:
> #### _Our instance we're starting out with is a t2.medium, that has 2 CPUs_
> #### _Our application initially had its available CPUs handling stress from the following command: `sudo nice -n -20 stress-ng --cpu 2 &`._
* > ##### `sudo nice -n -20` is a command that will give the command that follows it, prioroty in task scheduling of our machine. `nice` established that we want priority, `-n -20` set our "niceness" value to 20, which is the highest priority.
* > ##### `stress-ng --cpu 2` will be used to simulate high CPU utilization on 2 CPUs on our machine. 

> #### _Our QA engineer simulated user load using JMeter. They sent 14,000 GET requests to our application server at `http://52.90.119.24/5000`_

___

## Load Test Diagram
<p align="left">
<img src="https://github.com/djtoler/Blitz2/blob/main/Blitz2_Diagram.png">
</p>


### Before Remediation

* Successful Requests: 13,933
* Failed Requests: 67
* Server Resource Utilization: 100%

### During Remediation

* Stopped our EC2 instance
* Detached the EBS volume from our current instance 
* Vertically scaled our infrastructure to a instance with double the available CPUs
* Restarted our EC2 instance

### After Remediation (projected)

* Successful Requests: 14000
* Failed Requests: 0
* Server Resource Utilization: 50%

___

## Diagnosis & Solution:

#### _We identified resource constraints on the t2.medium instance._

<p align="center">
<img src="https://github.com/djtoler/Blitz2/blob/main/medium_cpu_user_blitz2.PNG">
</p>

#### The image above shows our 2 available CPUs working at 100%. We need a way to distribute this workload amongst additional CPUs


### There are hundreds of different instance types available that serve different computing purposes. 

<p align="left">
<img src="https://github.com/djtoler/Blitz2/blob/main/00110CC3-CFA4-45A1-B134-598CBC182038.jpeg" height="500">
</p>

> #### The t2.xlarge has 2 additional CPUs compared to our current t2.medium instance. Since our test had a 99.5% success rate, scaling our instance vertically to a t2.xlarge will give us the additional processing power to handle the 14,000 requests. We'll still have some available so that our server isnt operating at capacity and can possibly handle more request if necesary 

## Projected Results For Next Load Test:

> #### We anticipate being able to successfully handle all 14,000 users on t2.xlarge without errors due to the additional CPUs.

> #### When running `sudo nice -n -20 stress-ng --cpu 2 &` and monitoring our CPU with `iostat -xz 1`, while using our new t2.xlarge instance... we can see that our CPU only reaches about 34%. We anticipate that the additional 66% should be enough to handle the short burst of 14,000 GET requests.

<p align="center">
<img src="https://github.com/djtoler/Blitz2/blob/main/Screenshot%202023-10-10%20at%2012.28.44%20PM.png">
</p>

