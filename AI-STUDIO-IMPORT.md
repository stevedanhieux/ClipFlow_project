# Import ClipFlow updates into Google AI Studio

This guide assumes you mean Google AI Studio (aistudio.google.com), rather than the AI Suite tab inside ClipFlow.

## Option 1 — update your existing app through GitHub (recommended)

1. Download clipflow-ai-studio-source.zip and extract it.
2. Back up/export your current AI Studio app before replacing files.
3. Copy the extracted files into the root of your app's linked GitHub repository. package.json and src/ belong at the repository root, not inside an extra clipflow/ folder. Keep unrelated files and review conflicts. Never upload API keys, .env, node_modules/, or data/.
4. Commit and push the update.
5. Open your existing AI Studio app, then Settings → GitHub, and pull the repository changes. Review any conflicts.
6. Paste the text from ai-studio-import-prompt.txt into the app's Build chat. Add provider credentials through server-side Secrets when needed.

## Option 2 — import as a new AI Studio app

1. Extract clipflow-ai-studio-source.zip into a new GitHub repository. A private repository is preferable if the code is private. Put package.json at the repository root.
2. Open Google AI Studio Build mode.
3. In the prompt input, choose Add files (+) → Import from GitHub and select that repository.
4. Paste ai-studio-import-prompt.txt and check the resulting preview.

The official guide documents GitHub import and two-way sync here:
https://ai.google.dev/gemini-api/docs/aistudio-build-mode

## Option 3 — replace changed files directly in the Code tab

Use clipflow-ai-studio-changed-files.zip when you want to retain the existing app without using GitHub. Extract it, then replace each file at the same relative path in AI Studio's Code tab. Create the new files listed in UPDATE-MANIFEST.txt. This package contains complete file contents, not snippets. Use the source files as the authority; do not ask the AI to recreate the implementation from a feature summary.

For a local Git checkout that still matches the original clipflow.zip, clipflow-ai-studio.patch is another way to apply the update before pushing. Run `git apply --check clipflow-ai-studio.patch`, then `git apply clipflow-ai-studio.patch` in the repository root. If it reports conflicts, review them instead of forcing an overwrite. A patch must be applied in a local checkout; attaching it in a chat does not itself apply it.

I have verified the official GitHub import route. I have not verified direct ZIP-to-project import in your AI Studio account. These ZIPs package files for extraction and repository upload or manual replacement; simply attaching a ZIP to a prompt is not a guaranteed project import.

## What is adapted in this source package

All ClipFlow code updates are included. This hosted package differs from the local package only in two defaults: the server listens on 0.0.0.0 so a hosted preview can reach it, and the director initially selects Gemini instead of local Ollama. API keys are not included. There is no new public deployment or account connection.

## Features that need a hosted runtime check

- FFmpeg/FFprobe must be installed in the server environment for MP4 rendering. Importing JavaScript source does not install those programs. Canvas slide export remains browser-based.
- yt-dlp must be installed for social downloads. Platforms may block the hosting environment even when the code imports successfully.
- A hosted server's localhost address refers to that server. It cannot reach Ollama installed on your personal PC through http://127.0.0.1:11434. Use Gemini or another configured remote provider in AI Studio, or run ClipFlow locally for Ollama.
- Files under data/ need persistent storage for reliable hosted use. Preview/container storage may be temporary. Long video render jobs should use a persistent worker rather than assuming an interactive preview will stay alive for an hour.
- The current server is a development implementation without user accounts. Keep the imported app private; a public multi-user deployment needs authentication, storage isolation, and render quotas.

Local builds and integration/render tests passed. These packages have not been imported into your Google account or tested inside its managed preview. No GitHub repository has been created or updated for you.
