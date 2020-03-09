# Auto-archive experiment

## What is this
It is an experimental and usable solution to the problem laid out in [Making Sure My Content Lives On](https://bkardell.com/blog/ArchivingByDefault.html).
If you're unfamilliar with the problem, go have a read - if you just want to see how to use it, this is the URL for you...


## How it works...
At a low level, you send an HTTP Post to a service at `https://dawn-rain-4cff.bkardell.workers.dev/`, with the content-type `application/json` and provide some stuff in the body.  This lets the Internet Archive know that you've published something new and requests that it be snapshotted and archived.

### What stuff do I put in the body?
It depends!

### Why?!
Because the aim is to make integration easy as possible with existing setups and low-friction despite wild variance in how people do things.

Depending on how much control you have, and what your setup is, different things will work for you.  Personally, I build my site via a (custom) static site generator  and 
host my domain (https://bkardell.com) on GitHub pages.  For me, integration with GitHub Pages (option 1 below) is the lowest friction solution - it takes about 5 minutes to setup.  Your mileage will vary, and but I'd like to give you easy options.

Generally speaking, there are two "ways" this is done - one is by you directly providing a URL to request be snapshotted. This is great if you can plug into it easily.  The other is to point to your RSS feed where the newest item will be requested.

### Option 1: GitHub Pages integration
This is in some ways the most complex, but actually the most convenient if you use GitHub Pages.  It works by plugging into  webhooks...

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
  "rss.json": "https://raw.githubusercontent.com/bkardell/bkardell.github.io/master/blog/feed.json"   
}
```

Cool - that's it.  When a 'publish' happens, it will notify the service, look for this file, find your RSS feed (currenly only the JSON version, but if this goes anywhere I'll expand it) and request a snapshot of the most recent item in it.

## Option 2: RSS 'Touch'
If option 1 isn't convenient for you, but you have some way to know that something new has published, you can simply toss the URL of your RSS feed at it when that happens and it will request snapshotting of the most recent item.

Just `POST` directly to `https://dawn-rain-4cff.bkardell.workers.dev/`. Make sure you set the `'content-type': 'application/json'` header and send a body containing JSON with the field `feedLocator`  ala   
  
```json  
{ "feedLocator": "https://bkardell.com/blog/feed.json"}
```


## Option 3: Just the URL
Or, if you have a lot of control and are totally comfortable/know exactly what changed and when - you can call it however you like (even CURL) and provide it the URL to the new content directly...

Just `POST` directly to `https://dawn-rain-4cff.bkardell.workers.dev/`. Make sure you set the `'content-type': 'application/json'` header and send a body containing JSON with the field `snapshotURL`  ala   
  
```json  
{ "snapshotURL": "https://bkardell.com/blog/TowardResponsive.html"}
```


## Can I make help?
Yeah...

1. Let me know what you think about the idea, try it out
2. If you set it up for some popular system, let me know how and we'll add some documentation here
3. Let me know how it could be better!
4. Tell someone else?
