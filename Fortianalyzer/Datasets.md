==Reports > Report definition > Datasets==
==A dataset is an SQL select query==

![[Pasted image 20241017015538.png]]

We have some specific gui interactions on top of SQL

The log type will determine what kind of logs will be stored in the $log variable
You can use specific logs like $log-attack, but is safer to use $log

You allso have to use the filter that indicates the devices, in which time period, this gets stored in $filter

Use the go button to test

### Macros

Fortianalyzer contains some default macros (stored sql queries) for common cases

##### root_domain
root_domain(hostname)
This function returns the root domain of the FQDN
![[Pasted image 20241017021257.png]]

##### nullifa
A function used to filter out n/a or N/A values
![[Pasted image 20241017021422.png]]

the actual query is 
 SELECT NULLIF(NULLIF(expression, 'N/A'), 'n/a').

##### email_domain and email_user
email_domain(from), returns anything after the @
email_user(from), returns anything before the @

![[Pasted image 20241017021608.png]]

##### from_dtime , from_itime
from_dtime(bigint) returns device timestamp with no timezone
from_itime(bigint) returns fortianalyzer timestamp with no timezone

![[Pasted image 20241017153949.png]]





### Log types

==When creating reports with fortianalyzerm the log type is assigned to the variable $log==

![[Pasted image 20241017012627.png]]
- $log-attack - attack log
- $log-dlp - DLP log
- $log-event - event log
- $log-netscan - netscan log
- $log-app-ctrl - [[Application control]] logs
- $log-emailfillter - emaill
- $log-traffic - traffic
- $log-virus - [[AV]] logs
- $log-webfilter - [[Web filter]] llogs