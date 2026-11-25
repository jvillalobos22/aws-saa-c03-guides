##  I. Introduction ![[Pasted image 20250513160711.png|100 ]]

1. Auto Scaling Groups (ASG) can manage resources with user traffic.
    
2. The goal of ASG is:    
    1. Scale out (add instances)
    2. Scale in (remove instances)
    3. Ensure there's a min/max for both ends
    4. Automatically register instances to ELB
    
3. All ASGs have a **launch config** (old) or a **launch template** (new)
    1. **Launch configuration**
	    1. Old way (being phased out).
	    2. Define once → cannot be modified.
	    3. Must create a **new LC every time** you need a change (AMI, SG, etc).
	    4. Limited feature set (on-demand only, no mixed instances/Spot flexibility).
	    5. Can’t do versioning → only one active at a time.
	2. **Launch template**
	    1. Newer, more powerful (and **exam answer** if both are listed).
	    2. Support **versioning** → you can keep multiple versions of the same template.
	    3. Allow **mixed instance policies** (combine On-Demand + Spot in one ASG).
	    4. Support **latest EC2 features** (e.g. T2/T3 Unlimited, Placement Groups, Capacity Reservations).
	    5. Can be used outside ASG too (manual launches).
	    6. Best practice: always use Launch Templates unless the exam explicitly asks about Launch Config.
4. It is possible to scale ASG based on CloudWatch alarms
    
5. It is now possible to define **better** auto scaling rules that are directly managed by EC2
    1. Target average CPU usage
    2. Number of request on ELB per instance
    3. Average network in/out
    
6. We can also scale based on custom metric e.g. number of connected users
    1. Send custom metric from app on EC2 to CloudWatch (`PutMetric` API)
    2. Create CloudWatch alarm to react to low/high values
    3. Use the alarm as scaling policy for ASG
    
7. ASG will automatically create a new instance when an existing instance is down or unhealthy (+ terminate the unhealthy instance).
    
## II. Scaling Policies

1. **Target-tracking scaling**
	- Easiest to set up.    
    - Works like a thermostat (e.g., keep average CPU = 50%).
    - Best for proportional metrics (CPU, request count, network).
    
2. **Step scaling**
    - Alarm breach size = scaling step.
    - Example: CPU > 70% → add 2 instances; CPU > 90% → add 4.
    
3. **Simple scaling**
	- Single adjustment per alarm.    
    - Legacy, rarely used.
    
4. **Scheduled scaling**
    1. Anticipate a scaling event based on historical data e.g. add 2 units on Fridays
    
5. **Scaling cool-downs**
    - Prevents over-scaling (default = 300s).
    - Can override per policy (e.g., 180s for scale-in).

## III. Health checks

1. **Defaults**
    - ASG uses **EC2 checks** by default.
    - You can add **ELB checks** (recommended if attached).
    - Unhealthy = terminate + replace.
    
2. **EC2 health checks**
    - Built-in (hardware/software).
    - Every minute, can’t be disabled.
    - Status: _impaired_ → wait a few mins, then terminate if not recovered.
    
3. **ELB health checks**
    - Checks via protocol (HTTP, HTTPS, TCP, SSL) + ping path.
    - Marks instance `OutOfService` if failing.
    - NLB supports active/passive health checks.
    
4. **ASG behavior**
    - Launches replacement immediately when unhealthy.
    - Health check **grace period** (default 300s) → let new instance warm up.
    - With connection draining: ASG waits for in-flight requests to finish.
    - ELB will stop routing traffic before replacement, but some requests may fail.

## IV. ASG Tips

1. ASG default termination policy (balance instances across AZs)
    1. Find the AZ with most number of instances
    2. **Delete the one with oldest launch config**
    
2. ASG provides several lifecycle hooks to define custom actions
	![[Pasted image 20250513151709.png]]

3. ASG and [[ELB]] work hand-in-hand    
![[Pasted image 20250513151952.png]]

4. **Fleet management** — Automatically replace unhealthy instances and maintains your fleet at a desired capacity.
5. **If you delete an ASG, all its running instances will be terminated.**
6. You can configure SNS to receive notifications whenever ASG grows or shrinks.

7. **ASG can span across AZs but not regions.**
8. You can use AWS CodeDeploy or CloudFormation to orchestrate code changes to multiple instances in your ASG.
9. Data is NOT copied automatically to a newly created instance in ASG.
10. Lifecycle hooks let you take action before an instance goes in service or before it gets terminates e.g. **you could use a terminate hook to write fleet log to S3 when an instance goes out of service.**
11. You don't have to use ELB to use ASG! You can use EC2 health check to identify and replace unhealthy instances.
12. When an `impaired` instance fails a health check, Amazon EC2 Auto Scaling automatically terminates it and replaces it with a new one. If you’re using an Elastic Load Balancing load balancer, Amazon EC2 Auto Scaling gracefully detaches the impaired instance from the load balancer before provisioning a new one and attaching it to the load balancer. This is all done automatically, so you don’t need to respond manually when an instance needs replacing.
13. The turnaround time is within minutes. The majority of replacements happen within less than 5 minutes, and on average it is significantly less than 5 minutes. It depends on a variety of factors, including how long it takes to boot up the AMI of your instance.
14. If you are not using ELB, you can re-route requests using Route 53 or a reverse proxy.
15. **You can configure ASG to mix and match EC2 instance types (Spot, RI)**. You can also customize number of CPUs, RAM etc.
16. **You can specify which subnets Auto Scaling will launch new instances into. ASG will always aim to keep your instances balanced across AZs.**
17. **You can attach already-running EC2 instances to your ASG but make sure you don't exceed the upper cap of the ASG.**
18. **Make sure to add ALL subnets to ASG while in VPC. ASG cannot do that automatically.**
19. **Instances showing `OutOfService` from ELB? Health check configuration not properly defined.**
20. Prevent ASG from scaling temporarily? Use one of this 2 options
	 - Place the EC2 instance that is experiencing issues into the `Standby` state
	 - Suspend the scaling processes responsible for launching new instances

21. Actions such as changing the Availability Zones (AZ) for your group or explicitly terminating or detaching instances can lead to the Auto Scaling group **becoming unbalanced** between Availability Zones. Amazon EC2 Auto Scaling **compensates by rebalancing the Availability Zones**.
22. **When rebalancing**, Amazon EC2 Auto Scaling **launches new instances before terminating the old ones**, so that rebalancing does not compromise the performance or availability of your application.
23. However, **the scaling activity of Auto Scaling** works in a different sequence compared to the rebalancing activity. Auto Scaling **creates a new scaling activity for terminating the unhealthy instance and then terminates it**. **Later**, another scaling activity **launches a new instance** to replace the terminated instance.