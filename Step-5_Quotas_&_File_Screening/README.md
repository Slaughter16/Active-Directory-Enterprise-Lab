## Quotas & File Screening (FSRM)
-install file server resource manager 

-Create quotas to limit storage per folder or per user

-Configure file screens to block unwanted file types (e.g., .mp3, .exe)

-Configure notifications or reports for exceeded thresholds


1. Install File Server Resource Manager

go to manage add roles and features 
image
installation type keep it as role based click next 
image
server selection keep it default click next 
image

Server Roles File and Storage Services already checked but need to select what we wanted to install expand FIle and iSCI Services then select File Server Resource Manager Tools then cick add features then click next until confirmation 
image 
image 
image 

Confirmation click install and verify File Server Resource Manager installed
image
image

2. Open File Server Resouce Manager

To setup Quota managment expand quota management then right click Quotas and select create Quota which allows for more customzied quota
image
image

In create quota select the shared folder (HRData$) for the quota path  then click Define custom quota properties (Custom Properties) can select the limit for how much maxiumum to be can also put description and space limit and best practice select hard quota do not allow users to exceed limit 

image 
image 

then can add notification threshold click add  and can put 80% for generate notifications when usage reaches % and can check send email to following administartso such as the distro list 
image 

3. Setup File Screening managment 

expand file screening managemnt click file screens to contorl what file types on this shared folder for this lab we can restrict certian files so could select derive properoties from this file screen template but if want to block more file types can do custom properties for this lab we only wnat text and document file types in the shared folder so select custom properties  ensure active screening selected and check (Audio and Video Files, Compresses Files, Executable Files, Image Files, Web Page Files) then click okay and can also save template name so know which setting goes to
 image 
 image 
 image 

 verify file screening created
 image 
