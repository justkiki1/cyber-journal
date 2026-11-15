# Week 1: Setting up Splunk and my first searches

## What I did
- Installed Splunk Enterprise (free trial) on my Windows laptop and logged in at localhost:8000.
- Ran my first search: `index=_internal | head 10`. It shows Splunk's own logs.
- Finished the free "Intro to Splunk" course and passed the quiz with 79%.
- Downloaded the BOTS v1 practice dataset, copied it into Splunk's apps folder, and loaded it. Splunk showed over 900,000 events.

## What broke, and how I fixed it
- **Windows 11 virtual machine:** I tried to build one in VirtualBox, but the screen stayed black. Task Manager showed my laptop's memory at 89%. My laptop has 8 GB of RAM, which is too little for a Windows 11 VM, so I moved on to labs that don't need one.
- **Splunk stopped loading:** After a restart, localhost:8000 said "refused to connect". I opened Command Prompt as administrator and ran `splunk start`, and it worked.
- **Copying the dataset:** Windows blocked me from pasting into the Program Files folder. I used the `xcopy` command from an administrator Command Prompt instead.
- **No results in my search:** I had picked a "real-time" time range. Changing it to "All time" fixed it.

## What I'd check first next time
1. Check how much RAM a lab needs before I install it.
2. Check that the Splunk service is running before I decide it's broken.
3. Check the time range whenever a search shows no results.

## What I learned
Splunk searches logs a bit like SQL. `stats count by` groups results, like GROUP BY.
