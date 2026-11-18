# Week 3: What I learned from the Splunk investigation courses

## What I did
- Finished the free course **The Art of Investigation** and scored 92% on the quiz.
- Finished the free course **SOC Essentials: Investigating with Splunk ES 8.x** (about 6 hours of video).
- The free versions have no labs, so I practised the ideas on the BOTS v1 data in my own Splunk.

## What I learned
- **Think before you search.** The first step of an investigation is to decide what question I am trying to answer. In The Art of Investigation we looked into an illegal logon at a beverage company, and Splunk helped us get to the root of it.
- **Splunk Enterprise Security (ES)** is a Splunk product that security teams use to work through alerts in one place.
- **Managing risk:** risk scores help an analyst see which devices or users need attention first.
- **Containment:** if the risk is high and the attacker still has access, isolate the infected device so malware cannot spread.
- **Investigate, take action, analyse:** look at the logs to find out what happened, then act (isolate, block, reset), then review.

## How this connects to my BOTS investigation
In my BOTS write-up I asked one question at a time: who is sending the requests, what are they sending, did a password work, what happened after the login. That is the same method the courses teach.

If I had to contain that attack, I would:
1. Isolate the web server so the attacker cannot use the uploaded file agent.php.
2. Block the two attacker IPs.
3. Keep the logs as evidence.
4. Remove agent.php and reset the admin password.
5. Tell the team lead or the website owner.

## What I found hard
- ES and Risk Based Alerting were new terms for me. I had no lab to try them in, so I need more practice to understand them well.

## Next week
- Python script to count failed logins by IP.
- Centri Blue Team Junior Analyst certificate.
