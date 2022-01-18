# Settings for better Firefox performance in Fedora

## Enable vaapi support for video acceleration
* Enter `about:config`
* Set to **true** `media.ffmpeg.vaapi-drm-display.enabled`
* Set to **true** `media.ffmpeg.vaapi.enabled`

## Fix for crackling sound in sites like Udemy
* Enter `about:config`
* Set to **500** `media.eme.max-throughput-ms`
