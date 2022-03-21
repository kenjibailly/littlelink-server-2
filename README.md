# 🔗 LittleLink-Server

This project is based on the great work from [littlelink](https://github.com/sethcottle/littlelink)
It takes the same simple approach to a link page and hosts it within a NodeJS server with React Server Side Rendering, containerized for you to use. Now, customizing `LittleLink` with `littlelink-server` is as easy as passing in some environment variables.  If you need help configuring this, please see [this video that explains everything](https://www.youtube.com/watch?v=42SqfI_AjXU) and a live example at [technotim.live](https://technotim.live/).

## 🚀 Getting Started

## 📍 Supported Links & Buttons & Config

Check the [config](https://github.com/techno-tim/littlelink-server/blob/master/src/config.js) file for all supported buttons and configuration!

## 📈 Analytics Support

### Google Analytics

See [Getting Started with Analytics](https://support.google.com/analytics/answer/1008015?hl=en).  After getting your GA Tracking Id, use your tracking Id as environment variable like `GA_TRACKING_ID=G-XXXXXXXXXX`  (See the example below)

All buttons clicked will be tracked automatically if `GA_TRACKING_ID` exists.

Sample event for YouTube button.

```javascript
  window.gtag('event', 'youtube-button');
```

### Umami

See [Adding a website & Collecting data](https://umami.is/docs/collect-data) page to add and generate your tracking code.

Generated tracking code should look like:

```javascript
<script async defer data-website-id="00000000-1111-2222-3333-444444444444" src="https://your-umami-app.com/umami.js"></script>
```

Use `data-website-id` as environment variable `UMAMI_WEBSITE_ID` and `src` as `UMAMI_APP_URL`.

Sample event for YouTube button.

```javascript
  window.umami('youtube-button');
```

### Matomo 

See [Installing Matomo fo how to configure analytics](https://matomo.org/docs/installation/) and [how to find your site id](https://matomo.org/faq/general/faq_19212/)

Use `MATOMO_URL` for your URL and `MATOMO_SITE_ID` for your site id

Sample event for YouTube button.

```javascript
  window._paq.push(['trackEvent', 'youtube-button']]);
```

## 🩺 Health Check

A health check endpoint exists on `/healthcheck`.  If healthy, it will return with a `200` and the following response:

```json
{
  "status": "ok"
}
```

To skip express from logging these calls, add the environment variable:

```bash
SKIP_HEALTH_CHECK_LOGS=true
``` 

## 🐳 Docker

This container image is published on both [GitHub Container Registry](https://github.com/techno-tim/littlelink-server/pkgs/container/littlelink-server) and [DockerHub](https://hub.docker.com/repository/docker/timothystewart6/littlelink-server) choose whichever one works for you.  They will both be updated during CI.

The example below will generate a site exactly like <https://technotim.live>

`docker-compose.yml`

```yml
---
version: "3.0"
services:
  littlelink-server:
    image: ghcr.io/techno-tim/littlelink-server:latest
    # dockerhub is also supported timothystewart6/littlelink-server
    # image: timothystewart6/littlelink-server:latest
    container_name: littlelink-server
    environment:
      - META_TITLE=Techno Tim
      - META_DESCRIPTION=Software Engineer | Gamer | Twitch Streamer | Content Creator on YouTube | Homelab | 🇺🇸 🇯🇵  | Full Nerd
      - META_AUTHOR=Techno Tim
      - LANG=en
      - META_INDEX_STATUS=all
      - OG_SITE_NAME=Techno Tim
      - OG_TITLE=Techno Tim
      - OG_DESCRIPTION=The home of Techno Tim
      - OG_URL=https://technotim.live
      - OG_IMAGE=https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_400x400.jpg
      - OG_IMAGE_WIDTH=400
      - OG_IMAGE_HEIGHT=400
      - GA_TRACKING_ID=G-XXXXXXXXXX
      - THEME=Dark
      - FAVICON_URL=https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_200x200.jpg
      - AVATAR_URL=https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_200x200.jpg
      - AVATAR_2X_URL=https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_400x400.jpg
      - AVATAR_ALT=Techno Tim Profile Pic
      - NAME=TechnoTim
      - BIO=Hey! Just a place where you can connect with me!
      # use ENV variable names for order, listed buttons will be boosted to the top
      - BUTTON_ORDER=YOUTUBE,TWITCH,TWITTER,GITHUB,INSTAGRAM,DISCORD,FACEBOOK,TIKTOK,PATREON,GEAR,DOCUMENTATION
      # you can render an unlimited amount of custom buttons by adding 
      # the CUSTOM_BUTTON_* variables and by using a comma as a separator.
      - CUSTOM_BUTTON_TEXT=Documentation,Recommended Gear
      - CUSTOM_BUTTON_URL=https://l.technotim.live/docs,https://l.technotim.live/gear
      - CUSTOM_BUTTON_COLOR=#000000,#000000
      - CUSTOM_BUTTON_TEXT_COLOR=#ffffff,#ffffff
      - CUSTOM_BUTTON_ALT_TEXT=Tech documentation site for my videos and more,Recommended Gear
      - CUSTOM_BUTTON_NAME=DOCUMENTATION,GEAR
      - CUSTOM_BUTTON_ICON=fas file-alt,fas fa-cog
      - GITHUB=https://l.technotim.live/github
      - TWITTER=https://l.technotim.live/twitter
      - INSTAGRAM=https://l.technotim.live/instagram
      - YOUTUBE=https://l.technotim.live/subscribe
      - TWITCH=https://l.technotim.live/twitch
      - DISCORD=https://l.technotim.live/discord
      - TIKTOK=https://l.technotim.live/tiktok
      - FACEBOOK=https://l.technotim.live/facebook
      - PATREON=https://l.technotim.live/patreon
      - FOOTER=Techno Tim © 2022
    ports:
      - 8080:3000
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
```

Docker command

```bash
docker run -d \
  --name=littlelink-server \
  -p 8080:3000 \
  -e META_TITLE='Techno Tim' \
  -e META_DESCRIPTION='Techno Tim Link page' \
  -e META_AUTHOR='Techno Tim' \
  -e LANG=en \
  -e META_INDEX_STATUS='noindex' \
  -e THEME='Dark' \
  -e FAVICON_URL='https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_200x200.jpg' \
  -e AVATAR_URL='https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_200x200.jpg' \
  -e AVATAR_2X_URL='https://pbs.twimg.com/profile_images/1286144221217316864/qIAsKOpB_400x400.jpg' \
  -e AVATAR_ALT='Techno Tim Profile Pic' \
  -e NAME='TechnoTim' \
  -e BIO='Hey! Just a place where you can connect with me!' \
  -e GITHUB='https://l.technotim.live/github' \
  -e TWITTER='https://l.technotim.live/twitter' \
  -e INSTAGRAM='https://www.instagram.com/techno.tim' \
  -e YOUTUBE='https://l.technotim.live/subscribe' \
  -e TWITCH='https://l.technotim.live/twitch' \
  -e DISCORD='https://l.technotim.live/discord' \
  -e TIKTOK='https://l.technotim.live/discord' \
  -e KIT='https://l.technotim.live/gear' \
  -e FOOTER=Techno Tim © 2022 \
  --restart unless-stopped \
  ghcr.io/techno-tim/littlelink-server:latest
```

## ☸ Kubernetes

[Unofficial helm chart provided by k8s-at-home](https://github.com/k8s-at-home/charts/tree/master/charts/stable/littlelink-server)

```bash
helm repo add k8s-at-home https://k8s-at-home.com/charts/
helm repo update
helm install littlelink-server \
  --set env.TZ="America/New York" \
  --set env.META_TITLE="TechnoTim"
    k8s-at-home/littlelink-server
```

Or use a values.yaml files

`helm install littlelink-server k8s-at-home/littlelink-server -f values.yaml`

## 👇 What is LittleLink?

![LittleLink](https://cdn.cottle.cloud/littlelink/social-circle.png)

LittleLink is a lightweight DIY alternative to services like [Linktree](https://linktr.ee)
and [many.link](https://www.google.com). LittleLink was built using [Skeleton](http://getskeleton.com/), a dead simple, responsive boilerplate—we just stripped out some additional code you wouldn't need and added in branded styles for popular services. 😊

![Themes](https://cdn.cottle.cloud/littlelink/themes.png)

LittleLink has more than 20 company button styles you can use and we'll be throwing more in soon. You'll also find a light and dark theme ready to go. Not a fan of the colors? Update `skeleton-light.css` or `skeleton-dark.css` to the HEX values of your choosing.

![Performance](https://cdn.cottle.cloud/littlelink/performance.png)

Using [Skeleton](http://getskeleton.com/) let us build something that loads quickly & doesn't have any of the unnecessary bloat you would get from using a large framework for a page that requires nothing more than simplicity. LittleLink scored a 99/100 in performance when tested with [Google Lighthouse](https://developers.google.com/web/tools/lighthouse).
