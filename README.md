# SOC Automation Home Lab

## Objective
Utilizing MyDFIR online guide, design and implement an automated Security Operations Center (SOC) workflow that enhances threat detection, incident response, and alert management. The primary focus was to generate test telemetry to mimic real-world attack scenarios, utilize a Security Information and Event Management (SIEM) system for real-time security monitoring, and incorporate Security Orchestration, Automation, and Response (SOAR) for automated playbook execution.

MyDFIR online guide: https://github.com/MyDFIR/SOC-Automation-Project

### Skills Learned
-	Enhanced understanding of SIEM concepts and practical application
-	Improved comprehension of SOAR capabilities
-	Gained hands-on experience deploying, configuring, and integrating security tools
-	Expanded knowledge of threat intelligence integration


### Tools Used
- Wazuh as the SIEM tool for log ingestion, analysis, and threat monitoring
- Shuffle as the SOAR solution to automate response to security alerts
- VirusTotal for enriching alerts


## Steps
<img width="468" alt="image" src="https://github.com/user-attachments/assets/25feb36c-df2e-46b2-ab70-e2d430526db9" />

*Ref 1: Network Diagram*


I began by creating a Ubuntu Virtual Machine (VM) to host the SIEM and a Windows 10 VM to be the test machine. Once the VM's were up and running, I installed and enabled Sysmon onto the windows VM and then installed Wazuh onto the Ubuntu VM. Once the Wazuh manager was installed, I logged onto Wazuh and added my Windows 10 test machine as an agent so we can detect threats.

<img width="468" alt="image" src="https://github.com/user-attachments/assets/7a24f9dc-52ea-437d-b63d-99f0d22b336d" />

*Ref 2: Successful installation of Wazuh manager*

<img width="468" alt="image" src="https://github.com/user-attachments/assets/a232085d-a756-4a66-88ed-30684a2bc3e9" />

*Ref 3: Installing and enabling Wazuh on my windows machine*

<img width="301" alt="image" src="https://github.com/user-attachments/assets/bea1ffa1-2a49-429c-8937-2f367086b14a" />

*Ref 4: Successful installation of agent*

Now that Wazuh is set up, the next thing to do is to begin generating telemetry from our windows test machine and ingest it into Wazuh. For this project, I wanted to detect and respond to mimikatz malware specifically. Mimikatz is used by hackers to extract sensitive information such as credentials and passwords. To begin generating telemetry, I configured the Wazuh manager on my windows test machine to ingest Sysmon logs and then I downloaded the mimikatz malware via GitHub, https://github.com/gentilkiwi/mimikatz/releases/tag/2.2.0-20220919.

<img width="391" alt="image" src="https://github.com/user-attachments/assets/58ef70e1-b6cc-4de5-969b-1159128796e2" />

*Ref 5: Configuring Wazuh to ingest Sysmon logs*

<img width="468" alt="image" src="https://github.com/user-attachments/assets/dd01ace3-a129-43b5-9ea5-11f347ca09ee" />

*Ref 6: Downloading Mimikatz*


By default, not all logs will show within the Wazuh manager unless a rule or alert is triggered. Therefore I had to configure the Wazuh manager to archive everything, regardless if a rule or alert has been triggered, and create an archive index.

<img width="461" alt="image" src="https://github.com/user-attachments/assets/5242bcc2-0f01-47ad-b572-a0d82fea8901" />

*Ref 7: Creating an archive index*

Next, I created a rule to detect mimikatz through the original file name and then executed the malware to test the newly created rule.

<img width="468" alt="image" src="https://github.com/user-attachments/assets/7423db85-37ed-4fca-9a87-fd31e6b5a2d9" />

*Ref 8: Creating mimikatz detection rule*

<img width="1512" alt="mimikatz usage in dashboard" src="https://github.com/user-attachments/assets/e21500ba-f386-418b-909c-8245745882d1" />


*Ref 9: Mimikatz usage detected*

<img width="1512" alt="proof rule works" src="https://github.com/user-attachments/assets/1bdbf4e0-8a41-408a-9b5d-6badbff63454" />


*Ref 10: Successful detection of Mimikatz*

Finally, I integrated Shuffle as our SOAR solution for automated playbook execution. For this project, the workflow is as follows: Mimikatz detected and alert is sent to Shuffle, Shuffle receives Mimikatz alert, extract SHA256 Hash from file and check reputation score with VirusTotal, send an email to SOC analyst to begin investigation.

<img width="468" alt="image" src="https://github.com/user-attachments/assets/2092cc40-ccac-46ca-a794-6d3ac1326799" />

*Ref 11: Shuffle workflow*
