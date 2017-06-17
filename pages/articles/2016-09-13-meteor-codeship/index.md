---
title: Meteor + Codeship
date: "2016-09-13T22:40:32.169Z"
layout: post
path: "/2016/09/13/Meteor-Codeship/"
category: "Meteor"
description: "In my last Article I wrote about acceptance tests in Meteor.
                We could run those test periodically locally e.g. before every push to VCS.
                Much better solution is to use some Continuous Integration (CI) server. CI can build our app,
                install dependencies and run tests after each push to VCS."
---

In my last Article I wrote about acceptance tests in [Meteor](https://www.meteor.com/). We could run those test periodically locally e.g. before every push to VCS.
Much better solution is to use some Continuous Integration (CI) server. CI can build our app, install dependencies and run tests after each push to VCS.

My favourite CI server right now is [Codeship](codeship.com). They have great [UX](https://en.wikipedia.org/wiki/User_experience_design) and offer free plan for Bitbucket.
At Codeship you have to set commands which install your app and then run your tests. This is done trough *project settings* at Codeship website.

## Setup commands
This section should install anything which is needed to run your app and tests.

```bash
# Install node
nvm install 4.0
# Install npm packages
npm install -g chimp coffee-script spacejam@1.6.2-rc.1

# Install meteor
curl -o meteor_install_script.sh https://install.meteor.com/
chmod +x meteor_install_script.sh
# get sudo for Meteor install script 
sed -i "s/type sudo >\/dev\/null 2>&1/\ false /g" meteor_install_script.sh
./meteor_install_script.sh
export PATH=$PATH:~/.meteor/
meteor --version

# Install any other app depenedcies
# clone of your app at this location
cd ~/clone/ && npm install
```

## Test commands

```bash
    cd ~/clone
    
    # run unit tests
    spacejam test --driver-package=practicalmeteor:mocha-console-runner
    
    # run full app integration tests 
    spacejam test --full-app --driver-package=practicalmeteor:mocha-console-runner
    
    # start your in background 
    nohup bash -c "meteor --settings=settings.json 2>&1 &" && sleep 3m; cat nohup.out
    # then Chimp will use that instance and run acceptance tests
    chimp --ddp=http://localhost:3000 --mocha --path=chimp/tests
```

At line 4 and 6 use [spacejam](https://www.npmjs.com/package/spacejam) to run meteor test in command line interface.
At line 10 run Meteor at [background](https://documentation.codeship.com/continuous-integration/run-a-command-in-the-background/).