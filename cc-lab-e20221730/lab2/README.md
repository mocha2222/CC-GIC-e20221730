# Cloud Lab 2 Submission — Load Balancer and Auto Scaling

Copy this template to `cc-lab-YOUR_ID/lab2/README.md` in your own repository. Follow [the instructions](lab2-instruction.md). Put your screenshots in `lab2/images/` with the file names below.

- **Student name: Soeng Prakbormey**
- **Student ID: e20221730**
- **Learner Lab user name: SOENG_PRAKBORMEY**
- **Date completed: 10/30/2026**

---

## Security groups

![LoadBalancerSecurityGroup](images/01-sg-load-balancer.jpg)

![LaunchTemplateSecurityGroup](images/02-sg-launch-template.jpg)

## Target group

![LoadBalancerTargetGroup](images/03-target-group.jpg)

![Health checks](images/04-health-checks.jpg)

## Application Load Balancer

![Load balancer list](images/05-load-balancer-list.jpg)

![Load balancer details](images/06-load-balancer-details.jpg)

## Launch template

![AutoScalingLaunchTemplate](images/07-launch-template.jpg)

## Auto Scaling group

![Auto Scaling groups](images/08-asg-list.jpg)

![Auto Scaling group details](images/09-asg-details.jpg)

## Load balancing test

![First request](images/10-browser-1.jpg)

![Second request, different host](images/11-browser-2.jpg)

## Simulated application failure

![Stopping httpd](images/12-stop-httpd.jpg)

![Target unhealthy or draining](images/13-target-unhealthy.jpg)

![New target healthy](images/14-target-replaced.jpg)

About how many minutes passed between stopping `httpd` and the new instance becoming healthy?

---

## Questions

1. **Why does the page show a different host name when you refresh it?**
   The load balancer sends each request to one of the healthy EC2 instances in the target group. Each instance has its own host name. Therefore, when the request goes to a different instance, the page shows a different host name.

2. **After you stopped `httpd`, the instance was still *running* in EC2, but the target group marked it *unhealthy*. What did the health check test, and why did the instance fail it?**
   The health check tried to connect to the instance using HTTP on port 80. It checked whether the web server could respond successfully. The EC2 instance was still running, but stopping `httpd` stopped the web server. The instance could no longer answer the health check, so the target group marked it as unhealthy.

3. **Who replaced the failed instance: the load balancer, the target group, or the Auto Scaling group? Explain the role of each one.**
   The Auto Scaling group replaced the failed instance. The load balancer sends user requests to the available healthy instances. The target group checks the health of the registered instances. When the target group reported that an instance was unhealthy, the Auto Scaling group detected the problem and launched a replacement instance to keep the desired number of instances running.

4. **With desired 2, minimum 2 and maximum 3, how many instances run normally? In what situation could there be 3?**
   Normally, two instances run because the desired capacity is 2. There can be three instances when traffic or demand increases and an Auto Scaling policy adds one more instance. The maximum capacity is 3, so Auto Scaling cannot normally increase the group beyond three instances.

5. **The instances' security group allows HTTP from anywhere. How could you change it so that the instances accept HTTP **only** from the load balancer? answer this question in simple word**
   Create or use a security group for the load balancer. In the instances' security group, remove the HTTP rule that allows traffic from `0.0.0.0/0` (anywhere). Add an inbound rule for HTTP on port 80 with the load balancer's security group as the source. Then users can access the instances through the load balancer, but direct HTTP access from the internet is blocked.

## Problems and how I solved them (optional)

## AI note (optional)

One suggestion an AI tool gave you and how you checked it.

## Clean up

- [ ] `MyAutoScalingGroup` deleted and its instances terminated.
- [ ] `AutoScalingLaunchTemplate` deleted.
- [ ] `MyLoadBalancer` and `LoadBalancerTargetGroup` deleted.
- [ ] Both security groups deleted.