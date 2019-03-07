At OUT OF SCREEN we decided to use Rocket.chat instead of Slack in order to preserve ownership on our data.

It was quite easy to deploy on a cheap Digital Ocean droplet, https://rocket.chat/docs/installation/paas-deployments/digital-ocean/.

There is currently no out-of-the-box Heroku integration, so here is how to setup one:

In Rocket.chat follow Administration > Integrations > Create a new incoming integration and use the following for the script :

```
class Script {

  process_incoming_request({ request }) {
    // console is a global helper to improve debug
    // console.log(request.url.query);
    const query = request.url.query;

    return {
      content: {
        text: `${query.user} released in production ` +
        `version ${query.release} \n` +
        `commit ${query.head}: _"${query.git_log}"_`
       }
    };
  }
}
```


Then go to your Heroku app, and add a resource 'HTTP Deploy hook', in which you copy the new webhook URL.

🎉 It's done!
You will now receive nice looking notifications in your Rocket.chat rooms:

![](/images/rocket-chat-heroku)
