Homework Dashboard — README

This is a single static HTML file (homework_dashboard.html) for the Kidora Learn ERP Homework module. No build step, no backend — everything is hardcoded HTML/CSS/JS, so you plug in real data by editing the file directly.

File structure

Everything lives in one file:

<style> — all CSS, using CSS variables at the top (:root) for colors
<body> — all the dashboard sections, top to bottom
<script> — the two bits of interactivity: the trend chart line and the student tracker's search/filter
Color variables

Defined once in :root at the top of the <style> block — change these to re-theme the whole dashboard:

Variable	Used for
--blue	Primary brand color, headings, KPI numbers
--blue-mid	"Assigned" bars, chart accents
--orange	Underline accents on section headers
--green	"On time" / "Submitted" / "streak" states
--red	"Missed" / "At risk" / "overdue" states
--steel	Muted secondary text (labels, meta info)
--ink	Main body text color
--grid	Borders and hairlines
--bg	Light page background used inside tracks/bars
Sections, top to bottom

Topbar — school name and reporting period. Static text, edit directly.

Toolbar — date range picker (#dateFrom / #dateTo, not wired to anything yet) and the Download Homework Report button, which just toggles a dropdown menu of report links (href="#" — point these at real export endpoints).

Filters bar — Academic Year, Branch, Class, Section, Submission Status, Period. These are plain <select> elements with no onchange handlers yet — they're visual/placeholder until wired to a data source.

KPI row — six stat cards (Homework Assigned, Submitted On Time, Pending Review, Overdue Submissions, Completion Rate, Defaulters). Each .kpi block has a .num (the figure) and a .delta (up/down class controls red/green arrow). Hardcoded — update the numbers directly.

Submission Trend chart — SVG line chart, fixed on August 2026. The line data comes from two JS arrays near the bottom of the file:

js
var cumulativeSubmitted = [10, 22, 34, 47, 60, 73, 86, 98, 108, 118, 128, 138]; // one value per month
var missedOverdue       = [6, 7.5, 6.8, 7, 6.5, 6, 5.6, 5.2, 5, 4.8, 4.5, 4.2];

renderTrend('7') (index 7 = August) draws the line for that month. The blue "Cumulative Submitted" line is styled bold with a rounded stroke and an arrowhead marker (#trendArrow in the SVG <defs>) to match the reference style you shared. To show a different month, change the argument passed to renderTrend() at the bottom of the script.

Submission Status Split — donut chart (On Time / Late / Missed) with the total shown in the center. The donut segments are drawn with stroke-dasharray/stroke-dashoffset on three stacked circles — the numbers there are percentages of 100, so if you change the underlying data, recompute the dasharray values to match. The center total (128) and the legend percentages are hardcoded — update both together.

Section-wise Comparison — bar chart comparing Assigned / Submitted / Missed across Section A/B/C. Each .bar-group has three .bar divs (assign, collect, missed) — the height is a percentage of the chart area and the <span> inside is the label. Update both together to keep the bar height and printed number consistent.

Student Tracker — the searchable/filterable table (Roll, Student, Class, Section, Submitted, Missed, Streak, Status). Filtering is handled by filterTracker(), which reads the Class dropdown, Section dropdown, and the search box, and shows/hides <tr> rows based on their data-class / data-section attributes. To add a student, copy an existing <tr data-class="..." data-section="..."> row and fill in the cells — no JS changes needed, the filter will pick it up automatically. A small status legend (On Time / Partial / At Risk) sits above the table.

Top Defaulters vs Top Performers — two-column list, pulled from the same students as the tracker table for consistency (ranked by missed count / streak). Each row is a repeated block (avatar initials + name + class/section + pill badge) — copy a row to add a student, and keep the initials, colors (--red/pink for defaulters, --green/mint for performers) consistent with the pattern already there.

Recent Homework — simple table of active assignments (Subject, Class, Due Date, Progress, Status, Action). Static rows, edit directly.

Known placeholders / things to wire up before going live
Date range inputs, all filter dropdowns, and the "All Classes" dropdown on Top Defaulters/Performers aren't connected to any filtering logic yet — only the Student Tracker's own controls are wired.
Download report links (href="#") need real export endpoints.
Section-wise Comparison chart numbers are placeholder — swap in real per-section assigned/submitted/missed counts.
Submission Trend data (cumulativeSubmitted, missedOverdue) is placeholder monthly data for one year — replace with real figures.
Making edits

Since there's no build step, just open the .html file in a text editor, find the section by its comment/heading text (e.g. search for "Top Defaulters"), and edit the HTML/numbers directly. Reload the file in a browser to see changes.
