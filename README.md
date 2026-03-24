# Me_and_LXC
General scenarios where I have used LXC over Docker and/or Podman

## AI and LXC

Consider:  You have a laptop or Mac and want to explore AI or compare Linux distributions, etc.  The problem is once you start installing packages outside an enterprise you have to make sure packages are updated, zero days are known and addressed, and run the risk of legacy code or worse, opne ports, running because you forgot to end a project or turn them off.  Now my Linux-native Dell XPS 13" is great for always installing updates ever time it starts up, but that doens't apply to the code, etc. especially with full stack applications and possible each with their own database. 

## Enter LXC.

Consider you want to try AI models or liked the app you had Agentic AI produce and want to run the front end dashboard locally and even keep the database in a cloud service like Neon or Supabase.  

By creating a consistent folder structure (or Github repositories) for your projects, you can launch then in containers, and for persistence, use systemd to keep them running.  

On my Ubuntu LXD setup, any container I launch is running when I restart my laptop. So using dashbloard.local:<port#> ensures I have access my application.

I focus on running "rootless containers" launched by an unprivledged account (no sudo rights, no wheel either).  A separate MARKDOWN file will provide details in the future (it's basic LXC setup)

## Organizating Containers as Folder Structure
For containers to work, just like safe Git repositories, only the project code and configuration should be kept in a folder.   All cloud specific credentials, accounts, etc have to remain outside.  Same goals for AI Models. 

1. For Models, I currently have a monitoring app that watchs /ai/models, and then provides a menu that you can select with model or models you will launch in containers.  As an improvement, I have some montoring tools on actual laptop resource consumption, that could be added (in future) to the same menu so you can see what's already running (and jump to its gui or cli), and have some controls (archive, backup, recover, terminate, schedule for deletion).
2. For Apps, or Projects, I currently have a app framework that will launch one or more apps, each with a corresponding folder in /ai/projects/, which is copied to /opt/(projectname) of the launched container. Again, as an improvement, the framework currently uses templates to launch systemd for any app I want persistent. I have yet to add rules to Agentic AI to match the ".config/settings.json" configuration I have per instance for my Go and python projects, but see this as an improvement down the line in terms of my "taste" or "style" of generated agenctic AI apps.  My current goal is to get through projects and launch apps to understand the ecosystem for persistent apps, which AI model always chooses the same stacks (great studies on this already online), and most important, how to run AI models and AI apps completely independently, and in full isolation from my Linux laptops installed packages, file sytems, etc. outside the /ai per model or per project restrict space.

### Structure

In the structure below, one app is a two-container app that launchs "eventdb-api" to receive external events from virtually any system that reaches its API, and "eventdb-dashboard" to allows one to visualized with Agentic AI generated Charting features, Playground (i.e., blend different data sets to see results), and Analyics (advanced data manipulation, derivatives, etc. - complete with generating new CSV or JSON event repositories, onetime or on a schedule, etc. 

a. project 1:  accept real time events from external systems and view in dashboard.  This project will use either a 2-container or 3-container design.  The core will be an API applicaiton and its data storage backend; and a real time dashboard updated by changes to the backend. The 2nd design features a "eventdb-gateway" container if you wish to have a robust secured web front-end that can be hosted remotely or locally to avoid direct access to the api and dashboard instances. 

b. project 2:  maintain a set of panels to organize categories and related links per category. In theory, this can be your default home page on all different browsers on any platform you used, and avoids having links saved to the private history section within each browser. Currently this is a single container design.  I access it via "dashboard.local:<port> supported by a local /etc/hosts entry for the container's IP and "dashboard.local"

```text
+---- /ai/
     |---- models/
     |     |----- model1/
     |     |----- model2/
     |
     |---- projects/
     |     |----- eventdb-api
     |     |----- eventdb-dashboard
     |     |----- link-dashboard
     |     |----- <... other apps ...>
     |
     |---- .secrets/
           |----- .link-neondb.secret
           |----- .eventdb-supabase.secret
           |----- <... other secrets ...>

```
### Non-Privileged users

For **AI models**, this documentation will use "**llmuser**" to launch a model in container.  The goal is to test the model, for a finite period of time, and keeping track of prompts and exporting (downloading results).   I don't keep any of the AI models running across reboots. 

For **AI projects**, while they are apps, I use "**ai**" as non-root user to launch all the folders that make up a project in each of their own containers.  Within the container, after the project folder's content is copied to /opt in its container, any "secrets" (.env, .env.local, etc.) are created in the container's /opt folder.  Next, any frontend (like node.js) and dependencies are installed, and the project is started.  Each project (api, dashboard, gateway) are started in its dedicated port.


## Summary

The rest of this repository will have LXC ideas and working projecs so if interested, you can adopt this '*model*' as your own. 
