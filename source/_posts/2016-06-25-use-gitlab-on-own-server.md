---
title: Install GitLab on Your Own Server Instead of GitHub!
tags:
  - Git
  - VPS
  - Safety
  - Effectiveness
  - Website
  - Software
id: '1731'
categories:
  - - Development
date: 2016-06-25 21:12:08
languages:
  zh-CN: https://guozeyu.com/2016/06/use-gitlab-on-own-server/
---

Most of the code, configuration files, etc. deployed on my server are version controlled using Git. In order to be more convenient to use and configure, a whole system is usually used for management. Obviously, in some code and configuration files, there will be some confidential content, such as some keys, so it must not be disclosed. Although GitHub.com provides the Private storage function, but because this function is paid, and it is very expensive for the Organization Plan, it is not very cost-effective; even if there is a free Private storage, you can store many of your important keys. It is still very insecure to put it on a third-party server, so it is a good choice to be able to host on your own host and to replace the software/service of GitHub.com. This article will talk about the pits I encountered when installing GitLab on my own server, and advanced use, including using the `.gitlab-ci.yml` file to achieve automatic build and real-time synchronization of mirroring to GitHub.
<!-- more -->

There are actually many software/services that can host on their own servers, such as GitHub Enterprise, Bitbucket Server. However, we still recommend the GitLab Community Edition, which is completely open source, free, and maintained by the community. There are no restrictions, but it has fewer features than the Enterprise Edition that should not be used in the first place.

## Installation and Pits Encountered

The specific installation method [see the document](https://about.gitlab.com/downloads/), the currently officially recommended system environment is Ubuntu 16.04 LTS, which is very easy to install, and the entire web environment will be configured. For more configuration after installation, please [see documentation](http://docs.gitlab.com/omnibus/). If there is more than one web program running on your host, you need to modify the existing web software, and you need to refer to the official Nginx configuration documentation. I use `sub_filter` in my code to replace the default title for better SEO and more branding. Then in order to achieve better use effect, you should also configure the SMTP outgoing server, I am using AWS SES; then I also need a receiving server that supports IMAP to achieve Reply by email, I am using Gmail, the limit of receiving emails There are fewer restrictions than sending emails~ The specific setting methods of these are available in the official documents. After installation, registration is allowed by default. If you do not want outsiders to register, you need to go directly to the web background to disable it. If you want to open registration, then it is best to think about what new registered users can do, for example, like me: only allow new users to create Issues and Snippets, then set the Default projects limit to `0` in the web background, and edit The configuration file in the background prohibits new users from creating groups. It is also recommended to enable reCAPTCHA and Akismet in the web background to prevent malicious registration and malicious issuance of Issues. Since registration is allowed, it is also recommended to [use OmniAuth](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/doc/integration/omniauth.md) to support third-party OAuth login.

## GitLab Runner

[GitLab Runner](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner) is very powerful, but it is not built-in. It can be extremely convenient to implement very useful functions such as automatic deployment. After installing and configuring the Runner, add a file named `.gitlab-ci.yml` in the project root directory. Taking the master branch as an example, in order to achieve every commit to master, the file is deployed to `/var/gitlab/ myapp` , then the file content should look like this:

````
pages:
stage: deploy
script:
- mkdir -p /var/gitlab/myapp
- git --work-tree=/var/gitlab/myapp checkout -f
only:
- master
````

Note that you need to create the `/var/gitlab` folder first, and set the user group of this folder to `gitlab-runner:gitlab-runner`

````
$ sudo chown -R gitlab-runner:gitlab-runner /var/gitlab
````

The core part of `.gitlab-ci.yml` is `script:` , the scripts here are all executed by the user `gitlab-runner`, you can modify it according to your needs, and several examples are also given later. Then commit, go to the settings page to activate the Runner of this project. It is recommended to set Builds to `git clone` instead of `git fetch` in the settings, because the latter often has strange problems, and the speed bottleneck of the former is mainly network transmission.

### Deploy the Runner on the Same Host, Or Not?

The official documentation strongly discourages deploying Runners on the same host, but this statement is not correct. This is not officially recommended because some builds take a long time and take up a lot of CPU and memory resources. But if the build script you're executing doesn't do this, it's okay to install on the same host.

### Common Deployment Examples

These kinds of deployments are the ones I use more often, and you can use them as examples to make various deployments according to your own needs. The following web deployment methods do not consume much system resources, and because of the use of `nice`, they will not block other tasks and can be deployed on the same host.

#### Jekyll

Modify the `git checkout` line of the previous `.gitlab-ci.yml` file and replace it with:

````
jekyll build --incremental -d /var/gitlab/myapp
````

#### Check PHP for Compilation Errors

You can also add the following code to `.gitlab-ci.yml` to automatically check the compilation errors of all PHP files. The compiled files will not be displayed, only the compilation errors will be displayed:

````
if find . -type f -name "*.php" -exec nice php -l {} \; grep -v "No syntax errors"; then false; else echo "No syntax errors"; fi
````

#### Automatically Sync with GitHub

The following procedure requires root access to the host, or adding `sudo` before each command line. First, you need to give the `gitlab-runner` user a separate SSH Key:

````
$ ssh-keygen -f /home/gitlab-runner/.ssh/id_rsa
````

Then, create `/home/gitlab-runner/.ssh/known_hosts` with:

````
github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa + PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31 / yMf + Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB + weqqUUmpaaasXVal72J + UX2B + 2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi / w4yCE6gbODqnTWlg7 + wC604ydGXA8VJiS5ap43JXiUFFAaQ ==
````

After that, get the contents of `/home/gitlab-runner/.ssh/id_rsa.pub` file, [Add this SSH Key on GitHub](https://github.com/settings/keys). Since you are using the root account, don't forget to modify the user group after you are done:

````
$ sudo chown -R gitlab-runner:gitlab-runner /home/gitlab-runner/.ssh
````

Then, also through `.gitlab-ci.yml` To achieve automatic synchronization:

````
git push --force --mirror git@github.com:[Organization]/[Project].git
````

Just change `[Organization]` and `[Project]` to your own names.

## Talk About the Benefits of Installing GitLab on Your Own Server

All files are stored in their own server, which is more secure. They have the highest authority and will not encounter the situation that the project is deleted. During deployment, the delay is extremely low, and the reliability is also high. You will not encounter the dilemma that your own server has no problem but the third-party service is down and cannot be deployed. It can be deployed to the nearest server or internal server according to the situation. For example, the server of GitHub is located on the east coast of the United States. The connection in Asia is not fast, and the country is not stable. The most important thing is that if you already have a VPS or something, and you have a lot of free time, then you can get private storage for free, but [pay attention to performance requirements](http://docs.gitlab.com/ ce/install/requirements.html#hardware-requirements), do not enable if there is not enough free time. Since it can configure real-time synchronization mirroring to GitHub, GitLab has so many functions that GitHub does not have. In fact, GitLab can be fully used as the main version control tool, and GitHub just saves a mirror for backup.
