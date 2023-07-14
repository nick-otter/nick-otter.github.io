---
layout: post
title:  "The Story of Atlassian's 13-Day Outage"
categories: incidents
---

# The Story of Atlassian's 13-Day Outage
{: style="text-align: center"}

From [Stanislav Kozlovski](https://www.linkedin.com/in/stanislavkozlovski/?lipi=urn%3Ali%3Apage%3Ad_flagship3_pulse_read%3BFTAYtv0gRrWx9zMyM49YyA%3D%3D).

It’s a nice, refreshing spring day.
You open up your laptop in the morning and go straight to that design document you started last night.
You are excited to finish it before the meetings of the day start coming in.
Next, you plan to fix the critical Jira ticket.
Suddenly:
No alt text provided for this image
Little did you know you wouldn’t be able to touch that document, nor be able to close that JIRA ticket for TWO WEEKS.
Atlassian had a major 13-day outage for a subset of its customers in April 2022.
This is their story 👇
In 2020, Atlassian acquired Mindville.
Mindville’s flagship product was Insight - a general asset management software built on top of JIRA. Used for things like equipment tracking/management for IT, etc.
In 2021, Atlassian finished integrating the Insight product into their product natively.
In 2022, they began the process of deleting the old, standalone legacy app on their customers’ sites that had it installed.

Can you see where I’m going with this?

The engineers used an existing script and process to delete the instances of this application, but there was one unexpected thing that came up:

🚨 HUMAN ERROR 🚨

Miscommunication, in particular. Don't we all know that too well?
No alt text provided for this image
Team A was passing input to team B that would do the deletion.
Team A gave them the ID of the whole site on which the apps-to-be-deleted are situated.
A site in Atlassian terminology is the wide container of multiple products belonging to a single customer. (e.g. Confluence and Jira Software for `<site-name>.atlassian.net`).
The deleting team didn’t notice this, and directly passed the site ID to their script.
They went through a peer review process, but that process did not focus on cross-checking what the ID referred to.
It only focused on what endpoint is being called and how it’s being called.
The API had no further warnings or checks. Whatever you gave it, it executed.

The result?

775 customers’ sites were deleted on April 5, 2022.
• Jira
• the Confluence wiki
• their Pagerduty-competitor Opsgenie
were totally unavailable to these customers.

Imagine not having production alerting for two weeks. Wow. 🤯

Atlassian did not detect this.
The deletion was done via a standard workflow and no alerts fired off.
It’s only the first customer to open a ticket - a mere 8 minutes after the script started running, that had Atlassian begin to understand what had happened.
No alt text provided for this image
Unlucky timing, this coincided with their major annual conference - Teams22 in Las Vegas. 🥳
It’s likely a majority of their leadership was having long nights there.


Atlassian's response was to immediately form a cross-functional, global incident response team and have them work 24/7 until the incident is resolved. The incident management leaders met every 3 hours to coordinate the workstreams.
A priority was to establish communication with the owner of each impacted site to inform them of what had happened - but there was one problem.
The customer contact information was deleted.
No alt text provided for this image

The customers could NOT file tickets as they normally would because that required providing a site ID, which was now deleted. 😬
Atlassian didn’t have immediate access to key customer contacts like the IT administrator because it was simply deleted.
They scrambled to build an ad-hoc list through every possible way - billing, old support tickets, backups, direct customer outreach - quickly realizing these were largely outdated.

Quite the nightmare scenario. 😥

The lack of site IDs also broke their escalation management dashboards, as the tickets wouldn’t be grouped correctly.
How prepared were they to handle such an incident?

Well, they had previously touted all the disaster recovery work they’ve done in a blog post.

They were reasoning about and publishing their disaster recovery goals like:
💡 • Recovery time objective (RTO): How quickly can the data be recovered and returned to a customer during a disaster?
💡 • Recovery point objective (RPO): How fresh is the recovered data after it is recovered from a backup? How much data will be lost since the last backup?

RTO was < 4 hours and RPO was < 1 hour for their most critical services.

They had protection against:
• infrastructure-level failures; loss of a database, entire availability zone, etc.
• data corruption like ransomware, software bugs, operational errors - they use immutable backups & service storage backup restoration testing

But. These worked only for database-wide losses.
Since databases are shared by tenants, and only a few tenants were surgically deleted from it - none of the runbooks worked well to restore individual customer data in them.

You can’t naively roll back to a backup, as that would erase every non-impacted customer’s data.
They did have a runbook for restoring a single site after it was erroneously deleted - but it was not close to scaling for many customers at once.

So how did they deal with the 883 sites that were now gone?

🌪April 5-6th (day 1 & 2): Detection, start & choosing an approach
Most of the company figured out in the first day that restoring a site is a complex, multi-step process requiring many teams & days to complete.

During this time, many development teams executed the restoration steps for batches of sites, as well as polished & improved the automation to allow it to work for more sites at once.

The batches were of up to 60 tenants and the end-to-end time to hand a site back to the customer was 4-5 days.

Once they saw how slow it was coming along, they realized they needed to parallelize and automate the restoration process.

They split into two working groups:
1. manual execution group - validate the steps and execute them for a small number of sites. 🫡
2. automation development group - take the existing process and build automation to execute them across larger batches. 🤖

The reasoning was simple - the site restoration runbook was just too much work - it would have taken around 3 weeks.

The runbook was around 70 individual steps that centered around creating a new site for the customer and migrating all of their old data to the new site.

1. create a new site for each deleted one
2. re-create each individual product, service and data store inside the site.
3. re-map all of the old tenant data. Atlassian has an immutable “cloudId” identifier used to tag tenants’ data, and this new site was created with a new id. This meant they needed to re-map everything, and that was particularly problematic for third-party ecosystem apps.

Step 3) sounds like the kicker to me.

