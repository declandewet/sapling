# Sapling

A Slack bot for initiating new projects.

## README-DRIVEN PHASE

This project is in the "readme" phase of [readme-driven-development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html), and is not currently in a usable state - as much of the internals are not yet written. Have patience :smile:.

## Hypotheticals

**NOTE**: Each step for creating a project is separated out into phases. Phases can be initiated independently, but each phase with the exception of _phase 1_ will require an _existing_ project. The way the proposed setup works is more akin to a command line interface, however, as this project matures, these phases will be melded together in continuation, providing a more conversational & interactive interface. This is just early stages, so I'm focusing on minimum-viability rather than a completely fleshed out experience. That'll come later.

### Hypothetical Phase 1: Starting a New Project

This phase creates a new repository on Github and grants relevant members write access to the repo.
By default, a project will be initiated with a `master` branch and a `develop` branch - the `develop` branch will be made the default branch for the repository. This will eventually be configurable.

> **declan**: @sap init private carrot/fruit-basket w/ @jescalan @nimit @kylemac
>
> **sap**: Ok, I will add you, @jescalan, @nimit and @kylemac to `https://github.com/carrot/fruit-basket`.
>
> **sap**: Your project's **private** repository has been created: https://github.com/carrot/fruit-basket.



### Hypothetical Phase 2: Generating boilerplate code

Tell Sapling what [Sprout](https://github.com/carrot/sprout) template you would like to form the foundation of your project, and he will cycle through the template's questions and push the generated code to the existing repository respective to the answers you provided.

> **declan**: @sap sprout init spike carrot/fruit-basket
>
> **sap**: Ok, I will generate a **spike** template for **carrot/fruit-basket**. First though, some questions...
>
> _sap will now ask the questions configured in the **spike** template..._
>
> _…some time later…_
>
> **sap**: Great! Your repository has been updated: https://github.com/carrot/fruit-basket



### Hypothetical Phase 3: Setting Up 10kft

Tell Sapling whether or not you want to create a project on 10kft for this project. Sapling will report back with a project code and add the configured team members to the project.

> **Author Note:** I'm not yet entirely clued up on the specifics/procedures for project creation on 10kft so this section is a work-in-progress and will likely be handled last, once all the other features are implemented.



### Hypothetical Phase 4: Setting up a hosting provider (currently limited to Netlify)

Tell Sapling which hosting provider you would like to use to host the project. If staging is required, Sapling will assume that the `develop` branch is where the staging environment will live, although this will eventually be configurable. If the configured users have Netlify accounts, they will be given push access.

> **declan**: @sap set host carrot/fruit-basket netlify
>
> **sap**: Ok, I will host branch `master` of **carrot/fruit-basket** on **Netlify** as `fruit-basket-production.netlify.com`. Would you also like me to create a staging environment?
>
> **declan**: yes
>
> **sap**: Ok, I will host branch `develop` of **carrot/fruit-basket** on **Netlify** as `fruit-basket-staging.netlify.com`.
>
> **sap**: Your project is now hosted:
>
> > **Staging**                                                 **Production**
> >
> > [fruit-basket-staging.netlify.com](#)        [fruit-basket-production.netlify.com](#)



### Hypothetical Phase 5: Generating Slack Channels

Sapling will generate Slack channels for the given project. If configured in an earlier phase, the channel description will include the 10kft project code.

> **declan**: @sap open channel carrot/fruit-basket
>
> **sap**: Ok, I will open a new channel called **fruit-basket** and invite you, @jescalan, @nimit and @kylemac. Would you also like me to create an internal channel?
>
> **declan**: yes
>
> **sap**: Ok, I will open a new channel called **fruit-basket-internal** and invite you, @jescalan, @nimit and @kylemac.
>
> **sap**: Channels were created.



### Hypothetical Phase 6: Email Notification

Tell Sapling to email the configured users, as well as a list of configured users who represent upper management, that a project has officially started. If an email address is not configured for a user, it will be inferred from the Slack API.

> **declan**: @sap notify carrot/fruit-basket
>
> **sap**: Ok, I will notify @jescalan, @nimit, @kylemac, @tom and @tony that work on **carrot/fruit-basket** has officially started.
>
> **sap**: Email sent.



## FAQs

- **How does Sapling know how to map a Slack user to their Github / 10kft / Netlify / etc. profile?**
  There are three ways of doing this:

  - Teach Sapling in conversation:

    > **declan**: @sap map user @nimit github:nimitzshah email:<redacted>
    >
    > **sap**: Ok - I will link @nimit to the Github profile nimitzshah

  - configure this info in `users.yml`:

    ```yaml
    <slack user id>:
      github: <github handle>
      email: # if not supplied, will be inferred from slack
    ```

  - provide a function that returns a promise for an array of objects of this shape - useful if your company maintains an internal API where all this data is already aggregated:

    ```js
    sapling.getUsers = function () {
      return fetch('https://api.com/users')
        .then((response) => response.json())
        .then((users) => {
          return users.map((user) => {
            return {
              slack: 'slack id',
              github: 'github handle',
              netlify: 'netlify username',
              '10000ft': '10kft username'
            }
          })
        })
    }
    ```

    ​
