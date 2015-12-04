# Deployment of TYPO3 CMS using Magallanes with Rsync

This project shows how a TYPO3 CMS (7 LTS) installation can be deployed by using Magallanes.

**Be aware that the deployment tasks are not yet finished**

*Requirements:*
- ssh
- ssh-keys to your host(s)

## Getting started

### Installation 

Start by cloning this project and a simple `composer install` and you will get a basic TYPO3 CMS installation (which is not yet runnable)
 
*todo:* describe how to get it running

### Setup deployment

The directory `.mage/config/` contains all information for your deployments:

The file `environment/dev.yml` defines the deployment to your dev environment:
 
```
deployment:
  strategy: rsync
  user: root
  from: ./.mage/artifact/
  to: /var/www/deploy
  document-root: '/var/www/html'
releases:
  enabled: true
  max: 5
  symlink: current
  directory: releases
hosts:
  - "your-url.com"
  pre-deploy:
    - typo3-artifact:
        excludes:
          - /fileadmin
          - /uploads
  on-deploy:
  post-release:
    - clear-op-cache: {frontend-url: 'https://your-url.com'}
  post-deploy:
```

Some words on the configuration you **must** change to your specific setup:
- deployment.user: Define the user which is used for calling all remote bash calls
- deployment.to: Define a directory which is used to deploy the project on your host(s).
- deployment.document-root: your document root
- hosts: URL to your host, connection will be done with ssh keys.

### Start deployment

By calling  `./bin/mage deploy to:dev` in your command line, you will deploy to your **dev** configuration.
