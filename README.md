# Liquibase Modernization Workshop

This workshop teaches you how to bring databases operations to your regular CI\CD pipeline and implement continuos delivery for databases using [Liquibase](https://www.liquibase.com/)

# Description

In this workshop, you will gain hands-on experience deploying database changes using Liquibase Pro. 

Liquibase Pro is a database DevOps solution that allows organizations to accelerate, secure, observe, and govern their database change pipelines. By completing this workshop, you’ll learn how to establish automated database CI/CD pipelines that monitor and re-enforce data governance policies and allow your developers to shift left by catching errors before they cause slowdowns or deployment failures. You’ll also learn how to add data governance with Quality Checks, a feature of Liquibase Pro that allows you to identify changes that could present undesired risk or practices that violate data governance policy or organization standards.

 ## Types of Events
 
 - Self-paced
 - AWS hosted

### Target Audience

This workshop will help those who design and develop CI\CD pipelines, developer platforms, and DevOps architecture. In addition, this workshop will also benefit those who develop, deploy, and review database scripts as well as those who manage database instances, oversee data governance and compliance and looking to learn how to apply guardrails to database changes.

### Learning Objectives

* What is database DevOps (or DataOps) and why is it important for accelerating software delivery.
* What Liquibase is, and what benefits does it offer.
* How to implement Liquibase Pro in your CI\CD pipelines with an eye towards standardization for all teams as well as capture logs for observability.
* How to structure an application repository with database change management automation in mind.
* How to communicate the value of Liquibase and database DevOps to your organization.

## Building the Website

This site is built with Hugo, so you'll need it [installed](https://gohugo.io/getting-started/quick-start/#step-1-install-hugo)

First, clone this repo:

```bash
git clone git@github.com:aws-samples/aws-modernization-with-liquibase.git
```

Ensure you've also cloned the submodules:

```bash
git submodule init
git submodule update
```

Then serve the website with Hugo:

```bash
hugo server
```

## Authors

Contributors names and contact info

* Marina Novikova (@mariswa)
* TBD

## License

This project is licensed. See the LICENSE.md file for details

## Acknowledgments

* Markdown cheat sheet (https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* Learn theme markdown (https://learn.netlify.app/en/cont/markdown/)
* Menu extras and shortcuts (https://learn.netlify.app/en/cont/menushortcuts/) 
* Using Font Awesome Emoji's to help your page pop (https://learn.netlify.app/en/cont/icons/)