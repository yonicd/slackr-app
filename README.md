# slackr-app

The Slack application `slackr` is an incoming webhook that is used with the `R` package [slackr](https://github.com/hrbrmstr/slackr) created by [Bob Rudis](https://github.com/hrbrmstr) to interact with Slack workplace channels.


## Usage

1. Click on the Button <a href="https://slack.com/oauth/authorize?client_id=220157155520.220159943344&scope=incoming-webhook,files:read,files:write:user,chat:write:bot,chat:write:user,mpim:write,mpim:read,mpim:history,im:write,im:read,im:history,groups:write,groups:read,groups:history,channels:write,channels:read,channels:history,emoji:read,usergroups:read,users:read" target="_blank"><img alt="Add to Slack" height="40" width="139" src="https://platform.slack-edge.com/img/add_to_slack.png" srcset="https://platform.slack-edge.com/img/add_to_slack.png 1x, https://platform.slack-edge.com/img/add_to_slack@2x.png 2x"></a>
2. Select the workplace to install the application
3. Select the default channel to post to (this can be changed later)
4. If successful a `SLACK_ID_KEY` will be return. Keep this key
5. If not successful an error message will be returned.
6. To keep the data safe you need your workplace `MEMBERID` to authenticate the `SLACK_ID_KEY`.
  - [Here are instructions how to locate your Member ID](https://medium.com/@moshfeu/how-to-find-my-member-id-in-slack-workspace-d4bba942e38c)
7. Use the following API to get the credentials you will need for the `slackr` `R` package. `https://slackr-auth.herokuapp.com/creds/<MEMBERID>/<SLACK_ID_KEY>`
8. A json will be returned with three elements
  - api_token: xoxp-XXXXXXXXXXXX-XXXXXXXXXXXX-XXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  - webhook_uri:  https://hooks.slack.com/services/XXXXX/XXXXX/XXXXX
  - channel: the channel you selected in step 3
  
From here we move to setup instructions for the [R package](https://github.com/hrbrmstr/slackr#setup)

The `slackr_setup()` function will try to read setup values from a `~/.slackr` (you can change the default) configuration file, which may be easier and more secure than passing them in manually (plus, will allow you to have multiple slackr configs for multiple Slack.com teams). The file is in Debian Control File (DCF) format since it really doesn't need to be JSON and R has a handy `read.dcf()` function since that's what `DESCRIPTION` files are coded in. Here's the basic format for the configuration file:

    api_token: xoxp-XXXXXXXXXXXX-XXXXXXXXXXXX-XXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    channel: #general
    incoming_webhook_url: https://hooks.slack.com/services/XXXXX/XXXXX/XXXXX

You can also change the default emoji icon (from the one you setup at integration creation time) with `icon_emoji`.

In the `R` session to load the credentials run the following
  
```r
library(slackr)
slackr_setup()
```

Send a message to test it all out

```r
slackr('Here I am')
```

Try another channel

```r
slackr('Here I am', channel='#random')
```

