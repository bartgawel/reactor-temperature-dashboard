# Reactor Plotly Dashboard — embedded Excel worksheet add-in

This is a **Content Add-in** (different from the earlier task-pane add-in). It embeds live Plotly HTML inside an Excel worksheet as an add-in object. Save the `.xlsx` workbook after inserting the content add-in. The embedded add-in should remain on reopening supported clients, provided the HTTPS-hosted HTML and manifest remain accessible, add-ins are enabled, and the file remains `.xlsx`.

## Hosting
1. Upload `content.html` and `icon.png` to the same GitHub Pages repository as the earlier dashboard (`reactor-temperature-dashboard`).
2. Edit **all** `YOUR-USERNAME` placeholders in `manifest-content.xml` to your actual GitHub Pages username (or change to your HTTPS hosting domain).
3. Confirm `https://YOUR-USERNAME.github.io/reactor-temperature-dashboard/content.html` loads in a browser.
4. Sideload/install **manifest-content.xml as a new content add-in**, not the earlier task-pane manifest. Your organization may restrict content add-in sideloading.

## Insert in Excel
1. Create and select an empty worksheet, e.g. `Reactor_Dashboard`.
2. From **Home/Insert > Add-ins** select **Reactor Plotly Dashboard (Embedded)** to insert the content object into the worksheet (menu names vary).
3. Enlarge the embedded object's frame (approx. 980 x 720 px or larger); the complete dashboard scrolls inside it.
4. In the dashboard, use **Data sheet** to choose `Sheet1_without_duplicates`, `Sheet1_corrected_reactor_temp`, or your data tab. It reloads automatically from the selected sheet and remembers the selection in workbook add-in settings.
5. Save the workbook as `.xlsx` on OneDrive/SharePoint or locally. Close and reopen and check persistence. Changes to the source workbook should be available through **Refresh selected sheet**; the dashboard attempts to load the source on reopening.

## Notes
- The **content object** is live Plotly, including zoom, hover and filters. This is **not** a static image.
- The HTML code is hosted on GitHub Pages, not packed into the `.xlsx`; the workbook stores an add-in reference and potentially an image snapshot. Keep hosting accessible. Network restrictions/Office add-in policy can prevent reload.
- Do not save as `.xls` or CSV: those formats do not preserve live content add-ins.
- If your workbook had an earlier add-in that disappeared, test this new **ContentApp** manifest on a small workbook first. Persistence depends on Excel support, deployment, trust, and file save state; it cannot be absolutely guaranteed from the code alone.
- This is **a separate installed add-in** from the earlier task-pane manifest; do not overwrite the old manifest with this one if you want both.
- Source rows with valid timestamps and temperatures feed the four charts; the recipe name is excluded.
