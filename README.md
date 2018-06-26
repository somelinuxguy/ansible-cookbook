# ansible-cookbook
A collection of playbooks, and roles that I find very useful. I tend to need these no matter where I go, and I wager you might too.

I hope these help you as much as they help me.

## Task: playbook-task-aws.yml
### What it do:
This playbook will build an EC2 server, then run a script on it.
### Requirements
* Ansible 2.2+
* Python2
* Boto2 Libraries
* AWS Keys for boto
* OR an appropriate IAM role for the machine running this playbook (outside the scope of this doc, learn this on your own please)
### Directions:
This should just run with `ansible-playbook playbook-task-aws.yml` but you'll want to edit it first. Note that my AMI is fake, use a real one. Note that my subnet is also fake, make sure you use a real one.

It is outside the scope of this doc to tell you what an AMI and Subnet is, or where to find their IDs.

When the machine comes up, we assume that you have "baked in" a script to your image, so that Ansible can run it for you. This could not be a script, it could be a role that does any number of things. I'm just using this sarcastic little script here to show you what you COULD do.

You can tweak tags and grouping too. I like to tag all my machines, it makes management much easier. Use them!

## Role: FakeCapistrano
### What it do:
This is a role, so you would apply it to multiple machines. It is intended to be included in a playbook.

It mimics the set up of a Capistrano (thats Ruby if you weren't aware) deployment. I very much like the scheme of a Capistrano deployment, but I very much dislike that it requires a lot of Ruby and is hard to maintain.

Ansible is just YML. Heck, my mom could read this and know what it says with NotePad. So why not use the same tool that deploys your machine, to also deploy the code? Seems silly to deploy with tool A then have tool A tell tool B to go run. Just do both. No blocking, no interprocess hackery, no muss.

Read more about Capistrano at an internet near you.
### Requirements
* Ansible 2.2+
* Python2
* a server to target

### Directions:
You probably have a playbook vaguely looking like this:
```
---
- hosts: tag_Env_testing
  remote_user: ansible
  become: yes
  become_method: sudo

  roles:
    - os.common
    - app.common
    - app.deploy
```

If you check out this role, it would be app.deploy in this stack, because it sets up your Capistrano-style directories and does Capistrano-style code checkout and post-checkout configurations.

Side note: Use good hierarchy in your role names. I'm a code hacker so I like the "dot notation" because it helps me visual that os is a class, and common is a method in that class, hence os.common is my role name.
