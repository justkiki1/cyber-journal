# Week 2: Investigating a web attack with Splunk (BOTS v1)

## Summary
On 2016-08-10, two outside IP addresses attacked the Joomla admin page of the website imreallynotbatman.com. One IP made 412 automated login attempts in about 90 seconds. The other IP then logged in as the admin using the password "batman" and uploaded a PHP file called agent.php. It also browsed the server's folders. The first IP later requested agent.php 194 times, which suggests the file was used as a backdoor.

I used the free BOTS v1 practice dataset from Splunk. All times are as shown in Splunk.

## Timeline (2016-08-10)
| Time | What happened |
|---|---|
| 22:45:21 to 22:46:51 | 23.22.63.114 sends 412 login attempts to the admin page (brute force) |
| 22:48:05 | 40.80.148.42 logs in as admin with the password "batman" |
| 22:50:31 | 40.80.148.42 uploads agent.php |
| 22:51:36 and 23:16:14 | 40.80.148.42 browses the server's folders |
| 22:55:22 to 23:21:34 | 23.22.63.114 requests agent.php 194 times |

## Indicators
| What | Value |
|---|---|
| Attacker IP (scanning, login, upload) | 40.80.148.42 |
| Attacker IP (password guessing, uses agent.php) | 23.22.63.114 |
| Password used | batman |
| Uploaded file | agent.php |
| Attacked website | imreallynotbatman.com |

## Likely root cause
The admin account had a weak, guessable password, and the login page did not stop repeated attempts.

## Recommendations
- Use strong, unique passwords for admin accounts.
- Limit login attempts and block IPs that guess passwords.
- Restrict the admin page so only trusted people can reach it.
- Remove agent.php and check the server for other unknown files.
- Block both IP addresses.

## Splunk searches I used
Find the websites with the most traffic:
```
index=botsv1 earliest=0 sourcetype=stream:http | stats count by site | sort - count
```
Find who sent requests to the website:
```
index=botsv1 earliest=0 sourcetype=stream:http site="imreallynotbatman.com" | stats count by src_ip | sort - count
```
Find passwords tried from more than one IP:
```
index=botsv1 earliest=0 sourcetype=stream:http | search http_method=POST uri_path="/joomla/administrator/index.php" | rex field=form_data "passwd=(?<passwd>[^&]+)" | stats count values(src_ip) as src_ips by passwd | where mvcount(src_ips)>1
```
Find uploaded files:
```
index=botsv1 earliest=0 sourcetype=stream:http src_ip="40.80.148.42" | search part_filename=* | table _time uri_path part_filename
```
Find who used agent.php and when:
```
index=botsv1 earliest=0 sourcetype=stream:http | search uri_path="/joomla/agent.php" | stats count min(_time) as first max(_time) as last by src_ip | convert ctime(first) ctime(last)
```

## What I learned
- A password tried by the brute-force IP was later used by the other IP, which pointed to the successful login.
- A website's answer code can't always tell a failed login from a successful one, so I had to find another way.
- When a Splunk search is empty, check the field name first.
