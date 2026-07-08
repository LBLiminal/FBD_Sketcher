# FBD Sketcher — Excel Add-in

Draw free body diagrams in a side panel inside Excel, right next to your calcs.
Two integrations with the worksheet:

- **Insert to sheet** — renders the current diagram to an image and drops it into the active worksheet (draggable, sized sensibly).
- **Pull cells** — reads the cells you've selected in Excel and brings their values into the diagram. If you have exactly one cell selected *and* one element selected on the canvas, it sets that element's label (e.g. select force `F1`, select cell `B12` = 15.2, hit Pull → the arrow is labelled `15.2`). Otherwise each value drops in as a draggable text label.

The same `taskpane.html` also works as a normal standalone file in any browser — the Excel buttons simply stay hidden when Office isn't present.

---

## Files

```
manifest.xml       the add-in manifest you register with Excel (edit the URLs first)
taskpane.html      the app itself (self-contained: fonts + logic embedded)
commands.html      tiny file the manifest requires; nothing to edit
assets/            icons used on the ribbon button
```

---

## One-time setup

### 1. Register the manifest with Excel (sideload)

**Windows — via a Shared Folder catalog:**

1. Put your edited `manifest.xml` in a folder, e.g. `C:\Users\Laurence\OfficeAddins`.
2. Excel → **File → Options → Trust Center → Trust Center Settings → Trusted Add-in Catalogs**.
3. In **Catalog Url** paste the folder path, click **Add catalog**, tick **Show in Menu**, OK.
4. Restart Excel.
5. **Home tab → Add-ins → Shared Folder** (or **Insert → My Add-ins → Shared Folder**),
   pick **FBD Sketcher**.

A **FBD Sketcher** button appears on the Home ribbon — click it to open the panel.

> Mac / Excel on the web: sideloading is done differently (Insert → Add-ins →
> Upload My Add-in on the web; a `~/Library/Containers/.../wef` folder on Mac).
> Ask if you want those steps.

---

## Using it

1. Click **FBD Sketcher** on the Home ribbon — the panel opens on the right.
   Widen it by dragging its left edge; the tool likes a bit of room.
2. Draw your diagram as usual (paste a CAD screenshot as a background if you like).
3. **Insert to sheet** to drop the diagram onto the worksheet as an image.
4. Or select cells in the sheet, (optionally) an element on the canvas, then
   **Pull cells** to bring the numbers in as labels.

The status line at the bottom of the panel confirms each action (and shows any error).

### New in v1.3 — narrow-pane UI

- The header collapses to **File ▾ / Export ▾ menus** plus the everyday controls,
  so it fits a normal task-pane width in one or two rows instead of eight.
- **Background controls moved** to the properties panel (shown when nothing is
  selected, as a "Canvas" section) — they only appear when relevant.
- The **properties panel is collapsible** (» in its header, « tab to reopen);
  it reopens automatically when you select an element, and starts collapsed on
  very narrow panes. The **palette goes icon-only** below ~560 px.
- A **ghost preview** of the active tool follows the cursor (with snapping) so
  you can see exactly where a symbol will land before clicking.
- Status line now separates **instructions from feedback** — action messages
  ("Copied", "Inserted…") flash briefly, then the tool hint returns.
- Papercuts: engineer-mode header group now actually hides when engineer mode
  is off (CSS specificity bug), undo/redo grey out when unavailable, footer
  **zoom % indicator** (click to reset to 100%), distinct template icons,
  danger-red Clear button, Refresh greys out with no linked cells, shortcut
  letters shown in the palette, and the canvas now uses **pointer events** so
  pen/touch input works.

### New in v1.2

- **Attachments** — loads and supports placed on a beam stick to it and follow
  when the beam moves or reshapes (UDL / dimension endpoints too). Drag an
  element clear of the beam to detach it, or use Detach in the properties panel.
- **Solve reactions** — in engineer mode the equilibrium panel gains a button
  that solves support reactions for any statically determinate support set
  (pin + roller, a single fixed support, etc.) and adds them as green labelled
  arrows. Re-run after changing loads; the equilibrium check then reads Balanced.
- **Linked cells** — Pull cells now remembers which cell each value came from.
  The **Refresh** button re-pulls every linked label (and magnitude, in engineer
  mode) so diagrams track the calc sheet. Unlink from the properties panel.
- **Update-in-place inserts** — Insert to sheet names the image after the
  workbook diagram; inserting again replaces that image where it sits, keeping
  any resizing you did. No more stale duplicates.
- **Real-unit grid** — in engineer mode, grid/snap spacing is set in drawing
  units (e.g. 0.2 m) next to the scale, so geometry lands on round dimensions.

### Handy since the v1.1 update

- **Draggable labels** — select a force / moment / UDL / dimension and drag the
  round handle to move its label clear of overlapping text; the label keeps
  following the element, and "Reset label position" in the properties panel
  puts it back.
- **Fit** button (or press `0`) zooms to frame the whole diagram — also happens
  automatically when a diagram is opened or loaded from the workbook.
- **P R X S** place pin / roller / fixed / spring supports from the keyboard.
- **Ctrl+C / Ctrl+V** copies and pastes selected elements — including between two
  FBD windows. `Ctrl+Shift+Z` redoes; `Esc` now cancels a drag in progress.
- **`[` / `]`** (or the To back / To front buttons) change drawing order.
- With several elements selected, the properties panel offers a shared **colour**.
- **Export resolution** picker (1× / 2× / 4×) next to the export buttons; exported
  files are named after the workbook diagram (or dated) instead of `fbd.png`.
- In engineer mode, a blank dimension label shows the **real scaled length**
  (e.g. `2.5 m`), and **Pull cells** onto a selected force/moment/UDL sets its
  magnitude as well as the label, so the equilibrium check tracks the sheet.
- Grid spacing is adjustable in the header; a "saved" tick in the footer confirms
  each autosave (and warns if the diagram gets too big for browser storage —
  pasted backgrounds are now auto-downscaled to keep that from happening).

---

## Updating later

Just replace `taskpane.html` (or any file) in the GitHub repo — the change is live
within a minute, no re-registering needed. Bump `<Version>` in `manifest.xml` only
if you change the ribbon button or manifest itself.

---

## Notes

- **Fonts / licensing:** the panel embeds only Martian Mono (SIL Open Font Licence,
  freely redistributable).
- **Nothing leaves the sheet without you:** the add-in only reads the cells you
  select and only writes an image when you click Insert. It never moves data on its own.
- **Manifest Id** is a fixed GUID unique to this add-in; leave it as-is.
