+++
date = '2026-03-09T00:00:00-04:00'
draft = false
title = 'Rendering unified diffs in React'
tags = ['diff', 'react', 'frontend']
categories = ['engineering']
image = "/images/diff-unified-react-banner.png"
featuredImage = "/images/diff-unified-react-banner.png"
+++

You can show unified-diff style changes (like `git diff`) between two strings in any web app — for example, comparing a saved baseline to the current value in a form or editor. Here’s a minimal approach using one dependency and a bit of React.

### What you get

- **Unified format**: each line prefixed with `+` (added), `-` (removed), or two spaces `  ` (unchanged).
- **Line-level diff**: compute add/remove/unchanged chunks with the `diff` package.
- **Word-level highlighting (optional)**: on changed lines, highlight only the words that changed, not the whole line.
- **Familiar colors**: green for additions, red for removals.

---

### 1. Install and compute the diff

```bash
npm install diff
```

```ts
import { diffLines, diffWords, type Change } from "diff";

function computeDiff(oldText: string, newText: string): Change[] {
  return diffLines(oldText || "", newText || "");
}
```

`diffLines` returns an array of `Change` objects:

```ts
type Change = {
  value: string;
  added?: true;
  removed?: true;
};
```

Unchanged chunks have no `added` or `removed` flag.

---

### 2. Normalize both sides to the same format

If one value is HTML and the other is plain text (or markdown), normalize both to the same format before diffing, or the diff will be noisy.

```ts
function normalizeForDiff(
  value: string,
  format: "text" | "markdown"
): string {
  if (!value?.trim()) return "";
  if (format === "markdown") return value.trim();
  // If you have HTML, convert to text/markdown first, e.g.:
  // return htmlToMarkdown(value);
  return value.trim();
}

const oldNorm = normalizeForDiff(baseline, "markdown");
const newNorm = normalizeForDiff(current, "text");
const changes = computeDiff(oldNorm, newNorm);
```

Always diff comparable strings (e.g. both markdown or both plain text).

---

### 3. Turn changes into lines with unified-style prefixes

Walk the `Change[]`, split each `value` by newlines, and assign a prefix per line so it looks like `git diff`:

```ts
type DiffLine = {
  prefix: string;
  line: string;
  isAdded: boolean;
  isRemoved: boolean;
};

const lines: DiffLine[] = [];

for (let i = 0; i < changes.length; i++) {
  const change = changes[i];
  const lineStrs = (change.value || "").split("\n");

  for (let j = 0; j < lineStrs.length; j++) {
    const line = lineStrs[j] ?? "";
    const isLast = j === lineStrs.length - 1 && line === "";
    if (isLast && change.added) continue; // skip trailing newline in added block

    lines.push({
      prefix: change.added ? "+ " : change.removed ? "- " : "  ",
      line,
      isAdded: !!change.added,
      isRemoved: !!change.removed
    });
  }
}
```

Rendering is then a loop over `lines`, showing prefix plus line in a `<pre>` or list, with background color based on `isAdded` / `isRemoved`.

---

### 4. Word-level highlighting on changed lines (optional)

For lines that are added or removed, run `diffWords` between the old and new line so only the changed words are highlighted:

```ts
const wordChanges = diffWords(oldLine, newLine);
// wordChanges: Change[] — each part has .value, and optionally .added or .removed
```

Render each part: if `part.added`, use a green background; if `part.removed`, use red; otherwise plain text. That gives line-level context with word-level precision.

Example rendering for one line:

```tsx
wordChanges.map((part, k) => {
  if (part.added) {
    return (
      <span
        key={k}
        style={{ background: "#e5f7ee", color: "#00b248" }}
      >
        {part.value}
      </span>
    );
  }

  if (part.removed) {
    return (
      <span
        key={k}
        style={{ background: "#ffebf2", color: "#cc0000" }}
      >
        {part.value}
      </span>
    );
  }

  return <span key={k}>{part.value}</span>;
});
```

Use your own palette or CSS variables.

---

### 5. Debounce the current value

If the new string comes from an editor or input, debounce it before recomputing the diff so you don’t run the diff on every keystroke:

```tsx
const [debouncedCurrent, setDebouncedCurrent] = useState(current);

useEffect(() => {
  const t = setTimeout(() => setDebouncedCurrent(current), 500);
  return () => clearTimeout(t);
}, [current]);
```

Then pass `debouncedCurrent` into `computeDiff` instead of `current`.
