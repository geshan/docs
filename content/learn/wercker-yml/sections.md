## Sections

As mentioned in the introduction, the wercker.yml has different
sections that each have a specific purpose. We will go over each
individual section in this part.

### Box

The box section allows you to choose a
[container](/learn/containers/introduction.html) which will be used to run
the builds and deploys. This item will contain a single reference to the box.
The box will be prefixed by the owner and it can be post-fixed with a `":"`
followed by a tag. If no tag is given, then the tag "latest" will be used.
If no user is specified, default containers from the [Docker
Hub](/learn/containers/docker-hub.html) will be used.

```yaml
box: ruby
```

It is also possible to reference a box based on an `id`. This is
convenient and more clear when using different container per
[pipeline](/learn/pipelines/introduction.html) and as such it doesn't
make sense to have a *top-level* box definition. You specify an `id` as
follows:

```yaml
box:
    id: nodesource/wheezy
```

It is also to specify environment variables that your container might
need:

```yaml
box:
    id: nodesource/wheezy
    env:
        SOME_ENV_VAR: foo
```


### Services

The services section allow you to specify supporting boxes, like databases or
queue servers. This item should contain an array of supporting boxes. The
reference will be the same as to a main box. So it will be prefixed and can
contain a tag.

```yaml
services:
    - mongodb
    - redis
```

This will load two services, `mongodb` and `redis`, both default Docker
containers from Docker Hub, and both using the latest versions.

Similar to non-service containers, you might require environment variables
which you need to inject inside the container. This could be for
instance a `username/password` combination. You can use the `env`
clause to do exactly that:

```yaml
box:
    id: mies/rethinkdb
    env:
        USERNAME: foo
        PASSWORD: bar
```

- - -
> Want to learn more? Read more about this on the
> [docs](/docs/services/index.html)


### Dev

The `dev` section sets up your local development pipeline. It ensures that
the local development environment is executed in the same way and with the same
containers that are set up in the wercker
[cloud environment](/learn/build/pulling-builds.html).
With a `development` pipeline you can also run and test your applications locally.

In order to use the the development pipeline, incorporate the following in your
`wercker.yml` file.

```yaml
dev:
  steps:
    - npm-install
    - internal/watch:
        code: node app.js
        reload: true
```

- - -
> Want to learn more? Read more about this on the
> [docs](/docs/using-the-cli/local-development.html)

### Build

The `build` section will contain all the configuration information for the build
[pipeline](/learn/pipelines/introduction.html).

```yaml
build:
    steps:
        # Execute the bundle install step, a step provided by wercker
        - bundle-install
        # Execute a custom script step.
        - script:
            name: middleman build
            code: bundle exec middleman build --verbose
```

Two types of [steps](/learn/steps/introduction.html) are defined in this
build section. First a `bundle-install` step that installs the Rubygem
dependencies. This step is available from the
[step registry](/learn/steps/step-registry.html). The second step in an inline
script that in this case compiles our static site.

### Deploy

The `deploy` section will contain all the configuration information for the deploy
[pipeline](/learn/pipelines/introduction.html).

```yaml
deploy:
    steps:
        # Execute the s3sync deploy step, a step provided by wercker
        - s3sync:
            key_id: $AWS_ACCESS_KEY_ID
            key_secret: $AWS_SECRET_ACCESS_KEY
            bucket_url: $AWS_BUCKET_URL
            source_dir: build/
    # notify slack on successful or failed deploys
    after-steps:
        - kobim/slack-post:
            url: $SLACK_URL
            channel: notifications
            username: werckerbot
```

Here we see the Amazon Web Services S3 synchronization step that can
sync static assets to S3. Environment variables are used that hold the
correct credentials. The values of these environment variables are
specified through the [wercker web interface](/learn/pipelines/using-env-vars.html).

Next, we have an [after-step](/learn/steps/after-steps.html) that notifies a Slack chat room of either
passed or failed deploys in a [Slack](http://slack.com) chat room. The `$SLACK_URL` environment variables holds
the webhook url it should post the notification to.

We've covered a lot of ground on this page but fortunately there are
dedicated sections for topics such as [pipelines](/learn/pipelines/introduction.html), [containers](/learn/containers/introduction.html) and
[steps](/learn/steps/introduction.html).

[&lsaquo; Introduction](/learn/wercker-yml/introduction.html "nav previous yml")
[Environment variables &rsaquo;](/learn/wercker-yml/environment-variables.html "nav next yml")
