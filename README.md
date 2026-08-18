# Patrol Encounter Rate Stats Workflow

## Introduction

This workflow helps you to measure how frequently your patrols encounter events of interest, relative to the effort those patrols put in. It combines patrol tracks and patrol events from **EarthRanger** into a single summary table of encounter rate statistics.

**What this workflow does:**

- Downloads patrol tracks and patrol events from EarthRanger for your chosen time range
- Converts patrol observations into trajectories to measure effort (distance travelled, time on patrol, days patrolled)
- Counts patrol events (or totals an event details field such as "Number of Animals")
- Calculates encounter rate metrics per category — for example Total Encounters, Encounters per Hour, and Encounters per Kilometer, broken down by patrol type, patrol subject, time period, or spatial region
- Presents everything as an interactive, sortable summary table on a dashboard, with a built-in download option

**Who should use this:**

- Conservation managers evaluating patrol effectiveness and coverage
- Ecologists and researchers analyzing wildlife sighting rates against patrol effort
- Anyone who needs a per-category encounter rate summary from patrol data stored in EarthRanger

## Prerequisites

Before using this workflow, you need:

1. **Ecoscope Desktop** installed on your computer
   - If you haven't installed it yet, please follow the installation instructions for Ecoscope Desktop

2. **EarthRanger Data Source** configured in Ecoscope Desktop
   - You must have already set up a connection to your EarthRanger server
   - Your data source should be configured with proper authentication credentials
   - You'll need to know the name of your configured data source (e.g., "mep_dev")

3. **Patrols and Patrol Events** recorded in EarthRanger
   - Your EarthRanger site must have patrols with tracked subjects (so effort can be measured) and events reported during those patrols
   - You'll need to know your patrol type names — you can find them in your EarthRanger Admin site under Activity → Patrol Types
   - If you plan to count only specific event types, you'll also need their exact "Event Types" values, found at `https://<your-site>.pamdas.org/admin/activity/eventtype/`

## Installation

1. Select "Workflow Templates" tab
2. Click "+ Add Template"
3. Copy and paste this URL https://github.com/wildlife-dynamics/patrol-encounter-rate-stats and wait for the workflow template to be downloaded and initialized
4. The template will now appear in your available template list

## Configuration Guide

### Basic Configuration

#### 1. Workflow Details

Basic information about this workflow run.

- **Workflow Name** (required): A name for this analysis
  - Example: `Patrol Encounter Rate Stats`
- **Workflow Description** (optional): A short description of what this run covers
  - Example: `Per-category encounter rate summary table.`

#### 2. Data Source

The EarthRanger connection to pull data from.

- **Data Source** (required): Select the name of your configured EarthRanger data source
  - Example: `mep_dev`

#### 3. Time Range

The analysis window. Patrols that overlap this window are included, and their tracks and events are truncated to it.

- **Since** (required): Start of the time range
  - Example: `2015-01-10T00:00:00`
- **Until** (required): End of the time range
  - Example: `2015-02-28T23:59:59`
- **Timezone** (required): The timezone your results should be reported in
  - Example: `Africa/Nairobi (UTC+03:00)`

#### 4. Patrol and Event Types

Which patrols and events to analyze.

- **Patrol Types** (optional): The patrol type(s) to analyze, selected from your EarthRanger site
  - Example: `ecoscope_patrol`
  - Note: Leave empty to analyze all patrol types
- **Event Types** (optional): The event type(s) to count — enter the "Event Types" value for each, one per field
  - Example: `wildlife_sighting_rep`
  - Note: Leave empty to count all patrol events. Values must match EarthRanger exactly — an unmatched value means zero events are counted
- **Patrol Status** (optional): Which patrol statuses to include
  - Default: `done`

#### 5. Group Data

Configure how results are grouped and split into per-group dashboard views. Each grouper creates a separate view of the summary table, switchable on the dashboard.

- **Groupers** (optional): Add one or more groupers of these kinds:
  - **Category**: `Patrol Serial Number`, `Patrol Type`, or `Patrol Subject`
  - **Time**: a time period such as Year (`%Y`), Month (`%B`), Year and Month (`%Y-%m`), Date (`%Y-%m-%d`), day-of-year (`%j`), day-of-month (`%d`), day-of-week (`%A`), or Hour (`%H`)
  - **Spatial**: the name of a spatial feature group from your EarthRanger site
  - Note: Leave empty for a single dashboard view covering all data

#### 6. Encounter Rate Summary