Of course, with so much complexity - there were other unforeseen dependencies between steps.
It was probably a huge scramble. 🗑🔥
No alt text provided for this image

Nevertheless, they put on their get-shit-done hats and restored 53% of the impacted users through this method between April 5th and April 14th. 🧢

On April 8th (3 days into it), they issued a code freeze to prevent any new changes from causing inconsistencies. All hands were truly on deck.

On April 9th (4 days into it), a faster approach was proposed. It seems like the automation team made good progress.
The “Restoration 2” approach offered more parallelism between the steps through reducing complexity & dependencies.

The key thing is they decided to re-use the old site `cloudId` identifier.
That removed over half of the Restoration 1 step.

If you ask me - Restoration 1 was more like a recreation, whereas Restoration 2 was a proper restoration process.

Anyway - they:
modified the automation scripts / processes to work for Restoration 2
restored the old records
validated carefully - they executed both Restoration 1 and Restoration 2 in parallel, in order to test & validate the new process.

That last bullet point is very important. Testing a brand new process during a production incident is incredibly risky.

Throughout this incident, they leveraged all of their 450+ support engineers across the globe.

This required the creation of new runbooks, workflows and handoffs/rosters.
Companies don’t usually have processes that help them mobilize the whole team on one task!
No alt text provided for this image

Throughout working on this incident, they incrementally optimized the process more and more, which can be summarized as:
more automation, more automation, more automation. 🤖
front-load and execute large steps asynchronously so that further steps don’t spend time being blocked on the sequential execution. ⚡️
As for the data they recovered – their RPO - they were able to get it back up to 5 minutes prior to the incident. 🏆
Their combination of full backups & incremental backups allowed them to choose any particular Point in Time to recover to.
Here is the full timeline, as shared by Atlassian in their RCA document:
No alt text provided for this image

This was an interesting summary of a messy incident - but what I aim to do here is to give YOU a bullet point list of mistakes you can learn from this.
The obvious disclaimer is that people don’t cause incidents - systems allow for mistakes to be made.
No alt text provided for this image

Incidents will always happen, and while Atlassian wasn’t perfect in dealing with it, they did get it sorted. 👏

It’s easy to jump on the bandwagon and badmouth the company - but that is generally a waste of time, emotions and energy.

What is productive, though, is to think deeply about:
what you would do in their shoes, were a similar incident to happen tomorrow in your company.
what universal principles should be applied to our software to reduce the chance of such outages.
In other words - mistakes you can learn from this. 💡
Pay attention to all points and think whether it applies to your organization.

Let’s dive into it
No alt text provided for this image
Atlassian's Learnings
First, we will go over what Atlassian publicly shared in their RCA document following the incident.

What they did and/or plan to do to improve:

✅ immediately blocked bulk site deletes - they made it impossible to repeat the mistake in the short term!
✅ soft deletes should be universal.
Good practice everywhere, a soft delete is the action of splitting a delete into two steps:
1. one that marks the intention for deletion.
2. another that does the actual deletion after some pre-configured retention time.
For Atlassian’s case - and any SaaS for that matter - the deletion of an entire site should simply not be possible without the customer deactivating it first.
Further, it should require multi-level protection to prevent errors and should retain data for enough time to allow for a safe and quick recovery.
And last but not least - soft delete actions should have a tested rollback plan that allows you to easily revert the decision.

Basically - make it hard for yourself to delete everything a customer has on your platform, and make it easy to restore.

✅ Disaster recovery should meet the RTO - accelerate multi-product/multi-site restoration for a larger set of customers

They slightly missed their Recovery Time Objective by around 13 days…
So they’ll work on accelerating multi-site restoration and add this particular case to their DR testing suite.
Lol. They better.
✅ improve the incident management process for very large scale incidents
The incident management process wasn’t made to support multiple sub-streams working on the problem at once.
At peak - there were hundreds of engineers/support working on it at once.
I don’t want to imagine the Slack channel...
No alt text provided for this image
✅ backup authorized account contact information outside the product’s instances
The last thing you want is to lose the contact information when you need it most. It’s a simple but perhaps non-intuitive step to keep contact information separate.

✅ invest in a unified escalation system & workflows that allow for multiple work objects (tickets, tasks, etc) to be stored under a single customer account object.

They had two large problems here with their communication system:
1. customers could not create tickets without a valid account.
2. the escalation management system didn’t support grouping tickets well.

Boring support work - but they need to create mechanisms to allow customers to contact them without a valid account, and group it together with the rest of the tickets that customers may have.

Who knows how messy it was to handle hundreds of customers, each having at least a few tickets from different users in the company.
✅ better communication

Their communication was arguably the worst thing here, as we will dive into later.
One of the worst things is Atlassian didn’t provide meaningful ETAs on restoration. ❌
Customers didn’t know how to plan around it - do we replace Jira with google sheets for a day, a week, a month?

What about OpsGenie?

Atlassian made life particularly difficult for the system admins that are the Atlassian champions in the customers’ companies. Those couldn’t communicate anything back to their teams on when they expect the pain to end.
Further, their public messaging seemed dumbed down and didn’t match what a technical audience would expect to see.

It’s understandable that the complexity and unique event made it hard to estimate the time, they should have been more transparent about what they know and don’t know, as well as keep giving out directional estimates.

They took an action item to form better global incident communication team - 24/7 coverage with designated staff, backups for each role and a quarterly audit to verify readiness.
As well as create a template for more broad, public communication through multiple channels.

Another thing they missed repeating were critical reassurances like:
“this is not a cyberattack”
“there was no data loss”

This was only done on April 12th - 7 days into it.
The more you keep the public in the dark, the more rumours start to fly around.
Alright. These were all of Atlassian’s notable action items.
I did extra digging around social media and forum boards.
When reading this, keep in mind that this is a perfect example of what will be noticed, discussed and upvoted by your technical customer base were such an incident to happen to you:

🙆 Atlassian tout a blameless postmortem culture, yet one of the first bullet point reasons was that the cause was a communication gap - noting that it’s a human error. (1)
🙅 No direct apology from the CEOs - the co-CEO preamble basically touts how reliable they are. (2)
🎙 People, especially those with large followings, push for transparency. (3)
Atlassian did NOT share the number of users affected. 775 customers - but one customer can be a company with thousands of employees.
Gergely Orosz estimated between 50k-800k users were impacted.
No alt text provided for this image

🧨 4. The higher the scale, the higher the stakes.
Only 0.18% out of 220,000 customers were impacted, yet due to the sheer number (a total of 50k-800k users) and, of course, the long time to resolution - this incident was talked about a lot.

🐌 5. It took Atlassian very long to acknowledge the outage across their main channels.

Their Twitter account was completely silent for 5 days straight (7-12 April) in the midst of the outage.
They only tweeted once on April 7 to share the fact that a script “disabled” sites.

This radio silence got people furious.

News trended in niche communities like Hacker News & Reddit - people guessed (some correctly) what was happening, mocked them and ultimately forced them to respond.
Since it happened in the middle of the week, people were furious that they had 3 straight days of downtime.
I don’t blame them - I have no idea what we would have done at my company were this to happen.

