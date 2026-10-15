---
name: slopcannon
description: Get started with SLOPCANNON (slop.cc). Connect your API token and make your first AI music video.
---

# /slopcannon

Getting-started entrypoint for SLOPCANNON. For real production work, follow the `slopcannon-music-video` skill.

## Usage

```
/slopcannon                      # guided setup + first video
/slopcannon <natural language>   # jump straight to a request
```

## Examples

```
/slopcannon Make a 30s music video for this song: a lonely robot finds a dance floor, 9:16
/slopcannon How much would a 2-minute video cost?
/slopcannon Show my projects and credit balance
/slopcannon Continue my last project and reroll scene 3
```

## Guided first run

1. **Connect.** Call `slopcannon_capabilities`. If it fails with an auth error:
   - Have the user sign in at https://slop.cc/account and create an API token.
   - Have them paste it into the plugin's `SLOPCANNON_API_TOKEN` setting (or the environment variable of the same name) and reconnect.
   - **Never** ask them to paste the token into chat, and never echo or log it.
2. **Check readiness and balance.** Summarize what the manifest says is ready and how many credits are available. Pricing: 8 credits per second, 40-credit minimum per generation, scenes are 5 to 15 s, and each quote covers one scene. A 30 s video is about 240 credits before surcharges.
3. **Brief.** Ask for the song (MP3/WAV), the concept, and reference images for any recurring character. For a first video, suggest a short track or a 20 to 40 s excerpt.
4. **Rights.** Show SLOPCANNON's rights statement verbatim with the Terms/AUP links and the files to upload. Wait for the owner's explicit yes. Without file access, use `slopcannon_create_upload_link` and send them to the upload page.
5. **Plan.** Create a fresh project, analyze the song, and propose a scene plan (beats, cast, timing). Get the user's okay.
6. **Quote and scene 1.** Quote scene 1, show the total, get approval, generate it, and have the user review the take.
7. **Finish.** Quote and approve each remaining scene, generate, let the user pick takes, assemble, and deliver the download link.

Never publish or share the result unless the user explicitly asks.