The heart of the workflow: how the summary table rows are aggregated and which metric columns appear.

- **Summarize by** (optional): Group summary table rows by category, time period or spatial feature group — each entry adds a grouping column within the same dashboard view. The choices are the same Category / Time / Spatial options as in Group Data.
  - Default: `Patrol Type`
  - Note: Leave empty for a single overall row
- **Measure Encounters By** (required): Choose how encounters are measured
  - **Number of Events** (default): counts patrol events
  - **Sum of an Event Field**: totals a chosen event details field instead — an **Event Field to Sum** input appears, using the field title shown in EarthRanger (for example `Number of Animals`)
  - Note: The Encounter metrics follow this choice; the Total Events metric always counts events
- **Metrics** (required): The metric columns of the table, in the order you add them:

  | Metric | What it shows | Options |
  |---|---|---|
  | Total Encounters | Number of events, or the event-field total | — |
  | Encounters per Duration | Encounters per hour or per day of patrol time | Unit: `h` or `d` |
  | Encounters per Distance | Encounters per kilometer or meter travelled | Unit: `km` or `m` |
  | Encounters per Patrol | Encounters divided by the number of patrols | — |
  | Encounters per Patrol Day | Encounters divided by distinct days with patrol movement | — |
  | Total Events | Raw count of patrol events (always a count) | — |
  | Total Duration | Time on patrol | Unit: `h` or `d` |
  | Total Distance | Distance travelled on patrol | Unit: `km` or `m` |
  | Patrol Count | Number of patrols | — |
  | Patrol Days | Distinct calendar days with patrol movement | — |
  | Area Covered (Merged) | Area swept by patrols, overlaps merged | Swath Width (m) |
  | Area Covered (Unmerged) | Area swept by patrols, overlaps counted separately | Swath Width (m) |
  | Custom | Your own statistic over a data column | Display Name, Statistic (`sum`, `count`, `min`, `max`, `mean`, `nunique`, `median`), Column, Decimal Places, Convert Units |

  - Default: `Total Encounters`, `Encounters per Duration (h)`, `Encounters per Distance (km)`

### Advanced Configuration

These optional settings provide additional control over your workflow:

#### Filter Data

Clean up GPS data before effort and encounter rates are calculated.

- **Bounding Box**: Only include patrol observations and events whose coordinates fall inside this bounding box
- **Filter Exact Point Coordinates**: Exclude observations and events recorded at these exact coordinates (e.g. known bad GPS fixes)
- **Trajectory Filter**: Drop trajectory segments outside these length / time / speed bounds (e.g. to remove implausible jumps)

## Running the Workflow

Once you've configured all the settings:

1. **Review your configuration**
   - Double-check your time range, data source, and patrol types

2. **Save and run**
   - Click the "Submit" and the workflow will show up in "My Workflows" table button in Ecoscope Desktop
   - Click on "Run" and the workflow will begin processing

3. **Monitor progress and wait for completion**
   - You'll see status updates as the workflow runs
   - Processing time depends on:
     - The size of your date range
     - Number of patrols and tracked subjects
     - Number of patrol events in the system
   - The workflow completes with status "Success" or "Failed"

## Understanding Your Results

After the workflow completes successfully, open the workflow's dashboard.

### Visual Outputs (Dashboard)

The workflow creates a dashboard with one main visualization:

#### Encounter Rate Summary Table

- **Format**: Interactive table
- **Features**:
  - One row per Summarize by group (for example, one row per patrol type), or a single overall row if no Summarize by entry is set
  - Grouping columns first (with friendly headers such as `Patrol Type` or `Month`), followed by your chosen metric columns in the order you configured them
  - Sortable columns: click a column header to sort
  - Download button: export the table's data for use in Excel or Google Sheets
- If you configured **Group Data** groupers, the dashboard shows a separate table view per group (for example, one view per month), switchable from the dashboard's group selector.

### Reading the numbers

- **Effort metrics** (Total Duration, Total Distance, Patrol Count, Patrol Days, Area Covered) are measured from patrol trajectories.
- **Encounter metrics** follow your "Measure Encounters By" choice: event counts, or totals of your chosen event field.
- **Rates** are encounters divided by effort — e.g. Encounters per Duration (h) = encounters ÷ patrol hours.
- If a group has no effort (zero patrol time or distance), its rate cells show as empty/NA rather than a misleading zero. If your event type filter matches nothing, Total Events shows `0` and the effort columns still populate.

