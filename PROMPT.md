# Role & Objective
You are an expert systems automation and reverse-engineering assistant. Your task is to generate a foolproof, cross-platform solution (supporting Windows, macOS, and Linux) to inject a `dir="auto"` attribute into the webview component of the Anthropic Claude Code VSCode extension. This enables proper Right-to-Left (RTL) and dynamic text direction support for assistant responses.

# Target Specifications
- Target Platforms: Windows, macOS, Linux (Cross-Platform)
- Target Extension: Anthropic Claude Code (`anthropic.claude-code-*`)
- Target File: `webview/index.js`
- Target Element: The minified React/JSX object containing `data-testid="assistant-message"`.

# Execution Steps & Requirements

## 1. Cross-Platform Path Resolution
- Locate the VSCode extensions directory dynamically based on the operating system using standard Node.js modules (`os`, `path`):
  - Windows: `%USERPROFILE%\.vscode\extensions\`
  - macOS/Linux: `~/.vscode/extensions/`
- Scan and target all directories matching the glob/pattern: `anthropic.claude-code-*`
- If multiple versions coexist, the modification MUST be applied to `webview/index.js` across ALL installed versions to prevent version-mismatch skips.

## 2. Robust Minified Code Matching
- In `index.js`, locate the specific snippet rendering the assistant message container. 
- Since exact minification tokens (like variable names `ref:c`) or whitespaces can vary slightly across builds or updates, use a flexible Regular Expression matching strategy centered on the unique test ID:
  `"data-testid":"assistant-message"`
- Safely inject `dir:"auto",` immediately following the test ID attribute declaration.
- Account for optional whitespaces. The transformation must handle variations like:
  From: `"data-testid":"assistant-message",className:` OR `"data-testid":"assistant-message", className:`
  To:   `"data-testid":"assistant-message",dir:"auto",className:`

## 3. Automation Preferred (Cross-Platform Node.js Script)
- Provide a single, clean, ready-to-run Node.js script (`patch-claude.js`) using native modules (`fs`, `path`, `os`) to automate this entire discovery, regex-replacement, and rewriting process.
- The script must include safety checks:
  - Verify file existence before reading.
  - Check if `dir:"auto"` is already present to avoid duplicate injections.
  - Handle potential read/write permission errors gracefully.

## 4. Crucial Notifications (Include at the end of output)
- Explicitly remind the user that they **must fully restart VSCode** (not just reload the window) for the webview changes to take effect.
- Include a clear warning stating that **this is a volatile local patch** and will be wiped clean whenever the Claude Code extension undergoes an automatic update, requiring a re-run of the modification.
