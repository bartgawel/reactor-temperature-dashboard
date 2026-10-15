# Reactor Temperature Dashboard

Standalone HTML/JavaScript Office.js task-pane dashboard with Plotly. No npm installation required.

## What is included
- `taskpane.html`: all dashboard HTML, CSS, and JavaScript in a single file.
- `manifest.xml`: basic Excel task-pane manifest.
- `icon.png`: add-in icon.

## Publish / run in Excel
1. Create a GitHub repository named `reactor-temperature-dashboard` and upload these three files to its root.
2. In GitHub Settings → Pages, deploy from main branch / root, and confirm the URL `https://YOUR-USERNAME.github.io/reactor-temperature-dashboard/taskpane.html` works.
3. Replace `YOUR-USERNAME` in `manifest.xml` with your GitHub username (all three occurrences).
4. In Excel, use Add-ins → More Add-ins → My Add-ins → Upload My Add-in (UI labels can vary with organization), then select `manifest.xml`. Organizational policies may prevent sideloading.
5. Open your data worksheet and click **Read active Excel sheet**. It recognizes headers case-insensitively and ignores `KPI_CurrentRecipe_RecipeName`.

For standalone testing, open `taskpane.html` in a browser and import a CSV. GitHub Pages requires the public repository/page to be accessible over HTTPS. `office.js` and Plotly are fetched from their CDNs, so internet access is required.

## Data expectations
- One header row (row 1); samples in subsequent rows.
- `EventTimestamp_process_end` can be an Excel serial, ISO date string, or date/time string.
- `KPI_MaxTemperature` should contain numerical values in °C.
- All other requested columns are loaded, including `Campaign Number`.
- Recipe Name is deliberately excluded.
- Each filter allows multiple categorical selections via exclusion checkboxes; high-cardinality numerical variables use min/max inputs. The main chart supports scatter, lines, optional linear trend, coloring by a selected category, and filtered CSV export. Three additional comparison charts show mean KPI_MaxTemperature with ±1 sample standard deviation error bars and record counts on hover, grouped by reactor, product name, and water temperature. All three use the current dashboard filters; categories with one measurement have an SD of 0.

## Notes / limitations
- The add-in reads values only; it does not modify Excel data.
- Excel Online may block very wide datasets or large data transfers. The workbook is read in 450-row batches to reduce request size. For large workbooks, this may take time.
- The Office.js API uses raw Excel serial dates. The script treats them as Excel's standard 1900 date system; 1904-based workbooks require date adjustment.
- Filtering `Campaign Number` as categorical may generate many checkboxes if many campaigns exist. Search is included in each categorical filter.
- Source row numbers refer to data rows when the header is on row 1 and there are no extra header lines.


## Insert charts into the workbook / refresh on opening

1. Deploy the UPDATED `taskpane.html` and `icon.png` on your GitHub Pages site.
2. In UPDATED `manifest.xml`, replace **all** `YOUR-USERNAME` placeholders. The new manifest adds the shared-runtime command and **ReadWriteDocument** permission, needed to insert images. Your organization must allow sideloading/updating the manifest.
3. Open the workbook and the **Reactor Dashboard** task pane. Navigate to the measurement worksheet and click **Read active Excel sheet**.
4. Click **Insert / Refresh Dashboard**. A worksheet named `Reactor_Dashboard` is created/updated with **four persisted PNG chart snapshots**. Save the workbook. Only named add-in shapes are replaced on refresh; original measurements are never changed.
5. Click **Enable refresh on workbook open** *once per workbook*. Save and reopen the workbook. With a supported shared runtime, the add-in starts when the workbook opens, reads the **saved source worksheet**, and refreshes the four snapshots, regardless of which sheet is active. If startup is blocked by policy or client support, manually click **Read active Excel sheet** and **Insert / Refresh Dashboard** instead.

**Important:** Interactive Plotly charts stay interactive in the task pane; embedded Excel pictures are static saved snapshots. They do not dynamically update through Excel slicers, and data/filters in the embedded snapshot are the filters applied at refresh time. The automatic refresh needs the hosted add-in available online, Excel shared-runtime support and permission to load on open. Excel may require re-sideloading the upgraded manifest; overwriting HTML alone will not grant write permissions/startup support. Browser CSV mode cannot insert Excel sheets. Tested at source/syntax level only, not in your Office tenant.
