# YouTube Video Performance Monitor (Make + Discord)

An automated pipeline built in **Make** that checks a YouTube channel's videos, uses AI to classify how each one is performing based on its views, and sends **Discord** alerts so the team knows which videos need attention.

**Stack:** Make · YouTube Data API v3 · Make AI Toolkit · Discord

![Make scenario](https://github.com/iqra-khan740/Youtube-video-performance-monitor/blob/main/youtube-video-performance-monitor/docs/screenshots/make-scenario.png)

## How it works

| Step | Module | What it does |
|------|--------|--------------|
| 1 | **HTTP: Make a request (GET)** | Calls the YouTube Data API v3 search endpoint (`/youtube/v3/search?channelId=...`) to list the channel's videos |
| 2 | **Iterator** | Splits the response into one bundle per video |
| 3 | **HTTP: Make a request (GET)** | Fetches each video's details, including its view count |
| 4 | **Filter** | Only videos that meet the views-count condition continue |
| 5 | **Make AI Toolkit: Simple Text Prompt** | AI classifies each video's performance (for example *Underperforming*) |
| 6 | **Router** | Sends each video down one of four routes according to its classification |
| 7 | **Discord: Send a Message** | Posts the alert to Discord |

In the run shown above, 10 videos passed through the filter and the AI step; the router delivered 5 messages on the 1st route and 3 on the 4th.

## Example output

Videos classified as underperforming trigger an alert in the team's Discord channel with the title, the view count and the classification:

![Discord alerts](https://github.com/iqra-khan740/Youtube-video-performance-monitor/blob/main/youtube-video-performance-monitor/docs/screenshots/discord-alerts.jpeg)

```
⚠️ Video needs attention, please look into this:
Title: <video title>
Views: <view count>
Classified as: Underperforming
```

## Setup

1. Create a **YouTube Data API v3** key in Google Cloud Console.
2. In Make, build the scenario with the modules above and connect your **Discord** account and the **Make AI Toolkit**.
3. In the first HTTP module, set your `channelId` and API key.
4. Set the filter's views-count condition.
5. Edit the AI prompt so it returns one of your performance categories.
6. In each router route, set the condition for a category and choose the Discord channel for it.
7. Run once to test, then schedule the scenario.

## Customising

- Change the `channelId` to monitor another channel.
- Adjust the AI prompt and the router conditions to change the categories and when an alert fires.
- Add or remove router routes to send alerts to different Discord channels.
