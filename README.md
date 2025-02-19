# Spring boot integration with Deepseek using Ollama

DeepSeek is a Gen AI foundation model that helps answer questions, write text, or even generate code! 
It is similar to Open AI, Claude and other LLM models. However, it is open source and we can run it locally using Ollama.

## Install Ollama and run Deepseek locally

Download Ollama from https://ollama.com/download for your operating system and install following onscreen instructions.

Verify Whether Ollama is installed, go to terminal or Command prompt and type :
Ollama –version
To run Ollama , type :
Ollama serve

Once Ollama is running, we can install deepseek by executing the below command :

ollama pull deepseek-r1:latest 

Then on executing the below command, will list all the version of deepseek in the system :

Ollama list

<img width="451" alt="Screenshot 2025-02-18 at 9 26 03 PM" src="https://github.com/user-attachments/assets/8e7355aa-5ea3-4f5b-be9c-1059c2ab1b20" />

Finally, to run deepseek, execute the below command:

ollama run deepseek-r1:latest

Once we do this deepseek will start running and wait for the prompt :

<img width="577" alt="Screenshot 2025-02-18 at 9 26 35 PM" src="https://github.com/user-attachments/assets/1567f499-b359-4eb7-be8f-ae09d2542ce6" />

## Lets now build a simple Spring boot application to connect to the deepseek running locally :

Navigate to https://start.spring.io and add thymeleaf, Spring web and Ollama dependencies .
First of all add the below property in application.properties file :
spring.ai.ollama.chat.options.model=deepseek-r1:latest

Then Lets create a chat controller as below :

<img width="713" alt="Screenshot 2025-02-18 at 9 30 45 PM" src="https://github.com/user-attachments/assets/563f8a6b-1175-42dc-a1d6-b0e2be8ccf7e" />

It has a method to accept message prompt and send the response as a Flux stream to the front end page.

We are using Spring AI native methods to interact with AI models.

Lets create stream.html in the static foolder. Full code available in the github repo.

Once we start the application, and execute it runs like below :

<img width="1286" alt="Screenshot 2025-02-18 at 9 43 05 PM" src="https://github.com/user-attachments/assets/048ba2d9-93a1-4aba-8cb6-831bd2a91e40" />

### Congratulations we have created a chatbot in spring boot integrated with deepseek running in local using Ollama.

## Lets now run it with Groq API and deepseek model.

In the application.properties, comment the below line :
spring.ai.ollama.chat.options.model=deepseek-r1:latest

and add the below lines :

spring.ai.openai.api-key=<Your API Key> 
spring.ai.openai.base-url=https://api.groq.com/openai
spring.ai.openai.chat.options.model=deepseek-r1-distill-qwen-32b

Also in pom.xml comment the below dependency :

<dependency>
			<groupId>org.springframework.ai</groupId>
			<artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
		</dependency>

and add the below dependency :

<dependency>
			<groupId>org.springframework.ai</groupId>
			<artifactId>spring-ai-openai-spring-boot-starter</artifactId>
		</dependency>

Lets now run the application again :

<img width="1446" alt="Screenshot 2025-02-18 at 9 49 37 PM" src="https://github.com/user-attachments/assets/22c8a431-cd88-4306-beff-7a8c215a74a3" />

## Congratulations, we have now successfully ran our Spring boot chatbot integrating with groq API and deepseek model.

# Lets now deploy this application on AWS Elastic beanstalk environment. Below will be the architecture diagram :

![diagram-export-2-17-2025-10_06_20-PM](https://github.com/user-attachments/assets/5b5d31d7-9cbc-4750-9e7e-bc048a307b8c)

Navigate to AWS Console and Search for Elastic Bean stalk.

Click on Create Application. Enter the application name. Select the environment as Java 17. Enter any preferred domain name. Enter any version no .e.g 1.0

<img width="1583" alt="Screenshot 2025-02-18 at 9 53 56 PM" src="https://github.com/user-attachments/assets/b0bb5549-11f8-47f4-b769-12b0698df6a7" />

For application code, select from file and upload the jar file of our Spring boot application.

click next

For Service Access, we should create new service role. Key pair is optional .

<img width="1427" alt="Screenshot 2025-02-18 at 9 58 37 PM" src="https://github.com/user-attachments/assets/78c2fbf9-d3a9-44dc-9aa4-8434504b3ccf" />

For IAM instance profile, we need to create a new role. Go to IAM and click create role.

Add the below permissions to the role :

AWSElasticBeanstalkMulticontainerDocker
AWSElasticBeanstalkWebTier
AWSElasticBeanstalkWorkerTier
AmazonSSMReadOnlyAccess

Once the role is created , select it from IAM instance profile option and Click next

In the next few screens, keep everything else as default except environment properties , add
PORT as 8080
as Elastic bean stalk takes default port of 5000.

Finally hit submit. Application will be created in 5 mins.

<img width="1493" alt="Screenshot 2025-02-18 at 10 02 49 PM" src="https://github.com/user-attachments/assets/4356539d-d71f-4204-a90a-84de292b282a" />

Now, we can hit the url and it will look as below :

<img width="1389" alt="Screenshot 2025-02-18 at 10 03 43 PM" src="https://github.com/user-attachments/assets/7caa9f64-e815-4a6a-a4a3-31e9731153fe" />

### Congratulations, we have now successfully deployed Spring boot chatbot integrated with Groq API using Deepseek model on AWS Elastic bean stalk



