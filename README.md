# Access Separation within a single Confluent Cloud Kafka Cluster

## 0. Pre-requisites
You should have a Confluent Cloud account with credentials ready. If you dont have one, you can sign-up for Confluent Cloud at https://www.confluent.io/confluent-cloud/tryfree/

New signups receive $400 to spend during their first 30 days. No credit card required

You can also Sign up through your cloud marketplace (AWS, Azure and GCP) to unify billing and leverage existing commitments.

#### Create Environment and Cluster 
You can follow the link here. https://docs.confluent.io/cloud/current/get-started/index.html#quick-start-for-ccloud

## 1. Create Topics

- Once you are inside a cluster, click on "Topics" on the left navigation menu  > Click on "+Add Topic" or "Create topic"
- Give a name to topic and choose partitions as needed and click "Create with Defaults"
![image](https://user-images.githubusercontent.com/73946498/191195883-5a99e864-f76a-4d4b-8ddc-3c530a2e5378.png)

## 2. Create Service Account and ACLs for Team 1.

- Go to "Data Integration > API Keys" on the left navigation menu and click on "+Add Key"
- Choose Granular Access
- Choose "Create a new Service Account" and give a name and Description
![image](https://user-images.githubusercontent.com/73946498/191196576-ec1127cf-7ebc-4149-bc7f-89abcb2682a7.png)

- Go to "Topic" tab > type "team1". choose from suggestions which says "Create "team1"" > Patterntype = Prefixed > Operation = Read and Permission = Allow
- Choose "Add ACLs" 
- Go to "Topic" tab > type "team1". choose from suggestions which says "Create "team1"" > Patterntype = Prefixed > Operation = Write and Permission = Allow
- Choose "Add ACLs" 
- Go to "Consumer Group" tab > type "\*". choose from suggestions which says "Create "\*"" > Patterntype = Literal > Operation = Read and Permission = Allow
  
  It should look something like this
 
  ![image](https://user-images.githubusercontent.com/73946498/191198076-be38412d-e0e7-42f6-84c1-3cce864728c6.png)

- Click Next > displays API Key and Secret. Copy them and click "Download and Continue". It saves the Key-Secret pair in a file. Keep it in a safe and secure place.

## 3. Repeat the steps in Section 2 by replacing "team1" with "team2" to have a Service Account and ACLs created for Team 2.

## 4. Produce and Consume data using Team 1's Service Account to team1.orders topic works but not for team2.users topic
I will be using CLI to demonstrate granular Access. For first time users, click on 'CLI and Tools" on the bottom left corner of the left navigation pane.
"Confluent CLI" tab shows all the required commands with some pre-populated info such as environment-id and cluster-id.

![image](https://user-images.githubusercontent.com/73946498/191204328-a436b4da-d3e2-406b-8a3a-26b4f6c76d1f.png)

P.S: Ignore --offset in consumer. I had a lot of messages already present in the topic.

You can also see message in the Confluent Cloud UI
![image](https://user-images.githubusercontent.com/73946498/191202903-7f6bb0b6-193e-48b2-abdf-13d0a307afcd.png)


## 5. Produce and Consume data using Team 2's Service Account to team2.users topic works but not for team1.orders topic

![image](https://user-images.githubusercontent.com/73946498/191205949-80f6a175-9a32-4410-a4ed-fb37fbe7abd7.png)


## 6. Create a new topic with "team2" prefix and test

![image](https://user-images.githubusercontent.com/73946498/191218089-f7177aba-15f1-47a3-b5e9-e9a1bc5bd6fa.png)


## References
- Using ACLs: https://docs.confluent.io/platform/current/kafka/authorization.html#using-acls
