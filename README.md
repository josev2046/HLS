## OVP HLS Player: A Cookie-Minimal Approach

This repository outlines a high-level approach for delivering Video-on-Demand (VoD) content in a web browser, prioritising minimal cookie imposition on the client-side. The solution leverages an Online Video Platform (OVP) as a content source and integrates an open-source HLS (HTTP Live Streaming) player, namely HLS.js, for robust playback capabilities.

## Challenge Overview: Minimising Client-Side Cookies

The conventional method for embedding OVP content typically involves integrating their standard player via an iframe. This approach inherently introduces various cookies related to player operation, security protocols, and analytics, which are often beyond the direct control of the embedding party.

This project addresses the challenge of reducing client-side cookie footprints by advocating for direct access to an OVP's HLS streams. While opting for direct HLS stream access substantially diminishes the number of cookies attributable to the player or direct embeds on the client-side, it is imperative to acknowledge that the Content Delivery Network (CDN) may still issue essential or security-related cookies as part of its server-side operations. Nevertheless, the primary focus of this methodology is to minimise those cookies directly influenced by the client-side player or embedded components.

## Technical Implementation

### HLS.js Integration

The HLS.js library, a JavaScript utility, does not inherently set or drop cookies for its core functionality. It facilitates HLS content playback directly within the browser by utilising HTML5 video elements in conjunction with MediaSource Extensions (MSE). This allows for dynamic streaming without reliance on proprietary player mechanisms that often introduce tracking or preference cookies.

A foundational example demonstrating HLS.js's capabilities can be observed by providing a direct HLS URL: [hls.js demo](hls.js demo)

### Dynamic HLS URL Retrieval from OVP (API Integration)

To enhance the flexibility and utility of the solution, the HLS URL is not hardcoded but is retrieved dynamically from the OVP. This necessitates a server-side component (or an equivalent secure mechanism for proof-of-concept) to manage API token security. For this demonstration, we will be leveraging the Vimeo API.

The relevant OVP API endpoint for M3U8 playback URLs is:

`GET https://api.vimeo.com/users/{user_id}/videos/{video_id}/m3u8_playback`

**Security Prerogative**: It is of paramount importance that OVP API tokens remain secure. The HLS URL, being a sensitive resource, must be provided by a backend service to prevent client-side exposure of authentication credentials. The provided HTML `index.html` demonstrates a direct client-side fetch for simplicity in a demonstration environment; however, for any production deployment, a secure backend proxy is unequivocally required.

### src/index.html

![image](https://github.com/user-attachments/assets/091d4717-afe3-41a7-9e50-d89a78ecdb93)


The core client-side application resides within `src/index.html`. This file contains the HTML structure, CSS for basic presentation, and JavaScript logic for:

* Accepting OVP Video ID and API Token inputs.
* Initiating an asynchronous request to the OVP API (Vimeo API for this demo) to fetch the HLS playback URL.
* Initialising and managing the HLS.js player instance.
* Handling various HLS.js and native video playback errors, including recovery attempts for fatal errors.
* Providing user feedback on the loading and playback status.
