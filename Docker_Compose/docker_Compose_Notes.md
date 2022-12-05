## Docker Compose

A container orchestration framework running on a single host machine. Assuming the user already has a set up for Docker (engine and compose), the start up was painless as we would provide all of the internals (application, requirements, yaml file(s), Dockerfile(s)). Only slight alterations would probably be needed to point the compose at the specific data/database endpoints for the particular user. 

**Overview of Exploration** 

- What tool(s) were used
    - Docker and Docker compose with various applications that ran in the containers. These were created with both a Dockerfile and pre-defined images pulled from repositories
- How were they configured (and why)
    - Most of the examples I tried were two container composes that pulled images and adjusted them slightly (with Dockerfiles to build). I stuck with some simpler examples of using docker compose, but does have the potential to expand the configuration and what exists in the containers. There are tons of examples that should cover the majority of our potential tools that may be used.
- Are there other ways of configuring the tool?
    - You can pull images or define your own builds with Dockerfiles to configure indivdual containers. Multiple containers go into the compose which will run on a single host. If you want to run multiple containers on multiple hosts, you will need Docker Swarm.
- Where is this tool best applied in the MLops infrastructure? (If it can/should be)
    - This would best be used in the MLops pipeline to run processing on data and generating models. One piece that may pose some additional difficulty is event triggering (e.g. a stage gets completed which triggers a new process to start (may require a new set of containers to start up or certain scripts to run)). This particular piece will probably need to have an additional layer of orchestration on top of the compose(s). But, various docker compose could provide a very lightweight base to build off of
- What way(s) did you interact with the tool (script, command line, UI, etc.)
    - Mostly the command line though a script could be easily built once the baseline compose was created. If nothing inside the compose (yaml, app, Dockerfile) needs to change, it is a one line command to start the compose

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

I ran some other basic examples of different Docker compose set ups just to try them out. There seems to be a lot of basic set ups that are very easily accessible and could be built upon. 

**Requirements**

- Does the tool meet the requirements needed for MLOps innately?
    - Yes as a baseline it will provide everything needed. There are gaps that exist in terms of full capabilites, but these can also be filled by additional tools that can be built over the containers.
- If missing requirements, can they be added with other tools or with internal development by MLOps team?
    - Docker seems to work well with most applications (e.g. can easily run a container with specific app/tool that is desired). With compose, these tools can be added into the whole picture. 
- Is the tool compatible with other planned infrastructure?
    - Yes same note as above. There are a significant number of simplified baseline examples with all sorts of tools and apps that integrate together. (golang stuff, postgres/mysql/other storage, grafana suite, python applications/libraries, frontend tools for web ui, etc.)

**User Experience**

- How easy was the set up/implementation? (1-10 scale with user insight)
    - 8 or 9; User would need to have docker downloaded and set up, but once that happens and the compose code is located where the user wants to host compose, then it is a single command from the CLI to run everything. A user with some knowledge of coding/CLI should have no issue starting the process
- How easy was it to accomplish the desired task (1-10 scale with user insight)
    - 8 or 9. Assuming all internal processes for the ATR suite are automated, tasks require little to no human interaction and could easily be abstracted to a single UI to prevent any need for code level interaction after the initial start up. (We could honestly just make a script that autonmatically downloads docker (if it isn't set up), downloads the ATR code suite, starts the compose(s), and opens the UI leaving only one line of CLI code required for the user to run)
- How easy was the shut down/clean up process?
    - 9 - exceedingly easy. One CLI line used to shut down the compose (or stop). One or two additional lines may be needed to clean up resoures/remove unneeded images
- Was there useful documentation?

**Strengths**
    
- What are the advantages of this tool?
    - Are there wishlist items that ship with the tool?
    - Is it flexible and versatile?
    - Is implementation straightforward?

Docker compose is very lightweight and is easy to set up. There is plenty of documentation and example code that makes a baseline start up extremely easy. The code structure is fairly straightforward which should make any development/adjustments to a base container image simple. You can also automatically restart containers on failure. It is flexible in the sense that you can create as many separate containers as required with different components/apps and these containers can all communicate with each other. Since Docker acts as the base for basically all container related work, pretty much every application we could want is available and can be built in a container as well as our own app development if required. Plus shut down and clean up are quick and painless.

**Weaknesses**

- What are the disadvantages of this tool?
    - Are there missing requirements?
    - Is it difficult to use?
    - Is integration difficult?

The compose is designed to run on a single host which may limit available compute power for major jobs. An additional level of Docker (Docker Swarm) is needed to do this and (though further work is required by me) the additional level is not as straightforward as the compose (would require additional commands/scripting to allow network communication between composes/containers as this would not occur innately). Compose also does not seem to have redundancy in the sense that if the host goes down there is no way to bring the compose back up after restart. There also seems to be less overall control with respect to services failing (which may not trigger events that need to occur). In general, there is less event triggering control. However, all of these shortcomings can be overcome with additional apps to help manage the overall orchestration/distribution.

**Concerns**

Docker compose does not seem to innately have auto-scaling and a few other tools that do come fairly readily with Kubernetes/Kubernetes tools. Additionally, it is designed to run on a single host which may limit (?) the ability to run larger jobs especially if an organization has limited compute power. However, both of these may be overcome with the addition of Docker Swarm. I need to look into this more.

**Recommendation**

*A random aside, turns out you don't necessarily need a Dockerfile. I did not realize this until I decided to try out a Grafana-Prometheus docker compose example (for old times sake). But, what I learned was that in the docker compose yaml file you either define the image you want to use/pull from a repository or define a path to the Dockerfile that will be used to build the container. Of course this can be unique between each containter within the compose. Thought this was pretty cool.*

I think docker compose can do most of what we need to do especially with some additional tools. It is very lightweight which is nice (and allows it to be pretty easily set up anywhere). Plus it is versatile in the sense that we can build a container to do pretty much anything and add it into our compose. Thinking with respect to the ATR-as-a-Product, I think this might be best applied as the base of our eventual experimentation suite. We would need to establish some UI tied to controlling/starting the compose, but due to the ease of start up and the lightweight nature of docker compose, I could see this as a useful sandbox (if you will) to test different configurations, models, tests, pipeline structures (I envision here is also where a grafana suite set up would be applied to get insight into the data pipeline functionality as we would not necessarily want this in the production cluster). And multiple people could test pieces locally or on separate VMs if needed. Ideally though it would still integrate back into the production (to help with pulling from the data lake and if changes are required and done within the sandbox provide an easy reintegration). 
