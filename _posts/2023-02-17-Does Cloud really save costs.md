---
layout: post
title:  "Ops: Does Cloud really save costs"
categories: ops
---

# Ops: Does Cloud really save costs?
{: style="text-align: center"}

| Alex Xu | [Alex Xu](https://www.linkedin.com/posts/alexxubyte_systemdesign-coding-interviewtips-activity-7025866484350373891-B6LQ/?utm_source=share&utm_medium=member_android) |

Let’s look at this question 𝐢𝐧 𝐚 𝐥𝐨𝐧𝐠𝐞𝐫 𝐭𝐢𝐦𝐞 𝐫𝐚𝐧𝐠𝐞 to see what the cloud really brings us.

🔹 When a company or a business line initially starts, product-market fit (PMF) is key. The cloud enables quick setup to run the system with minimal necessary hardware. The cost is also transparent.

For example, if we run the databases on-premise, we need to take care of hardware setup, operating system installation, DBMS maintenance, etc. But if we use Amazon RDS (Relational Database Service), we just need to take care of application optimization. This saves us the trouble of hiring Linux admins and DB admins.

Later, if the business model doesn’t work, we can just stop using the services to save costs without thinking about how to deal with the hardware.

🔹 In research conducted by Cameron Fisher, the cloud starts from 𝐚𝐥𝐦𝐨𝐬𝐭 𝐳𝐞𝐫𝐨 𝐜𝐨𝐬𝐭. Over time, the cost starts to accumulate on subscriptions and deployment consulting. Ironically, because it is so easy to allocate services to the cloud for scalability or reliability reasons, an organization tends to 𝐨𝐯𝐞𝐫𝐮𝐬𝐞 the cloud after adopting the cloud. It is essential to set up a monitoring framework for cost transparency.

👉 Over to you: Which notable companies use on-premise solutions and why?

Reference:
1. AWS guide: Choosing between Amazon EC2 and Amazon RDS
2. Cloud versus On-Premise Computing by Cameron Fisher, MIT

![](/assets/cloud_costs.png)

---

Comments:

```
This looks like it only accounts for infrastructure. 

What are the ongoing costs of:

- having to pay and run an operations team
- slower development => larger team or slower development
- higher downtime ( you can't expect to compete with AWS) 
- opportunity cost 

I also don't think you can do a reasonable comparison without attaching a scale of company to the equation. 

If you're a startup with 20 devs, even the added overheads of an ops team could ruin the books. 
"We saved 50% vs our $400k AWS bill by hiring a $300k ops team"

If you're the size of basecamp then the overheads of running your own hardware are dwarfed by the cost savings vs cloud. 
```
```
I am Cloud First and work almost exclusively on cloud applications (serverless first even, but not exclusive) my thoughts on this debate is anytime someone says it's one way or the other they are missing many key points and ignore situational context.

Some key points I look for:
Are they avoiding lift and shift?
Are they using cloud native?
Are they using managed services?
Are they using serverless?
Do they monitor and adjust provisioned resources?
When accounting for costs, do they include total cost of ownership (e.g. employee time)
Even if more expensive, does the freedom of clicking a few buttons to change hardware power meet their needs better?
What is the outlook 10 years from now for this?
What sort of on prem solutions would be the alternative and do they provide the robustness and uptime needed by the client?
How much money is lost by either waiting for hardware to be delivered and installed, or lost money per minute/hour/day of downtime?
What happens if there is turnover in the staff, can the new staff get up to speed quickly on either the on-prem solution or the cloud solution?

There's probably more, but depending on many of these answers the move to cloud could be worth it or it could be a complete disaster.
```
```
For longer run you can get into 3yr-5yr contract where cost will reduce down to as low 50%.

Besides this after 5 yrs your on-premise hardware might become obsolete and then you again have to reinvest in capex to buy all of them.

But in cloud you can easily replace old hardware with the new as you are still on rental basis.

There are other factors like dynamic requirements, scalability, flexibility all these can make big difference to the business which is not considered in the above graph.
```
---