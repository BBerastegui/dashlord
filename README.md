# DashLord (translated into English automatically)

Dashboard of technical best practices

Data acquisition and report generation are automated by [GitHub actions](https://github.com/features/actions)

Examples:

- https://dashlord.incubator.net
- https://socialgouv.github.io/dashlord-fabrique
- https://mtes-mct.github.io/dashlord
- https://socialgouv.github.io/dnum-dashboard

## Use

To deploy your version of DashLord:

- Create a new repository [**from the dashlord template**](https://github.com/SocialGouv/dashlord)
- Edit the `dashlord.yml` file
- Edit the `.github/workflows/scans.yml` file if necessary
- Edit the `.github/workflows/report.yml` file if necessary (check the `base-path` where the website will be published, it will be the name of the repository)
- Launch `DashLord scans` in the `Actions` tab of your GitHub project

Once the scans are complete, a report will be generated in the `gh-pages` branch of the repository. You need to go to the `Settings` tab of the repository to enable the "GitHub Pages" feature and choose the `gh-pages` source. This publishes the report to `https://[organisation].github.io/[repository]` (publicly).

### GitHub actions

- The `DashLord scans` workflow allows you to launch a scan on URLs, it is executed when there is a change in the `dashlord.yml` file
- The `DashLord report` workflow is launched at the end of each `DashLord scans` and produces the report as a website.

These workflows can also be triggered manually in the "Actions" tab

## Customization

- The [`dashlord.yml`](./dashlord.yml) file allows you to configure the urls and some dashboard options
- The workflow [`.github/workflows/scans.yml`](./.github/workflows/scans.yml) allows you to customize certain scanners, and set the scan frequency (parameter `schedule` set by default every Sunday at midnight)
- The workflow [`.github/workflows/report.yml`](./.github/workflows/report.yml) generates the web report based on [SocialGouv/dashlord-actions/report](https:/ /github.com/SocialGouv/dashlord-actions).

### dashlord.yml

💡 Good practice: remove slashes at the end of urls

```yml
title: Dashboard title
description: Good technical practices
entity: Social Ministries
footer: Powered by SocialGouv
# `tools` allows to activate only some of the tools in the report
tools:
   404: true
   screenshot: true
   nmap: true
   zap: true
   wappalyzer: true
   http:true
   testssl: true
   lighthouse: true
   thirdparties: true
   nucleus: false
   updownio: true
   dependabot: true
   scancode: true
   statistics: true
   declaration-a11y: true
   trivy: true
   ecoindex: true
   sonarcloud: true
URLs:
   - url: https://www.free.fr
     title: Homepage free.fr
     tags:
       - telecom
       -provider
     repositories: # to retrieve security alerts from these repositories
       - free/free-ui
       - free/free-css
     docker: # to scan images with trivy
       - ghcr.io/socialgouv/fabrique/frontend
       - ghcr.io/socialgouv/factory/backend
     tools: # to disable some tools
       nmap: false
     pages: # to run lighthouse on additional pages
       - /profile
       - /mentions
   - url: https://www.lemonde.fr
     title: Homepage lemonde.fr
     tags:
       - press
```

### Availability Metrics

DashLord can monitor the level of performance and availability of your applications. (set up = 10mins)

- Create an account on [updown.io](https://updown.io)
- Add the urls to monitor (as defined in dashlord.yml)
- Activate the tool with `updownio: true` in the dashlord.yml file
- Add your "readonly" updown.io API key in a GitHub secret named `UPDOWNIO_API_KEY` (settings/secrets tab)

▶ On next scan, updown.io information will be uploaded to DashLord

## Tools

Each tool can be enabled/disabled in the report with the `tools` key from dashlord.yml.

| repos | descend |
| -------------------------------------------------- --------------------------------------- | -------------------------------------- |
| [SocialGouv/dashlord-actions](https://github.com/SocialGouv/dashlord-actions) | Dashlord specific actions |
| [SocialGouv/dashlord-nuclei-action](https://github.com/SocialGouv/dashlord-nuclei-action) | Dump nuclei result |
| [SocialGouv/httpobs-action](https://github.com/SocialGouv/httpobs-action) | Dump Mozilla HTTP Observatory result |
| [SocialGouv/thirdparties-action](https://github.com/SocialGouv/thirdparties-action) | Dump third party scripts scan result |
| [SocialGouv/wappalyzer-action](https://github.com/SocialGouv/wappalyzer-action) | Dump Wappalyzer scan result |
| [MTES-MCT/dependabotalerts-action](https://github.com/MTES-MCT/dependabotalerts-action)
