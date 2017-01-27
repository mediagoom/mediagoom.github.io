---
layout: post
title:  "How-to automate a @GitHub release using @travisci and @appveyor"
date:   2017-01-31 13:29:16 +0100
categories: mg
#excerpt: How to automate a release process using a version tag and builds from travisci and appveyour.
comments: true
---

[tr]: https://travis-ci.org/  "@travisci"
[apv]: https://www.appveyor.com/ "@appveyor" 

[apv-enc]: ../assets/appveyour-encrypt-data.png  "appveyour encrypt data"
[copy-token]: ../assets/github-copy-token.png  "copy-token"
[token]: ../assets/github-token.png "token"


No matter how good you are at writing software, sooner or later you will make a mistake.

Once you realize this as a fact then you have to get organized for when something bad happen.

Whatever your strategy is, one really important aspect is:  
how you can correlate software version the user is using with the source code from which it originated?

Well the answer is easy isn't it? just use a **git tag** and write the same version in your compiled software. 
Perfect. That's exactly what we do. We have just automated it so we can never forget.  

<!--excerpt-->

If you are interested in what we have done just read on.  


Well first of all, you need a Github project. You should already have automated the build with [@travisci] [tr] for Linux/mac builds and [@appveyor] [apv] for Windows builds. 
We will not enter into the details of setting up automated builds or automated tests however in case you think this should be an interesting topic just let us know. 
So you have your builds succeeding and passing all test and you are ready to release, and now what? 
Now you need to create a tag, pass that tag to your source code and then have the output of the build linked back to the Github tag as a Release. 

Let's view each steps in details.

## Passing the tag to the code.
Both [@travisci] [tr] and [@appveyor] [apv] have environment variables containing an incremental build number as well as the tag (if set). So you need to grab the variable value and write it somewhere in your code before the build begin. In this way in the future you will be able to know witch build produced your binary. For instance you can have a version function, as we have, and you can retrieve the version with the command line **--version**. 

In order to achieve this task, we have used python. Of course You can use anything you like. However, if you want to write it just once you need to pick up something that work well in both Linux and Windows.  
Following is the pseudo python code that we have used.

```python

import os

def version_file():
        file = os.path.dirname(os.path.realpath(__file__))
        file = os.path.join(file, 'src', 'mgcli', 'version.h')
        return file

def write_file(path, cnt):
        with open(path, 'w') as f:
            f.write(cnt)
            

def get_file_content(path, linex):
        i = 1
        ctx = ''
        with open(path, 'r') as f:
                for line in f:
                        if i != 3:
                            ctx += line
                        else:
                            ctx += linex
                            ctx += '\n' #os.linesep
                        i += 1
        return ctx

line='#define VERSION _T("'
v=version_file()
appv=os.environ.get('APPVEYOR_REPO_TAG')

appbuild=os.getenv('APPVEYOR_BUILD_VERSION', 'nn.nn.nn')
appm=os.getenv('APPVEYOR_REPO_COMMIT_MESSAGE', '===')
appb=os.getenv('APPVEYOR_REPO_BRANCH', '---')
apptag=os.getenv('APPVEYOR_REPO_TAG_NAME', 'no tag')


trevistag=os.environ.get('TRAVIS_TAG')

trbn=os.getenv('TRAVIS_BUILD_NUMBER', 'xx')
trc='TRAVIS'
trb=os.environ.get('TRAVIS_BRANCH')

trevis=False

if trb != None:
        trevis=True

lversion=os.environ.get('VERSION')
if None == lversion:
        lversion='0.0.x'
if None == appv:
        #not in appveyor
        if trevis:
                if trevistag != '':
                        line += trevistag
                else:
                        line += trbn
                line += ' ['
                line += trc
                line += ' on: '
                line += trb
                line += ']'
        else:
                 line += lversion
                 line += ' [local] '

else:
        if('false' == appv):
            line += appbuild 
        else:
            line += apptag
        line += ' ['
        line += appm
        line += ' '
        line += appb
        line += ']'

line += '")'

cnt = get_file_content(v, line)
print cnt
write_file(v, cnt)
```

To start you need to understand in witch environment you are. Either @appveyour or [@travisci] [tr].  
Once you know where you are you need to understand whether you are building a tag or not. If you are not building a tag you are not releasing, so you can either do nothing or just use the incremental build number to just be able to distinguish each build in case you are going to do further testing.  
In case of a tag build, you are releasing so you need to insert the tag in your source code. In our case we use a very simple approach: we have a header file with a function and we write the full file before the build.  
When the build finish both @appveyour and [@travisci] [tr] can be configured to upload the result of your build to Github releases. In order to have both of them adding binaries to the same release you have to specify the *same release name*. We simply use the tag as the release name.


## Appveyour
In order to achieve this in @appveyour we have the following configuration:

```yaml
deploy:
    # Deploy to GitHub Releases
  - provider: GitHub
    release: $(appveyor_repo_tag_name)
    artifact:  win-release.zip    #this should be the same as a path in artifact
    draft: true
    prerelease: true
    auth_token:
        secure: 9yjv88hRjZHOLHgfKr8Z93OYU1Rlz+JSaYxZ8gJlF77DopqJpeyqo5hpL95Aiwnf
    on:
     appveyor_repo_tag: true       # deploy on tag push only

```

You can use the code above as is just changing the artifact to be uploaded and the secure code.
In the artifact you must put a file produced in your build. Be sure your artifact is referenced in your artifact session with the same path:


```yaml

#---------------------------------#
#      artifacts configuration    #
#---------------------------------#

artifacts:
        - path: '.\test\bin\Win32\Release\mg.exe'
          name: mg.exe
        - path: '.\test\bin\Win32\Release\mg.pdb'
          name: mg.pdb
        - path: win-release.zip    #<----Same name
          name: win-release
```

To get your Github token got to https://github.com/settings/tokens. Once there hit *Generate new token*, 
Insert a description such as Appveyour and then select the first checkbox *repo*. 
Hit *Generate token*. 

![Generate token] [token]


Now you must copy the token witch appear on the screen.

![Copy token] [copy-token]


Now that you have your token head back to your [@appveyor] [apv] account and in the right corner hit your username and then select *Encrypt data*. 
In the *value to encrypt* box paste your token and then hit *Encrypt*. 

![appveyor encrypt] [apv-enc]


There you go. You can copy your encrypted token and enter it in your appveyor.yml file.

Now next time you will tag your code a new Github Release will be generated and the builded artifact will be uploaded to the release from [@appveyor] [apv].

## travisci

In order to start with the Github release integration in [@travisci] [tr] the easiest way is to install their client: [https://github.com/travis-ci/travis.rb] (https://github.com/travis-ci/travis.rb#installation) 
After installation login:

```travis login```

Then you can encrypt the key you have obtained previously using the command

```travis encrypt <your key>```

Copy the key to your .travis.yml file.
Your deploy section should look something like:

```yaml
deploy:
   provider: releases
   api_key:
     secure: "g6a6Z5/NHxSrUtcQix/Vbr3A5BYV2YOu
   file: out/Release/mg
   skip_cleanup: true
   on:
     repo: mediagoom/mg
     branch: master
     tags: true
```

Beside your *secure key* you should set your *repo* and the *file* to upload to the GitHub Releases.
This is it. Next tag build in @trevisci will upload the file to the same Github release created by @appveyour if you have @appveyour or will create a new one in case you just have @trevisci.

From now on every tag will automatically create a Release with artifact from your builds.



