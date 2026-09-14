# Contribution [#1]: allow copy photo to clipboard

**Contribution Number:** [1]  
**Student:** Jhenelle Walters  
**Issue:** [\[GitHub issue link\]](https://github.com/LibrePhotos/librephotos/issues/544)  
**Status:** Phase II Complete

---

## Why I Chose This Issue

I chose this issue because it is an issue where I could work on my front end development skills and also challenge myself with something a bit more complicated in order to develop my understanding of how websites work and how different functions of  webpages work in order to become a better front and developer. I've worked on developing webpages before and I've also created an app before so it's in my skill set and it's also something I can improve within. 

---

## Understanding the Issue

### Problem Description

 When you copy a photo from the timeline in librephotos, it copies a .webp instead of the actual photo which limits the use of copy and paste function and doesn't allow it to be used more frequently or across multiple programs.

### Expected Behavior

When a user selects and copies a photo from the timeline in LibrePhotos, the application should place the actual raw image data (or a widely compatible full-resolution image format) onto the system clipboard so it can be pasted universally across different desktop programs and messaging app

### Current Behavior

When you try to copy an image from the timeline into another program like Google Docs, nothing is pasted and the clipboard registers no valid binary image content.

### Affected Components

Frontend timeline image rendering components, event handlers managing clipboard interactions, and data copy utilities in the LibrePhotos frontend codebase.
---

## Reproduction Process

### Environment Setup

Following the development installation I downloaded Docker Desktop
navigated to the deploy/compose directory
And created my .env file
Then in terminal I ran "docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d"
Opened the application using http://localhost:3000

### Steps to Reproduce

1. Launch LibrePhotos in the local development environment (http://localhost:3000).
2. Navigate to the timeline and right-click on an image tile.
3. Select "Copy Image" (or press Ctrl+C / Cmd+C).
4. Attempt to paste (Ctrl+V / Cmd+V) into Google Docs or another external document.
5. Observe that no image appears in the document.


### Reproduction Evidence

- **Commit showing reproduction:** 
 [N/A (reproduced directly on existing main branch via manual UI testing)](https://github.com/KJhenelle/librephotos)
- **Screenshots/logs:** 
![Terminal Clipboard Output](./TerminalCheck.png)
- **My findings:**
 When attempting to copy a photo directly from the timeline view, the system clipboard only captures plain text and HTML metadata rather than raw binary image data (e.g., «class PNGf» or standard raster formats). Because the binary image payload is absent or unhandled by standard operating system pasteboards, external applications like Google Docs cannot recognize or paste the photo.

---

## Solution Approach

### Analysis

Browsers handle native right-click "Copy Image" by capturing the active DOM `
element source. Because LibrePhotos serves optimized WebP assets and authenticated media paths, browsers often fail to serialize the binary data to standard pasteboards, or external applications fail to decode WebP clipboard buffers. Standardizing the clipboard payload requires an explicit application-level handler that fetches the image blob, converts it to standardimage/png`, and writes it via the modern Async Clipboard API.

### Proposed Solution

Introduce an explicit "Copy to Clipboard" action (via the photo selection toolbar, tile overlay icon, or keyboard shortcut) that uses navigator.clipboard.write([new ClipboardItem({ 'image/png': blob })]) to write standardized PNG binary image data to the system clipboard.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Photos copied from the timeline fail to paste into external applications because the clipboard receives metadata or an unsupported format instead of standard raster image binary data.

**Match:** Similar web clipboard implementations fetch the target image via fetch(), convert the response blob or canvas render into image/png, and write it using navigator.clipboard.write().

**Plan:** [Step-by-step implementation plan]
1. Locate the timeline photo card and selection action components in apps/frontend/src/.
2. Implement a clipboard utility helper function that takes an image URL, fetches the binary blob, converts it if necessary to PNG format, and writes it to the clipboard using navigator.clipboard.write().
3. Add a "Copy to Clipboard" action button or context menu entry accessible from the photo tile/selection bar.
4. Add user feedback (e.g., a toast notification) confirming successful copy or displaying permission warnings.

**Implement:** [\[Link to your branch/commits as you work\]](https://github.com/KJhenelle/librephotos)

**Review:** Ensure compliance with LibrePhotos contribution guidelines, ESLint rules, and cross-browser clipboard permission considerations.

**Evaluate:** Verify that copying a photo places «class PNGf» onto the clipboard and successfully pastes into Google Docs and external editors.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [1] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
