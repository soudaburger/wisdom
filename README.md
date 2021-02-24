# General Principles

* If you do something once, it's acceptable to do it without automation. If you do it twice, you should automate it if you have time. If you do it three times, you should have automated it already.

* Any manual check that you do to determine your service's health can be turned into an automated monitor. Coverage builds confidence, and reduces incident response time.

* No matter what your own monitoring says, if your customer reports a problem, you have a problem. Measure as much as you can from the customer perspective (e.g. User Journeys). Catch things before they do.

* AUTOMATE YOUR TLS CERTIFICATE RENEWALS, FOR THE LOVE OF ALL THAT IS HOLY
  * Automate the monitoring, and tie it in to the renewal process. Make it impossible to "forget" to monitor a TLS endpoint.
  * Don't just read the cert off of the host; verify it from an external source.

* Slack Is Not Documentation. If you find yourself scrolling back to find some info, put that info in an actual document.

* Documentation is not just about the What, but also the Why. Describe what led you to make certain choices, and what kept you from choosing others. The context is crucial for future Operators.

* Someday, It'll Be Your Last Day - everybody moves on eventually. Write documentation as if that day is tomorrow, and you won't be around for follow-up questions.
  * Your Memory Isn't That Great - you'd be surprised at how many details you can forget, and how quickly it can happen. Take notes. They don't have to be documentation, but just something for your brain to key off of. Especially things that frustrated you or took a lot of time.
    * Ranting about it in Slack does not count as "taking notes"
    
* Most Problems Have Been Solved - maybe not *well*, but if you have a problem that it seems like other people would have, check to see if they do. Solutions will exist for many of them; do not write your own base64 encoder in JavaScript. That exists already.

* Don't put secrets in Git. ANY. Not even the keys you "don't care about". Make the habit unambiguous, and never do it. 
  * This will force you to learn about cool things like IAM and Vault; invest the time necessary to learn and adopt these methods.
  * Do not let developers check in passwords "for a POC I threw together" that they will "get rid of as soon as I add that in". They won't.
  * Usernames are secrets too.
  * All of this also applies to Containers.

* Make It Impossible To Fail. Humans are fallible, and will eventually make mistakes. Help them out by removing the opprtunity for mistakes to happen. Examples: 
  * Create succinct PRs that can be well-reviewed without something slipping through
  * Implement hard gates againt releases if monitoring / integration testing fails
  * Write tools with specific functions that limit the user's choice to what they need ("least-function" programming)
  * Don't provide access to any resource (host, file, website, network port, IAM Role) that isn't absolutely needed (do not use built-in roles)

* Dashboards provide context for an Alert. They're not for telling you there's an alert; you already know that.
* Email is a useless channel for alerting, if the intent is to respond to those alerts.
* Don't send alerts to people that aren't going to act on them.
* Don't send alerts that do not require a response. Alert fatigue can be very harmful to a team's confidence in their monitoring, which makes it Toil, not a Tool.

* If you haven't actually tested your DR plan, it doesn't exist
* If you haven't actually tested your backups, they don't exist
* If it's not in monitoring, it doesn't exist.

* Write comments in your code. Not long ones, not too many. Just in the places where someone might need more context (the Why).

* Set up Cost Alerts for your cloud accounts *now*. You do not want to find out the hard way.

* Outsource the common things to people who do it better than you. You cannot implement your own CDN more cheaply than you can using a vendor. You cannot run a more secure SCM service than GitHub. You cannot run a better email system than Gmail. Thankfully, you don't have to!

* If you find yourself setting up an FTP server, stop. There are so many better options.
  * If a customer is insisting, at the very least get them to use SFTP. Or a web-based option.

* The goal of a DevOps team is not to deliver a Service; it's to _always_ deliver a Service.

* Design as many services as possible to be multi-instance (preferably all of them). Being forced to solve the problems around concurrent instances will enable you to do things like externalize your state, load balance, auto-scale, etc. 


