## Docker Compose

A container orchestration framework running on a single host machine. Assuming the user already has a set up for Docker (engine and compose), the start up was painless as we would provide all of the internals (application, requirements, yaml file(s), Dockerfile(s)). Only slight alterations would probably be needed to point the compose at the specific data/database endpoints for the particular user. 

**Overview of Exploration** 

- What tool(s) were used
- How were they configured (and why)
- Are there other ways of configuring the tool?
- Where is this tool best applied in the MLops infrastructure? (If it can/should be)
- What way(s) did you interact with the tool (script, command line, UI, etc.)

**Procedure**

Relatively simple to run if the environment is set up (I already had both the docker engine and docker compose on my Linux kernel so I did not need to download/set up anything to make simple examples work). However, for anyone who does not have those pieces in place on whatever OS they run on, this will be an additional requirement. 

I then copied a simple Hello World docker compose set up into a test directory. This included 4 items: the app with a flask app hosted with redis, the requirements for the containers, the docker-compose.yml (describes how to set up the services), and the Dockerfile (runs the start-up for the Docker containers). Once those were in place it was one command to get the app up. *I had to add sudo in front of all docker commands probably because I did something dumb when I set it up/didn't add the group properly. Additionally, I had to run ```sudo service docker start``` to initialize my docker daemon*

```docker compose up```

or 

```docker compose up -d``` to run the compose in detached mode

Then, to stop the service once you are complete and subsequently bring all of the containers down (and removed entirely).

```docker compose stop```
```docker compose down``` 

*Add -v or --volumes to the down command to remove any volumes associated with the service*

**Requirements**

- Does the tool meet the requirements needed for MLOps innately?
- If missing requirements, can they be added with other tools or with internal development by MLOps team?
- Is the tool compatible with other planned infrastructure?

**User Experience**

- How easy was the set up/implementation? (1-10 scale with user insight)
- How easy was it to accomplish the desired task (1-10 scale with user insight)
- How easy was the shut down/clean up process?
- Was there useful documentation?

**Strengths**
    
- What are the advantages of this tool?
    - Are there wishlist items that ship with the tool?
    - Is it flexible and versatile?
    - Is implementation straightforward?

**Weaknesses**

- What are the disadvantages of this tool?
    - Are there missing requirements?
    - Is it difficult to use?
    - Is integration difficult?

**Concerns**



**Recommendation**

User recommendation on the tool for potential NLOps integration. Add any additional notes or caveats here. 
