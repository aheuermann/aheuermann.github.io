---
layout: post
title: Using Bower with Travis-CI
category: posts
type: text
---

I ran into a couple hang-ups while trying to get a client-side library running on Travis-CI.  Due to the nature of client-side dependency conflicts and a few quirks with bower, it ended up being a little more work than anticipated, but I managed to get it working.

The first issue I hit was with conflicting libraries. When running `bower install` from the command line, it will give you a choice of which version you would like to use. To get around this, you can use `bower install -f` which will force the more recent version, but if you want more control, you can also specify `resolutions` in your `bower.json`. 

```json
{
    "dependencies": {...}, 
    "resolutions": {
      "underscore": "~1.5.2"
    }
} 
```

Then I hit this issue.

`The authenticity of host 'github.com (192.30.252.129)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?`

Bower was trying to download dependencies over ssh/git and Travis does not have the Github RSA key fingerprint. NPM defaults to https so you never hit this (unless you manually specify someting like `git@github.com:jashkenas/underscore.git` in your `package.json`). To get around this you can turn off the check by placing `StrictHostKeyChecking no` in `~/.ssh/config`. So here is my working `.travis.yml`. 

```yaml
before_script:
  - npm install -g grunt-cli
  - npm install -g bower
  - echo -e "Host github.com\n\tStrictHostKeyChecking no\n" >> ~/.ssh/config
  - bower install -f

language: node_js
node_js:
    - "0.10"
```
