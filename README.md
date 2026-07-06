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
