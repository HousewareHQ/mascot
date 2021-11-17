# Mascot
Mascot - The Telemetry SDK for Modern Revenue Teams

# The Why

Product-led Growth(PLG) is the Go-to-Market standard for SaaS in ‘21. Today, SaaS companies are building their data stacks in a multi-tenant fashion, typically setting up their Cloud Data Warehouse at Series C and onwards. The best data engineers are setting up pipelines and internal tooling that help them achieve a Customer 360 view, which becomes the core data layer for their PLG playbooks.

In the last few years, early stage B2B SaaS startups at the seed stage are facing the brunt of this expensive setup. Development teams in different companies are busy building similar suboptimal IFTTT tooling and BI dashboards on top of messy data sitting in different SaaS sources. With Web3, decentralization in the SaaS ecosystem will lead to an evolution of the data layer surrounding customer data.

We, at Houseware, aim to be a core enabler in the future of how teams will work with customer data applications. We are building the golden standard for SaaS customer data records, and we are open-sourcing our core data layer - *Mascot*.

# Mascot SDK

Mascot SDK is the core Telemetry SDK that powers Houseware apps and SaaS teams working with customer data! 

Mascot is tightly integrated with a cloud data warehouse hosting raw and processed customer data. It is the backend layer sitting between the end user and their SaaS Customer360 in a warehouse on Snowflake. MascotDB is a one-table representation of customer data aggregated from different SaaS sources -- which makes it easier to query, visualize and take decisions from customer data. This layer is being written in GoLang to optimize for high-performance, with the ELT framework built on Fivetran and DBT. This entire layer as well as REST APIs and SDK packages built on top of MascotDB shall be open-sourced.


The Mascot SDK can be consumed by PLG vendors for building their niche in the SaaS ecosystem. MascotDB allows for mature development teams in SaaS startups to consume the data directly via ‘Snowflake Shares' as well. Development teams can use Mascot’s cloud layer for storing their customer data, or even self-host MascotDB on their internal servers.

The following Python code snippet is our vision for Mascot’s SDK packages, growth engineers would now be able to define their revenue strategies in code. Assume pyscot is the Python package interfacing with MascotDB underneath the layer.

```
import pyscot


//Create a mascot object
mascot = pyscot.mascot(TEAM_ID= os.config(“MASCOT_TEAM_ID”), API_KEY = os.config(“MASCOT_API_KEY”)

// Get the list of all users who have clicked on the pricing plan after opening a campaign email in the last 7 days
campaign_leads = mascot.get_users_following_sequence(trigger_event=”Campaign Email Opened”, followup_event=”Clicked on Pricing Plan”, last_n_days=7)

// Send all these users an upsell pitch email with a Calendly invitation.
mascot.hubspot.trigger_email_campaign(user_list = campaign_leads, campaign_name = “Send Calendly invite for Upsell”)

```

# MascotDB

We are simplifying how customer data is stored on MascotDB, allowing for automatic SQL generation capabilities while maintaining a high-performance query engine. We are currently building dbt transformation packages on top of data ingested from several SaaS tools, writing custom logic for activities like ‘Invoice Paid’ or ‘Email opened’. As a result, we have one time-series table following the [Entity Attribute Value](https://en.wikipedia.org/wiki/Entity%E2%80%93attribute%E2%80%93value_model) data model, also commonly referred to as ‘long and skinny tabular structure. Self-joins on the entity and filter on timestamps/occurence counters help us satisfy many of the common use cases for Revenue & Growth teams. We are deliberately leaving out the exact database spec because we are running tests on a few more use cases before sharing this externally.

# Use Cases

Since customer activities in SaaS are well-generalized across the board, we are able to build querying capabilities that scale well across various use-cases. A few examples are:
 - Simple counters and aggregations:
   - How many users with the domain name ‘abc.com’ did X in the last Z days
 - Sequences:
   - Which users did X immediately after Y
 - Lead Scoring
   - Find the list of users on the ABC plan who have recently become power users(>N% increase in X over the last Y days), and have also clicked on Z at least once.
 - Conversion
   - What percentage of users did X immediately after Y in the last Z days
   - What percentage of users who do X do Y as well
 - Churn Prediction
   - Which users have NOT done X in the last Y days
 - Upsell
   - Deep dive into a user’s journey: List all events of customer X in the last Y days
 - Alerting/IFTTT:
   - If a user has performed X and Y excluding Z in the last N days, alert the team on Slack/send an email to ABC/trigger an email campaign to the user.
   - We have shipped our own alerting PLG application sitting on top of Product data from Segment and triggering alerts in Slack: [quick product demo](https://www.loom.com/share/c0e9c69996654d54a82b63f95ee36546) for the same!
 - Exploratory Analyses, Attribution & A/B Testing:
   - Let’s find all users who clicked on X but have never done Y, and deep dive into their event journeys
   - What percentage of End Users Vs Decision Makers have tried out Feature X
   - Email Campaign 1 vs Email Campaign 2: Which is yielding better results in terms of users doing X.

# Roadmap

We are currently building the data layer for MascotDB and integrating the first batch of SaaS sources(Segment, Amplitude, Hubspot, Customer.io and Stripe). We are parallely building the REST APIs that are consumed by our native PLG apps. Once a working version of this data+API layer is ready, we will be open-sourcing this repository.

We also plan to build a UI layer for Mascot, where users can manage their subscriptions to the different SaaS sources as well as access control for their team members. By connecting to Mascot, users get access to all PLG applications built by Houseware out-of-the-box.

We will be building Mascot in public, building a culture of external accountability in our systems. If you are interested in contributing, do reach out to the maintainers: [@navydish](https://github.com/navydish) and [@shubh24](https://github.com/shubh24)

# Open Questions

We are still in the early days here, and have many questions keeping us up at night. If you have ideas or thoughts around the same, do send us a ping!
 - Why should we stick to a single time-series table - can we shift to a star schema approach in the future, if needed? How is the performance vs use case trade-off going to look like in the long term?
 - Why should we have a set-in-stone database schema for MascotDB? Should we build functionalities for data engineers to be able to edit the structure of their own MascotDB and perform custom use cases on top of it?
 - Each SaaS source's spec changes every now and then, and backward compatibility should be well-supported by Mascot's APIs and SDK. How are we ensuring this?
 - For complex PLG use cases on the UI layer, how does the SDK need to evolve over time? The typical query patterns might not be enough for everyone.
 - We see the trend of users controlling their data in B2C applications with the wave of Web3 applications being built. In the near future, businesses will demand control over how and where their data is being stored/shared. Decentralization in B2B SaaS will fundamentally change how customer data is stored. How does Mascot evolve with the times?
 - What does our logo look like!

