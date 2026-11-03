# Orbit quiz

## Architecture

- The application lives entirely in [index.html](../index.html): inline CSS, semantic HTML, a decorative inline SVG, and a classic inline JavaScript script. Keep it self-contained and runnable from a `file://` URL without a server or dependencies.
- The `questions` array supplies the ten questions, answer choices, zero-based correct-answer indices, categories, and explanatory facts.
- Quiz state is `current`, `score`, and `answered`. `showQuestion()` renders choices, `selectAnswer()` locks the question and updates feedback/scoring/progress, and `showResults()` switches the panel to results. Restart resets state and restores the first question.

## Run and verify

- Open `index.html` directly in a browser; in VS Code, use the integrated browser with the absolute `file:///…/index.html` URL.
- For a focused answer-button check: reload, confirm Next is disabled, choose **Mars** on question 1, and verify score 1, progress 1/10, correct feedback, and enabled Next. Clicking another answer must not change the score.
- For the incorrect path, reload and choose **Venus**: score stays 0, progress becomes 1/10, Venus is marked incorrect, and Mars is revealed as correct.
- For the full flow, complete ten questions and check results totals and accuracy. **Explore again** must reset score, progress, question, feedback, and mission heading.
- For interaction/style changes, check keyboard activation and focus visibility, a 375px viewport without horizontal overflow, and reduced-motion behavior.
- [VS Code MCP configuration](../.vscode/mcp.json) provides an optional Playwright server for browser checks via `npx -y @playwright/mcp@latest` (Node.js 18+). This is development tooling, not an application dependency.

## Conventions and coupled behavior

- Build dynamic content with `createElement`, `textContent`, and `replaceChildren`, following the existing rendering functions.
- Answer buttons use `aria-disabled="true"` rather than native `disabled`, retaining keyboard focus. The `answered` guard prevents repeat scoring; Next uses native `disabled` until an answer is selected.
- Correct/incorrect feedback combines color, marks, screen-reader text, and a polite atomic live region. Preserve these together when changing answer presentation.
- Update both the native progress value and its accessible text through `updateProgress()`. Move focus to the question heading on advancement/restart and to the results heading on completion.
- Keep answer-specific hover, press, and focus styling scoped to `.answer`; hover and press apply only to unanswered buttons. Color transitions include both the button and its `.letter` badge.
- Keep animations and transitions covered by `prefers-reduced-motion: reduce`, and retain the forced-colors answer-state overrides.
- Responsive layouts use the existing 800px and 600px breakpoints. Decorative artwork is inline SVG and hidden from assistive technology.
