Early phase of development, only just working and not at a stage that is easy to follow so be warned.

Premise:

Use git to push projects on to your production server and have it checkout and auto-restart your application.

Uses forever to keep your application up.

Installation: (Rough steps for now)

1) Install gitosis (create a user 'git' with home located in '/home/git')

2) Install NodeJs

3) Rename '/usr/local/bin/gitosis-serve' to '/usr/local/bin/_gitosis-serve'

4) Copy forklift files to '/usr/local/bin/'

5) Add a 'test' project to 'gitosis-admin.git'

6) Push gitosis-admin.git to server

7) Create local 'test' project

8) Push project 'test' project

9) Due to bug yet to fix make another change to 'test' and push it again so forklift hooks can then checkout and start our project

How it works:

Gitosis makes use of git hooks to update the authorized users when changes are pushed to 'gitosis-admin.git'. In forklift we hijack this process by replacing gitosis-serve, providing the opportunity to run our own custom code instead, or if the command is git related passing it along to the re-named '_gitosis_serve'

Forklift provides a number of commands for start/stop/restart as well as listing application instances.

I'm actually using another git project called proxy.git to allow me to set ports for each subsequent application. This is responsible for picking up requests to port 80 and passing them on to correct port depending on the domain specified. I'm using bounce for this.

Process to add a new application:

1) Add application to gitosis-admin.git (push it)

2) Add application to proxy.git and define a domain name and port for it (push it)

3) Create a new project and push it to the git repo we just created in gitosis-admin.git. It is expected that the application has app.js at it's root. See https://github.com/jimib/nodejs-basejump for an accepted format of application.

3b) On the server the latest version of the application is checked out, "npm install -d" is run to install dependencies and you can at this point define scripts to have it run a series of tests.

4) Change it and push it again. (Temporary until bug with hooks is fixed)


Disclaimer:
As stated, this is an early draft of the project so it's extremely flakey, it's currently only deemed a proof of concept.