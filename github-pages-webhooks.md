
### Setting up the hook itself
1. In the repo for your site, click 'settings'
2. From the menu on the left, choose 'Webhooks'
3. Click the 'Add webhook' button
4. Set the Payload URL to `https://dawn-rain-4cff.bkardell.workers.dev/`
5. Set the Content Type to `application/json`
6. Choose the `Let me select individual events.` radio button  
7. Check only the "Page builds" checkbox
8. Save the webhook

### RSS Integration
The webhook will let the worker know when your site is updated. However, because you can host on any domain we'll need a way to find this. To wit, in the root of your repo, add a file called `path.to.rss` which contains a URL where your rss feed can be found.  Here's what mine looks like  

```json
{   
  "rss": "https://raw.githubusercontent.com/bkardell/bkardell.github.io/master/blog/feed.rss"   
}
```

Cool - that's it.  When a 'publish' happens, it will notify the service, look for this file, find your RSS feed and request a snapshot of the most recent item in it.

(note: if you have a `feed.json` instead, you can use the property `rss.json` to point to that instead)
