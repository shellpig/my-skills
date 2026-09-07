---
name: webapp-testing
description: Toolkit for interacting with and testing local web applications using Playwright. Supports verifying frontend functionality, debugging UI behavior, capturing browser screenshots, and viewing browser logs.
license: Complete terms in LICENSE.txt
---

# Web Application Testing

**Use the project's own test toolchain first.** If it already has Playwright (in any
language), fixtures, and a command to run them, work inside that — a parallel script
in a second language duplicates the setup and rots. Read the project's test and
startup instructions before writing anything.

Write a standalone Python Playwright script only when the project has no suitable
entry point, or for throwaway reconnaissance you won't keep.

The rest of this skill assumes the standalone case; the waiting and selector
guidance applies either way.

**Helper Scripts Available**:
- `scripts/with_server.py` - Manages server lifecycle (supports multiple servers)

**Always run scripts with `--help` first** to see usage. DO NOT read the source until you try running the script first and find that a customized solution is abslutely necessary. These scripts can be very large and thus pollute your context window. They exist to be called directly as black-box scripts rather than ingested into your context window.

## Decision Tree: Choosing Your Approach

```
User task → Is it static HTML?
    ├─ Yes → Read HTML file directly to identify selectors
    │         ├─ Success → Write Playwright script using selectors
    │         └─ Fails/Incomplete → Treat as dynamic (below)
    │
    └─ No (dynamic webapp) → Is the server already running?
        ├─ No → Does the project define how to start it?
        │        ├─ Yes → Follow it. Some projects require a specific
        │        │         runtime, shell, working directory, or container
        │        │         because the obvious `npm run dev` is known-broken
        │        │         on that platform.
        │        └─ No  → Run: python scripts/with_server.py --help
        │                  Then use the helper + write a Playwright script
        │
        └─ Yes → Reconnaissance-then-action:
            1. Navigate, then wait for a condition you can name
               (target element visible, expected data rendered)
            2. Take screenshot or inspect DOM
            3. Identify selectors from rendered state
            4. Execute actions with discovered selectors
```

## Example: Using with_server.py

To start a server, run `--help` first, then use the helper:

**Single server:**
```bash
python scripts/with_server.py --server "npm run dev" --port 5173 -- python your_automation.py
```

**Multiple servers (e.g., backend + frontend):**
```bash
python scripts/with_server.py \
  --server "cd backend && python server.py" --port 3000 \
  --server "cd frontend && npm run dev" --port 5173 \
  -- python your_automation.py
```

To create an automation script, include only Playwright logic (servers are managed automatically):
```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True) # Always launch chromium in headless mode
    page = browser.new_page()
    page.goto('http://localhost:5173') # Server already running and ready
    page.locator('#app-root').wait_for() # Wait for a real readiness signal, not a timer
    # ... your automation logic
    browser.close()
```

## Reconnaissance-Then-Action Pattern

1. **Inspect rendered DOM**:
   ```python
   page.screenshot(path='/tmp/inspect.png', full_page=True)
   content = page.content()
   page.locator('button').all()
   ```

2. **Identify selectors** from inspection results

3. **Execute actions** using discovered selectors

## Common Pitfall

❌ **Don't** inspect the DOM the instant `goto()` returns on a dynamic app — the
   framework hasn't rendered yet
❌ **Don't** reach for `page.wait_for_load_state('networkidle')`. Playwright
   discourages it: an idle network doesn't mean the UI finished rendering or the
   save completed, and one long-lived request (polling, websocket, analytics) can
   hang the wait until it times out
✅ **Do** wait for the specific state that proves readiness — the element you're
   about to act on, the row you expect, the status text, the version counter that
   advances after a save:

```python
page.locator('[data-testid="character-row"]').first.wait_for()
expect(page.get_by_role('button', name='Save')).to_be_enabled()
```

Web-first assertions (`expect(...)`) retry on their own, so they double as waits.

## Best Practices

- **Use bundled scripts as black boxes** - To accomplish a task, consider whether one of the scripts available in `scripts/` can help. These scripts handle common, complex workflows reliably without cluttering the context window. Use `--help` to see usage, then invoke directly. 
- Use `sync_playwright()` for synchronous scripts
- Always close the browser when done — and only close what *you* started; leave the
  user's own running servers and browsers alone
- Use descriptive selectors: `text=`, `role=`, CSS selectors, or IDs
- Wait on observable state: `page.wait_for_selector()`, `locator.wait_for()`, or a
  retrying `expect()`. Keep `page.wait_for_timeout()` for ad-hoc debugging only — a
  fixed delay states no completion condition, so it is flaky when slow and wasteful
  when fast
- Headless by default. Run headed only when a human needs to watch
- **Know what the test run destroys before you start it.** Suites that seed or reset
  state can wipe real local data; check the reset scope and back up anything the
  user cares about

## Reference Files

- **examples/** - Examples showing common patterns:
  - `element_discovery.py` - Discovering buttons, links, and inputs on a page
  - `static_html_automation.py` - Using file:// URLs for local HTML
  - `console_logging.py` - Capturing console logs during automation