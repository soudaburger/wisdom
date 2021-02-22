# General Principles

## Prioritize SECURITY
  * SECURITY is a paramount priority for the entire organization
  * SECURITY is carefully considered when it comes to production environments.
  * SECURITY follows the pattern of “least privilege” to ensure users and services are properly scoped to ensure that services maintain a stable contract avoiding service outages.
  * SECURITY allows “bad actor” behavior easier to discern.
  * SECURITY includes describing and enumerating any required ports, file system access, user authentication, and user authorization. 

## Prioritize observability
> **“If it’s not in monitoring, it doesn’t exist”** -- Este

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