## Common Use Cases & Examples

Here are some typical scenarios and how to configure the workflow for each:

### Example 1: Encounter rates by patrol type (default)

**Goal**: Compare how often each patrol type encounters events, normalized by effort.

**Configuration**:
- **Time Range**:
  - Since: `2015-01-10T00:00:00`
  - Until: `2015-02-28T23:59:59`
  - Timezone: `Africa/Nairobi (UTC+03:00)`
- **Patrol Types**: `ecoscope_patrol`
- **Summarize by**: `Patrol Type` (default)
- **Metrics**: defaults (`Total Encounters`, `Encounters per Duration (h)`, `Encounters per Distance (km)`)

**Result**:
- One table with a row per patrol type, showing encounters, encounters per hour, and encounters per kilometer

---

### Example 2: Monthly encounter rate trend

**Goal**: See how encounter rates change month by month.

**Configuration**:
- **Time Range**: as above, covering several months
- **Summarize by**: Time → `Month (example: September)` (`%B`)
- **Metrics**: defaults

**Result**:
- One table with a `Month` column and one row per month, so you can spot seasonal patterns in encounter rates

---

### Example 3: Encounter rates by spatial region

**Goal**: Compare encounter rates across management sectors or other regions.

**Configuration**:
- **Time Range**: your analysis window
- **Summarize by**: Spatial → the name of your spatial feature group in EarthRanger (e.g. `Management Sectors`)
- **Group Data**: optionally add the same spatial group to also split the dashboard into one view per region

**Result**:
- A table with one row per spatial feature (e.g. per sector), each with its own encounter rate metrics

---

### Example 4: Patrol effort report with custom units

**Goal**: An effort-focused summary per patrol, with distance in meters and rates per day.

**Configuration**:
- **Summarize by**: `Patrol Serial Number`
- **Metrics**:
  - `Patrol Count`
  - `Patrol Days`
  - `Total Distance` (Unit: `m`)
  - `Encounters per Duration` (Unit: `d`)
  - `Encounters per Distance` (Unit: `km`)
  - `Encounters per Patrol`
  - `Encounters per Patrol Day`

**Result**:
- One row per patrol serial number with effort columns and per-patrol / per-patrol-day encounter rates, in the exact column order configured

## Troubleshooting

### Common Issues and Solutions

#### Workflow fails to start

**Problem**: The workflow fails immediately with a connection or authentication error.

**Solutions**:
- Verify your EarthRanger data source is configured in Ecoscope Desktop with the correct server URL, username, and password
- Confirm the data source name selected in the form matches your configured connection exactly
- Check that your EarthRanger account has permission to read patrols and events

#### No rows in the summary table

**Problem**: The workflow succeeds but the table is empty.

**Solutions**:
- Widen your time range — only patrols overlapping the range are included
- Check the **Patrol Types** selection matches patrol types that actually have patrols in the range
- Confirm the **Patrol Status** filter isn't excluding everything (default only includes `done` patrols)
- If you set a **Bounding Box** or point filters, confirm they don't exclude all observations

#### Total Events is 0 but patrols show effort

**Problem**: Effort columns populate but no events are counted.

**Solutions**:
- Check the **Event Types** values — they must match the "Event Types" value in EarthRanger exactly (e.g. `wildlife_sighting_rep`), one per field; verify at `https://<your-site>.pamdas.org/admin/activity/eventtype/`
- Leave **Event Types** empty to count all patrol events
- Confirm events were reported *within* patrols (patrol events), not as standalone events

#### Rate columns show empty/NA values

**Problem**: Encounters per Duration / Distance / Patrol cells are blank for some rows.

**Solutions**:
- This is expected when the group has zero effort (no patrol time, distance, or patrols) — a rate can't be computed
- Check whether patrol tracks exist for that group's subjects in the time range

#### Spatial grouper fails or produces no groups

**Problem**: A Spatial entry under Summarize by or Group Data doesn't resolve.

**Solutions**:
- The spatial feature group name must match your EarthRanger site exactly, including capitalization
- Confirm the feature group contains features and your account can access it

#### Workflow runs very slowly

**Problem**: The run takes much longer than expected.

**Solutions**:
- Note the first run after installation includes a one-time environment "warm-up" and is slower than subsequent runs
- Narrow the time range or restrict **Patrol Types** to reduce the amount of data fetched
- Large numbers of tracked subjects and observations increase processing time — consider splitting very long ranges into shorter runs
