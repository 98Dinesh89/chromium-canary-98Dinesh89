# Chromium Canary Submission — Dinesh (@98Dinesh89)

## 1. Loom Walkthrough Video
* **Video URL:** [Watch walkthrough](https://youtu.be/r-v6HoOxXdo)

## 2. Build Info
* **Chromium Commit ID:** `d343dda4dd95df6ecbfe8e00bf10fcda754d1e7e`
* **Target Built:** `content_shell`
* **My Laptop OS & RAM:** Windows 11 with WSL2 Ubuntu 26.04, 16 GB RAM

## 3. Architecture Notes
* **Feature A (`X-Candidate-Canary` header):** The browser process reads `--inject-candidate-id`. I added it to `kSwitchNames` in `render_process_host_impl.cc` so renderers receive it. A URLLoaderThrottle adds the header in the browser process (navigations) and in the renderer (subresources). The renderer one uses `cors_exempt_headers`, and the name is registered in `cors_exempt_header_list`, because a normal custom header there fails the debug CORS check. Scope: content_shell only; a chrome build would need the same hooks in chrome/.
* **Feature B (`[CANARY_ACK]`):** `console.cc` no longer exists in this Chromium version. Console API messages arrive in `FrameConsole::ReportMessageToClient` in `frame_console.cc`, in the Blink renderer. It logs the canary line when the source is the console API, the level is warning, and the text is exactly `canary-ping`.

## 4. AI Agent Workflow
* **AI Tool & Model Used:** Codex CLI (GPT-6 Luna).
* **How I Used Sub-Agents:** I ran separate read-only Codex sessions for (1) Blink console handling and (2) switch propagation and throttles, then used a separate coding session.
* **Other tools:** I also used a chat assistant (Claude) for planning, explanations and debugging build errors.
