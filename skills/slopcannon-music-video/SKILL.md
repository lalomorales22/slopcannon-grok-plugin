---
name: slopcannon-music-video
description: Direct and produce an AI music video with SLOPCANNON (slop.cc) from a song, optional named cast references, and a concept: rights confirmation, song analysis, timed scene plan, quotes and spend approval, first-scene review, generation, take selection, and assembly. Use when the user asks to make, plan, price, continue, or assemble a SLOPCANNON / slop.cc music video.
---

# SLOPCANNON music video

> Not for: general questions about AI video that need no SLOPCANNON account data. Not for: publishing or posting the finished video anywhere.

You are the director; SLOPCANNON is the execution harness. The official, always-current version of this workflow lives at `https://slop.cc/.well-known/skills/slopcannon-music-video/SKILL.md`. Where it and this file disagree, the official skill and the live capability manifest win.

## Before you start

1. The SLOPCANNON MCP server (`https://api.slop.cc/mcp`) is connected with the user's token. If a tool returns an auth error, ask the user to check `SLOPCANNON_API_TOKEN` in the plugin settings (tokens are created at `https://slop.cc/account`). Never ask for the token in chat; never print, log, echo, or pass it as a command-line flag.
2. Call `slopcannon_capabilities` (or read `https://api.slop.cc/v1/capabilities`). Treat it as authoritative for limits, prices, and readiness, and use the absolute URLs in its `endpoints` object. Never invent endpoints or tool names. Never claim a job was submitted without a returned job ID.
3. If a readiness flag the plan needs (`hostedRuntime.submissionReady`, server scene-audio slicing, `contract.assembly.serverReady`, ...) is false, preserve state and report the missing capability before spending.

## Pricing facts (verify against the live manifest)

- **8 credits per second** of generated video, **40-credit minimum** per generation.
- Scenes are **5 to 15 seconds** (`contract.generation.sceneDurationMs`).
- **One scene per quote**: every scene is its own approval. Don't add a separate paid test clip; the minimum makes a short probe cost about as much as a real scene, so scene 1 is the test.
- Server assembly is included; it has no separate charge.

## 1. Brief

Require one MP3 or WAV song and a concept or emotional/comedic arc. Require a named reference image for any character who must stay recognizable. Ask only about choices that materially change the result (aspect ratio, who sings which part, ending, content limits, budget cap). Infer the ordinary directing choices yourself.

## 2. Rights gate (the human owner must give it)

Before any song, reference, or derived scene-audio file is uploaded:

- Fetch the current rights statement, version, and Terms / Acceptable Use links from the platform's pre-upload discovery. Do not send media just to discover them.
- Show the statement **verbatim** with the links, list the exact files, and state the purpose (processing and combining them into the user's video).
- Wait for the human owner's own explicit yes ("I agree" or an unambiguous instruction to proceed). Owning the file, staying silent, a previous session, or a general "make a video" request is **not** assent. Never invent, infer, or auto-tick it.
- One sign-off covers that statement version for the session. Ask again if `requiredVersion` changes, a different person directs the work, or the human withdraws it.
- With no shell or file access, call `slopcannon_create_upload_link` and give the human the `uploadPageUrl`. They confirm and upload there. Then read the `assetId` with `slopcannon_get_upload_link`.

## 3. Clean project and analysis

1. `slopcannon_create_project` for each new video. Don't reuse earlier cast, prompts, or plans unless the user asks to continue that exact project. Attach references with `slopcannon_add_reference` and give each one an unambiguous name.
2. Get the exact decoded song duration from server-side preparation.
3. When vocals matter, get lawful lyrics and word timing (`slopcannon_start_song_analysis` / `slopcannon_get_song_analysis` when ready). Never write lyrics yourself.
4. Build a cue sheet with absolute times and one singer per phrase, plus a fixed singer-to-speaker map (for example `LEAD=S1, FRIEND=S2`) that never changes later.
5. Choose scene boundaries between phrases, 5 to 15 s each, and never cut through a word. Treat fast 1 to 3 s cuts as shots inside one scene.

## 4. Direct the full arc

Write a short production bible covering visual language, an identity lock for each character, who sings what, continuity anchors, forbidden drift (text, logos, motifs), and the beginning, climax, and final image. Give each scene its own story job and restate the cast and identity locks inside it. For visible singing, give the exact local onset (`localMs = wordOnsetMs - scene.startMs`) with the singer and speaker ID, e.g. `At 00:02.180, LEAD (S1) sings, [English]Exact lyric.`, and say which characters keep their mouths closed. Use `slopcannon_direct_scenes` / `slopcannon_plan_cuts` where they help.

## 5. Scene audio

Upload the master once (after rights assent). Request server slicing for all the scene windows (`slopcannon_slice_scene_audio`, then `slopcannon_get_scene_audio`). If slicing isn't available, stop before paid generation. Don't create a separate project for each scene.

## 6. Spend gate (always)

1. `slopcannon_create_quote` for the scene: show the model, resolution, billable seconds, reference surcharges, expiry, and total credits.
2. Get explicit approval for that exact amount, or a standing hard cap that covers it.
3. **Generate scene 1 only.** Have the user review identity, motion, who's singing, and mouth timing on the delivered take before you submit anything else.
4. Every later quote needs its own approval or must fit inside an approved cap. Rerolls are new paid work.
5. If the balance reports `scope: "organization-customer"`, its `availableCredits` is the real limit. Never ask for a ceiling to be raised on the user's behalf.

## 7. Generate, review, assemble

1. After scene 1 is approved, submit the remaining approved scenes within the provider's concurrency limit (`slopcannon_submit_generation`, `slopcannon_get_generation`). Recover uncertain submissions with `slopcannon_recover_generation`; never send duplicates.
2. Check each take for identity, the right singer, continuity, anatomy, and stray text or logos. Rerolls are for failed scenes only, with approval.
3. Record which take the user picked with `slopcannon_get_workspace` and `slopcannon_select_take`, passing the revision exactly as read. On a conflict, re-read the workspace. Don't force the write. Pick only takes the user chose. Delete (`slopcannon_delete_take`) only what the user discards; deletion is permanent and refunds nothing.
4. `slopcannon_start_assembly` (with `clips` omitted, it uses the current selections), then `slopcannon_get_assembly` and `slopcannon_assembly_download_links`. The master song plays untrimmed from time 0, and generated clip audio is dropped.
5. Check the receipt, `syncVerified`, the byte count and digest, and actually watch the result.

## Never

- Publish, post, or share the video (no `slopcannon_create_publish_link` unless the user explicitly asks for that exact action).
- Print, log, or request tokens in chat.
- Spend without a shown quote and explicit approval.
- Call a preview, pending job, or unverified assembly "finished".

## Deliver

The MP4 or official download link, the credits actually spent and how many rerolls, any remaining visual or lyric weaknesses, and the project and selected-take state so the user can keep editing.
