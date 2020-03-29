# Auto-archive experiment

## What is this
It would be useful to me (and lots of others) if it were easy to integrate a 'notify the internet archive I've published a new post so I can be sure it lives on despite all kinds of uncertainties - described in much more detail in [Making Sure My Content Lives On](https://bkardell.com/blog/ArchivingByDefault.html).  This repo documents some (hopfully) easy/flexible solution to this problem ..

## How it works...
At a low level, you send an HTTP Post to a service at `https://dawn-rain-4cff.bkardell.workers.dev/`, with the content-type `application/json` and provide some stuff in the body.  This lets the Internet Archive know that you've published something new and requests that it be snapshotted and archived.

### What stuff do I put in the body?
It depends!

### Why?!
Because the aim is to make integration easy as possible with existing setups and low-friction despite wild variance in how people do things.

Depending on how much control you have, and what your setup is, different things will work for you.  Personally, I build my site via a (custom) static site generator  and 
host my domain (https://bkardell.com) on GitHub pages.  For me, integration with GitHub Pages (option 1 below) is the lowest friction solution - it takes about 5 minutes to setup.  Your mileage will vary though depending on lots of details about your setup, so I'd like to give you easy options too.

Generally speaking, there are two "ways" this is done - one is by you directly providing a URL to request be snapshotted. This is great if you can plug into it easily.  The other is to point to your RSS feed where the newest item will be requested. There are also some easy integration options for your existing setup...

### Option 1: GitHub Pages integration
This is in some ways the most complex, but actually the most convenient if you use GitHub Pages.  It works by plugging into  webhooks - and I _think_ everything just snaps into the right place and time. It's always based off you RSS feed, so you won't accidentally request an archive for a thing that doesn't exist yet, or something that's not ready yet.  So... Simple, actually. 

Check out the [setup for github pages integration](github-pages-webhooks.md)

## Options 2: Wordpress integration
Super easy and convenient setup for [integration with Wordpress](wordpress-integration.md) contributed by @stuartlangridge

## Option 3: RSS 'Touch'
If option 1 isn't convenient for you, but you have some way to know that something new has published, you can simply toss the URL of your RSS feed at it when that happens and it will request snapshotting of the most recent item.

Just `POST` directly to `https://dawn-rain-4cff.bkardell.workers.dev/`. Make sure you set the `'content-type': 'application/json'` header and send a body containing JSON with the field `feedLocator`  ala   
  
```json  
{ "feedLocator": "https://bkardell.com/blog/feed.rss"}
```


## Option 4: Just the URL
Or, if you have a lot of control and are totally comfortable/know exactly what changed and when - you can call it however you like (even CURL) and provide it the URL to the new content directly...

Just `POST` directly to `https://dawn-rain-4cff.bkardell.workers.dev/`. Make sure you set the `'content-type': 'application/json'` header and send a body containing 
with the field `snapshotURL`  ala   
  
```json  
{ "snapshotURL": "https://bkardell.com/blog/TowardResponsive.html"}
```

Note though, if you're opting for this level, it seems that you can/probably have been able to do this by simply constructing a GET request yourself already that appends the URL to the `/save` path, so this potentially doesn't buy you a whole lot beyond a potentially simpler, slightly more uniform surface... And maybe provides me an opportunity to centralize some discussion.

## Can I help?
Defintely!

1. Let me know what you think about the idea, try it out
2. If you set it up for some popular system, let me know how and we'll add some documentation here
3. Let me know how it could be better!
4. Tell someone else?