## Prioritize SECURITY
  * SECURITY is a paramount priority for the entire organization
  * SECURITY is carefully considered when it comes to production environments.
  * SECURITY follows the pattern of “least privilege” to ensure users and services are properly scoped to ensure that services maintain a stable contract avoiding service outages.
  * SECURITY allows “bad actor” behavior easier to discern.
  * SECURITY includes describing and enumerating any required ports, file system access, user authentication, and user authorization. 

## Prioritize observability

> **“You can’t fix what you can’t see”** -- The Invisible Man

  * OBSERVABILITY ensures we are allowing ourselves the best possible chance to audit events and behaviors. 
  * OBSERVABILITY enables any service that operates as a “black box” will quickly become despised by on-call engineers and will guarantee a lot of phone calls back to the service owner.
  * OBSERVABILITY requires that, at a minimum, these metrics should be collected: request rate, response rate, and evet rate (all reported as “per second”).
  * OBSERVABILITY means those metrics alone will only track the general behavior of the service.
  * OBSERVABILITY dictates that if this behavior generates an alert we’ll have to start hunting through the system to find out if there is any action actually required.
  * OBSERVABILITY means adding instrumentation in the function calls that generate (or recieve) requests, responses and events, to highlight bottlenecks and help provide context around potential issues. 
  * OBSERVABILITY demands that all services bound to a port should carry the base rate metrics (requests, responses, events)

## Prioritize RELIABILITY
  * RELIABILITY means understanding and enumerating your failure modes.
  * RELIABILITY knows it won’t be possible to capture them all, but starting a list of known failures will help build a “run book”.
  * RELIABILITY expects that in the event of a failure, the run book can contain instructions on how to recover. 
  * RELIABILITY helps understand how the service degrades, and what effect will that have on related services.
  * RELIABILITY would ask, is there enough capacity in a cluster to accommodate the loss of any 1 machine? What about 2 machines? 

## Prioritize REPEATABILITY
* REPEATABILITY ensures the entire infrastructure is defined in code with the ability to be applied idempotently
* REPEATABILITY expects that if the code is applied twice, it should NOT try to install the same packages twice, or trigger a software deployment for no reason.
* REPEATABILITY allows us to build for scabilty.
  * All infrastructure elements should be defined with elasticity in mind.
  * Planning ahead for dynamic environments will prevent painful refactors in the future.
* REPEATABILITY wanrns us to be wary of state.
  * Where is the service state stored? 
  * What happens if there is a loss of state? 
  * Can elements of the service be updated independently of services responsible for state? 
* REPEATABILITY would ask us to imagine we have a web site hosted on our web-server which stores state in a database on our db-server:
  * In this configuration, we can update the site on web-server without losing state.
  * It’s even possible that no customers were even aware of the change.
  * We can also code some smarts into our web ui to degrade gracefully when the database is unavailable for an update. 
  * What happens when state is lost for short periods of time? Minutes to hours? 
  * What happens when state is lost for longer than the duration of any local spooling?
  * If the state is stored in RAM on web-server, there is no way to upgrade the site without clearing state forcing users to start over (whatever that means for the site)

## Do NOT operate in a silo
> **I cannot stress this enough.**

* Make sure all that information you gathered and documented from the previous 4 items is centrally located and available to everyone.
* As we introduce on-call shifts, this information could be the difference between a small hiccup, and a 3am ALL HANDS ON DECK scenario.
* Speaking of on-call, you are still not alone!
  * On-Call shifts work best with defined escalation paths.
  * At a minimum, there will be a Primary and an Escalation.
  * When we add more people, we will add another layer to have Primary, Secondary, and Escalation 

## One more time as a checklist:

* Is the service being monitored?
* Do we know what the service behavior looks like when it’s “healthy” ?
* Is functional, but degraded acceptable for this service?
* Is it dependent upon any other service directly or indirectly?  Ensure those are annotated.
* What about “unhealthy” ?
* Is the underlying machine environment being monitored? 
* Do we have an idea of how it might fail? 
  * And if it does, how to address the problem? 
  * What is the impact?
* What is the urgency of repair efforts?
* If the on-call engineer  cannot easily diagnose and correct the issue, who should be called?  
  * And if that person is unreachable?
* They should have enough documented information to easily understand who they should call when issues arise.

