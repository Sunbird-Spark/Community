# Configuration Options

#### ECAR generation (default: enabled)

ECAR files are content bundles used by the offline mobile app and desktop app. If your deployment does not support offline learning, you can disable ECAR generation to reduce publish processing time and storage usage.

```
ECAR_GENERATION_ENABLED=false
```

#### Video streaming (default: disabled)

Adaptive bitrate streaming (HLS) provides a better video experience for learners on variable internet connections (the video quality adjusts dynamically based on available bandwidth). Enable it if your learner base frequently accesses video on slow or variable connections.

```
VIDEO_STREAMING_ENABLED=true
```

{% hint style="info" %}
&#x20;Enabling video streaming requires additional cloud infrastructure (a video transcoding pipeline). The installer configures this automatically when VIDEO\_STREAMING\_ENABLED=true.
{% endhint %}

<br>
