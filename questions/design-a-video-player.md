# Design a Video Player

A video player loads a video, plays it without stalling on a changing network, and gives the user controls for play, seek, volume, captions, and quality. The question tests knowledge of how video is delivered in segments, and whether the candidate can build custom controls that remain accessible.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| Recorded video, live streams, or both? | Live adds latency targets and a moving timeline |
| How long are videos: 30 seconds or 2 hours? | Decides buffering and resume behavior |
| Are captions, multiple audio tracks, and speed control required? | Each is a separate track or setting |
| Does playback need content protection? | Adds a license request before playback |
| Is the player embedded on other sites? | Favors an iframe with a message API |

Quality targets to state: playback starts within 2 seconds, rebuffering stays under 1 percent of watch time, and every control works with a keyboard.

## Sketch

| Part | Responsibility |
|---|---|
| `<video>` element | Decodes and displays frames |
| Streaming engine | Fetches the manifest and segments, selects quality, fills the buffer |
| Controls | Play, seek bar, volume, captions, quality, full screen |
| Player state | `status`, `currentTime`, `duration`, `buffered`, `quality`, `volume` |
| Analytics | Reports start time, stalls, and quality changes |

## The decisions that matter

### 1. Adaptive bitrate streaming

A single video file forces one quality on every viewer. Adaptive streaming solves this. The server encodes the video at several qualities, for example 360p, 720p, and 1080p, and cuts each into segments of 2 to 6 seconds. A manifest file lists the qualities and the segment URLs. HLS and DASH are the two common formats for this.

The player downloads the manifest, then requests segments one at a time. Before each request it selects a quality from two signals: the measured download speed and the amount of video already buffered. When the network slows, the next segment arrives at a lower quality, and playback continues without a stall.

Media Source Extensions is the browser API that lets JavaScript pass these segments to the `<video>` element.

### 2. Buffering policy

| Setting | A typical value | Reason |
|---|---|---|
| Start quality | Low or medium | A fast start matters more than a sharp first segment |
| Buffer ahead | 30 seconds | Enough to absorb a network drop without wasting data |
| Buffer behind | 10 to 30 seconds | Allows short backward seeks; older data is released |
| Quality increases | Gradual, after sustained speed | Avoids switching back and forth |

### 3. Seeking

A seek to a position outside the buffer discards the pending requests and fetches the segment that contains the target time. Thumbnail previews on the seek bar come from a sprite image, which is one file holding many small frames.

### 4. Custom controls

Native controls are accessible but hard to style, so most products build their own. Each control is a real `<button>` with a label. The seek bar is a slider with `aria-valuetext` such as "2 minutes 10 seconds of 8 minutes". Space toggles play, the arrow keys seek by 5 seconds, and M mutes.

## Optimizations to mention

- Show a poster image at once, and load the streaming engine only when playback is likely.
- Preload only metadata until the user shows intent, to save data on phones.
- Pause when the player leaves the visible area, if the product allows it.
- Save the position every few seconds, so playback resumes on another device.
- Serve segments from a CDN with long cache lifetimes.

## What the interviewer probes

- Why does autoplay fail? Browsers block playback with sound until the user interacts; muted autoplay is allowed.
- How are captions synchronized? A text track holds cues with start and end times, and the browser shows them.
- What is measured? Time to first frame, stall count and duration, average quality, and errors.
- How does live differ? The manifest refreshes continuously, and the player stays a few segments behind the newest one.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-video-player) has a complete video streaming site case study.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-video-player) covers the encoding and delivery backend.
- Related pages: [accessibility](../concepts/accessibility.md), [performance and Core Web Vitals](../concepts/performance-and-core-web-vitals.md).
- Free article: [Designing a Scalable Video Streaming Platform Like YouTube and Netflix](https://www.designgurus.io/blog/design-video-streaming-platform?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-video-player), the encoding and delivery system behind the player.