Around April 12 (7 days since the incident), some customers received direct communication telling them that they expect the data to be restored in UP TO 2 WEEKS.
Imagine receiving that kind of comms after 7 days of downtime and no real information - just rumors.

😷 6. Unacceptable product strategy
Some software is more critical than others.

Namely, OpsGenie, their Pagerduty-competitor - was down for the whole 13 days too.
It’s a pretty weird design choice to group it under the same site, but more importantly - why wasn’t this acknowledged?

There was a lack of ANY post-mortem wording on prioritizing or isolating OpsGenie.

It wasn’t even acknowledged that its recovery should have been prioritized. It wasn’t even mentioned that there were technical limitations to this.
How could anyone have trust in a critical monitoring solution after such an incident and post-mortem treatment of it?

🤬 7. Outrage is amplified during such incidents.
No alt text provided for this image

Expect to hear about every problem in your product during such times and have it upvoted from all the people outraged.

It would be wise to deploy some support engineers to handle at least some responses here.

These times also shine a great spotlight on disgruntled past employees that come up to bad-mouth the company.

The top-voted comment on HackerNews claimed that Atlassian had more single point of failures than employees and that more than 50% of incidents are customer-reported (undetected).

Who knows how true it is, but most people probably believed it.

📢 8. Great marketing opportunity for competitors
Business is cutthroat. Such long-running incidents are great marketing opportunities for competitors - and they will hop on it like none other.

One competitor was offering the rest of the year free (6 months) for affected customers.

I personally heard about at least 4-5 Jira alternatives I never knew existed.

😇 9. Just be transparent about it ASAP.
The word will inevitably spread anyway. And. You will share the truth in the post-mortem a few days later.

Atlassian initially said the sites were “disabled", yet people correctly guessed it was most probably a deletion.

Further - a commenter shared that their engineering friends who work there confirmed it was a script that deleted a bunch of sites.
If you don’t do this - you’ll get what Atlassian did, which was to have people say:
“This avoidance of clear communication did more to erode trust in the Atlassian brand than the outage.”

🛡 10. Consider protecting yourself against such vendor outages
Do you store your production runbooks in a SaaS?

Imagine you had stored them in the Confluence wiki and were unable to access for 13 days straight.

Customers asked for restoration of just certain pieces of critical data - Atlassian was unable to provide that to them.

👏 11. Don’t oversell yourself.
If you portray your company as better than it is, and then the truth inevitably comes out in an incident like this - people lose trust immediately and will most likely discount anything else you say in the future.

Again about transparency - explain the inconsistency that people were wondering about - how come you tout a 4-6 hour restoration time yet this incident is taking days with no clear ETA?

✅ 12. Back up everything
It may be basic disaster recovery - but kudos to Atlassian for it. While this incident was very bad, we should count our blessings - it would have been 100x worse if they didn’t have backups to restore to!

👤 13. Single Ownership
As a commenter said - if two teams own something - nobody owns it.
The fact that one team had to dig up and provide the IDs, and another owned executing them, is unnecessary risk.
In large organizations, everything goes better when you have clear ownership.

📰 14. PR Tricks
They posted their detailed outage report at 20:28 UTC on a Friday.
That’s 13:28 PST and 16:28 EST.

Some people speculated that this was intentionally done to reduce the chances of getting the media to report a rehash of the outage - i.e. hoping that the next tech cycle has more interesting things.

👌 15. Post-Incident Updates
This is for the overachievers out there - consider posting follow-ups months after the outage.

Blog posts about how each action item was implemented, referencing back to the original outage, is bound to start building back trust with your technical audience.

🧑💻 16. API design best practices
It’s good practice to design your IDs in a clear way that communicate what they are - e.g “site-12314” in Atlassian’s case.
Then, deletion APIs should take special care to accept just one type of ID.
UI/CLI should also ask for a confirmation and return back information like - “deleting 30 customer sites with X users and Y apps - are you sure? Reply with ‘delete-30‘ to confirm.”


That’s all I have. 🎬

To end this long post - there were some solid memes I would like to share.

The best comments I saw:

😁 “That’s SaaS for you - Shutdown as a Service”.


😂 “I’ve seen failing Kickstarter updates with more information than this.”

🤣 “They're rolling out another new Jira interface.”

---