1. **Simple**
    - Only one record for multiple IPs
    - If you specify multiple IPs, Route53 would return them in random order
    - No health checks
2. **Weighted**
    - Splits traffic based on rule
    - Used with active-active systems
    - e.g. 80% to us-east-1 and 20% to eu-west-1
    - Helpful in situations when you want a small % of traffic going to new app version
    - Can be associated with health checks
3. **Latency-based**
    - Selects the route with lowest network latency for the end user
4. **Failover**
    - Selects based on health of the server
    - Used for disaster recovery
    - Used with active-passive policy
    - e.g. primary EC2 in us-east-1 and secondary S3 in eu-west-1
5. **Geolocation**
    - Routing based on location of the end user
6. **Geoproximity**
    - Routing based on geographical location of users as well as resources
    - Optionally choose to drive more/less traffic (aka throttle) to a given resource by a value, known as **bias**
    - A **bias** expands or shrinks the size of the geographic region from which traffic is routed to a resource
7. **Multi-value Answer**
    - Same as simple routing with health checks
    - Up to 8 healthy records are returned for each multi-value query
    - It's not a substitute for ELB