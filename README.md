# Matchbot

Matchbot is a chat bot built on the [Hubot][hubot] framework. It was initially generated by [generator-hubot][generator-hubot], and configured to be deployed on [Heroku][heroku] to get you up and running as quick as possible.

He lives in the cloud at [https://matchbot.herokuapp.com/].

This README is intended to help get you started. Definitely update and improve to talk about your own instance, how to use and deploy, what functionality he has, etc!

[heroku]: http://www.heroku.com
[hubot]: http://hubot.github.com
[generator-hubot]: https://github.com/github/generator-hubot

### Running Matchbot Locally

You can test your hubot by running the following.

You can start spectheglobot locally by running:

    % bin/hubot --name matchbot

You'll see some start up output about where your scripts come from and a
prompt:

    [Sun, 04 Dec 2011 18:41:11 GMT] INFO Loading adapter shell
    [Sun, 04 Dec 2011 18:41:11 GMT] INFO Loading scripts from /home/tomb/Development/hubot/scripts
    [Sun, 04 Dec 2011 18:41:11 GMT] INFO Loading scripts from /home/tomb/Development/hubot/src/scripts
    Hubot>

Then you can interact with Spec by typing `spec help`.

    Hubot> matchbot help

    Hubot> animate me <query> - The same thing as `image me`, except adds a few
    convert me <expression> to <units> - Convert expression to given units.
    help - Displays all of the help commands that Hubot knows about.
    ...


### Scripting

An example script is included at `scripts/example.coffee`, so check it out to
get started, along with the [Scripting Guide](https://github.com/github/hubot/blob/master/docs/scripting.md).

For many common tasks, there's a good chance someone has already one to do just
the thing.

### hubot-scripts

There will inevitably be functionality that everyone will want. Instead
of writing it yourself, you can check
[hubot-scripts][hubot-scripts] for existing scripts.

To enable scripts from the hubot-scripts package, add the script name with
extension as a double quoted string to the `hubot-scripts.json` file in this
repo.

[hubot-scripts]: https://github.com/github/hubot-scripts

### external-scripts

Hubot is able to load scripts from third-party `npm` package. Check the package's documentation, but in general it is:

1. Add the packages as dependencies into your `package.json`
2. `npm install` to make sure those packages are installed
3. Add the package name to `external-scripts.json` as a double quoted string

You can review `external-scripts.json` to see what is included by default.

##  Persistence

If you are going to use the `hubot-redis-brain` package
(strongly suggested), you will need to add the Redis to Go addon on Heroku which requires a verified
account or you can create an account at [Redis to Go][redistogo] and manually
set the `REDISTOGO_URL` variable.

    % heroku config:add REDISTOGO_URL="..."

If you don't require any persistence feel free to remove the
`hubot-redis-brain` from `external-scripts.json` and you don't need to worry
about redis at all.

[redistogo]: https://redistogo.com/

## Adapters

Adapters are the interface to the service you want your hubot to run on. This
can be something like Campfire or IRC. In our case, we're using the [Slack adapter](https://github.com/tinyspeck/hubot-slack). There are a number of third party
adapters that the community have contributed. Check
[Hubot Adapters][hubot-adapters] for the available ones.

If you would like to run a non-Campfire or shell adapter you will need to add
the adapter package as a dependency to the `package.json` file in the
`dependencies` section.

Once you've added the dependency and run `npm install` to install it you can
then run hubot with the adapter.

    % bin/hubot -a <adapter>

Where `<adapter>` is the name of your adapter without the `hubot-` prefix.

[hubot-adapters]: https://github.com/github/hubot/blob/master/docs/adapters.md

## Deployment

    % heroku create heroku-app-name
    % git push heroku master

If your Heroku account has been verified you can run the following to enable
and add the Redis to Go addon to your app.

    % heroku addons:add redistogo:nano

If you run into any problems, checkout Heroku's [docs][heroku-node-docs].

You'll need to edit the `Procfile` to set the name of your hubot.

More detailed documentation can be found on the
[deploying hubot onto Heroku][deploy-heroku] wiki page.


## Environment Variables

Matchbot depends on a few local environment variables. You can find these below. In order to run locally with these variables, you'll need to create a `.env` file. See the example env.template for syntax. After adding your environment variables the `.env`, you'll need to run `source .env` to load them. This will need to occur before running `bin/hubot`.

    % HUBOT_HEROKU_KEEPALIVE_URL //Find more at https://github.com/hubot-scripts/hubot-heroku-keepalive

## Alright, brain overload already. I'm ready to play

Run the command below in the #general channel of [Slack](http://slack.com) to check it out. Note: The name 'spec' below matches whatever variable is set via the variable HUBOT\_SLACK\_BOTNAME.

```
matchbot hello
```

### Deploying to UNIX or Windows

If you would like to deploy to either a UNIX operating system or Windows.
Please check out the [deploying hubot onto UNIX][deploy-unix] and
[deploying hubot onto Windows][deploy-windows] wiki pages.

[heroku-node-docs]: http://devcenter.heroku.com/articles/node-js
[deploy-heroku]: https://github.com/github/hubot/blob/master/docs/deploying/heroku.md
[deploy-unix]: https://github.com/github/hubot/blob/master/docs/deploying/unix.md
[deploy-windows]: https://github.com/github/hubot/blob/master/docs/deploying/unix.md

## Slack Variables

Spec The Globot is currently connected to the [Slack adapter](https://github.com/tinyspeck/hubot-slack). 

This adapter uses the following environment variables:

#### HUBOT\_SLACK\_TOKEN

This is the service token you are given when you add Hubot to your Team Services.

#### HUBOT\_SLACK\_TEAM

This is your team's Slack subdomain. For example, if your team is `https://myteam.slack.com/`, you would enter `myteam` here.

#### HUBOT\_SLACK\_BOTNAME

Optional. What your Hubot is called on Slack. If you entered `slack-hubot` here, you would address your bot like `slack-hubot: help`. Otherwise, defaults to `slackbot`.

#### HUBOT\_SLACK\_CHANNELMODE

Optional. If you entered `blacklist`, Hubot will not post in the rooms specified by HUBOT_SLACK_CHANNELS, or alternately *only* in those rooms if `whitelist` is specified instead. Defaults to `blacklist`.

#### HUBOT\_SLACK\_CHANNELS

Optional. A comma-separated list of channels to either be blacklisted or whitelisted, depending on the value of HUBOT_SLACK_CHANNELMODE.

#### HUBOT\_SLACK\_LINK\_NAMES

Optional. By default, Slack will not linkify channel names (starting with a '#') and usernames (starting with an '@'). You can enable this behavior by setting HUBOT_SLACK_LINK_NAMES to 1. Otherwise, defaults to 0. See [Slack API : Message Formatting Docs](https://api.slack.com/docs/formatting) for more information.

## Under the Hood

#### Receiving Messages:

The slack adapter adds a path to the robot's router that will accept POST requests to:

`/hubot/slack-webhook`

Source: [https://github.com/tinyspeck/hubot-slack/blob/2.2.0/src/slack.coffee#L161-L177](https://github.com/tinyspeck/hubot-slack/blob/2.2.0/src/slack.coffee#L161-L177)

Expected parameters:

- text
- user_id
- user_name
- channel_id
- channel_name

If there is a message and it can deduce an author from those paramters, it'll create a new [TextMessage](https://github.com/github/hubot/blob/v2.7.2/src/message.coffee#L14) object and have the robot receive it, from there proceeding down the regular hubot path.

#### Sending Messages

When a script calls `send()` or `reply()` this adapter makes a POST request to your team's specific URL webhook:

`https://<your_team_name>.slack.com/services/hooks/hubot`

with a JSON-formatted body including the following dictionary:

- username
- channel
- text
- link_names (optionally)

#### Message to a specific room:

Sometime, it's useful to send a message regardless of the channel's activity (like `robot.hear` or `robot.response`). Hubot has [`robot.messageRoom`](https://github.com/github/hubot/blob/v2.8.0/src/robot.coffee#L401-L409) available for this use case.

Slack API uses channel ID's by default, which uses computer-friendly alphanumeric ID. To use the pretty names, prefix it with a hash.

```coffeescript
robot.respond /hello$/i, (msg) ->
  robot.messageRoom '#general', 'hello there'
``` 

## More Adapters

[hubot-adapters]: https://github.com/github/hubot/blob/master/docs/adapters.md

## Restart the bot

You may want to get comfortable with `heroku logs` and `heroku restart`
if you're having issues.


## Want to create your own?

Follow the instructions over at [Hubot-Slack](https://github.com/tinyspeck/hubot-slack/blob/master/README.md).