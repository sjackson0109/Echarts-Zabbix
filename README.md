# ECharts Widget for Zabbix - [Monzphere](https://monzphere.com)

This module adds a customizable widget to Zabbix that allows creating interactive charts using the ECharts library.

![views](https://komarev.com/ghpvc/?username=matheusandrade&repo=https://github.com/Monzphere/Echarts-Zabbix) 
![Stars](https://img.shields.io/github/stars/Monzphere/Echarts-Zabbix?style=social)
![Forks](https://img.shields.io/github/forks/Monzphere/Echarts-Zabbix?style=social)
![Issues](https://img.shields.io/github/issues/Monzphere/Echarts-Zabbix)

![image](https://github.com/user-attachments/assets/0b321c1d-8993-477a-93df-a9ae55dbdb62)

## 📦 Installation

1. Download or clone this repository.
2. Copy the entire folder into the Zabbix modules directory:
   ```
   /usr/share/zabbix/modules/echarts/
   ```
3. In the Zabbix web interface, go to **Administration → General → Modules**.
4. Click **Scan directory**, then **Enable** the *ECharts Widget* module.
5. The widget will now be available when editing any dashboard.

> **Requirements**: Zabbix 6.4 or later (uses `CWidgetFieldTimePeriod`, `CWidgetFieldPatternSelectItem`, and other modern widget APIs).

## 🚀 Features

- **27 chart types** covering gauges, temporal series, hierarchical data, relational diagrams, and tables
- Configurable primary color — automatically generates palette variations for multi-series charts
- Simplified configuration through the native Zabbix widget interface
- Automatic item unit detection and value formatting
- Real-time updates following the dashboard refresh interval
- Full Light/Dark theme support
- Interactive and responsive tooltips (always visible, never clipped)
- Interactive zoom and pan for time-series charts
- Template dashboard support via override host field

## 🎨 Color

Each widget instance has a single **Color** field (`value_color`) that defines the primary color for the chart. When a chart has multiple series, the widget automatically generates palette variations derived from that base color. If no color is set, the default ECharts palette is used.

### Intelligent Color Distribution
- **Single-color base**: Set one color; the widget derives harmonious variations for additional series
- **Automatic cycling**: When the number of series exceeds the generated variations, the palette cycles automatically
- **Consistent mapping**: Each data series maintains its color throughout the visualization

## 📊 Chart Types

### Gauge Chart
- Semi-circular gauge displaying a single item value
- Three-zone color ranges (green → yellow → red, 0–30 / 30–70 / 70–100)
- Smooth pointer animation on value updates

### Liquid Chart
- Liquid fill visualization, ideal for percentage representation
- Fill height reflects the item value (0–100 %)
- Color follows the primary color setting; fluid wave animation

### Pie Chart
- Pie visualization supporting multiple items
- Single-item mode shows value vs. remaining (100 − value)
- Rounded slice borders with hover emphasis

### Horizontal Bar Chart
- Horizontal bars sorted by value (descending)
- Shows top 10 items; a **Show more** popup lists the full dataset
- Labels displayed inside each bar; value + unit in Y-axis

### Multi-level Gauge
- Single radial gauge displaying three computed bands from one item value: Current, Desired, and Undesired
- Each band has its own color and offset label
- Useful for capacity or SLA-style visualizations

### Treemap Chart
- Hierarchical data grouped by host, then by item
- Interactive drill-down with breadcrumb navigation
- Color-coded tiles; tooltip shows formatted value with units

### Nightingale Rose Chart
- Circular rose / polar-bar visualization
- Segment radius encodes item value
- Useful for comparing values across many items simultaneously

### Funnel Chart
- Funnel-shaped series ordered from largest to smallest
- Ideal for sequential process stages or ranked metrics
- Labels show item name and formatted value

### Treemap/Sunburst Chart
- Automatically alternates between a treemap and a sunburst view
- Animated transition between views every few seconds
- Rich drill-down interaction; suitable for hierarchical data

### LLD Table
- Tabular display of all matched items (Low-Level Discovery or regular)
- Pagination with **Show more** popup for large datasets
- Column sorting; values formatted with units

### Temporal Line Chart
- Historical data plotted as lines over a configurable time period
- Multiple items displayed as distinct colored lines
- Native Zabbix time period integration (compatible with dashboard time filters)
- Smart tooltips with cross-axis indicators; auto-scaling Y-axis (K / M / G)
- Intelligent time axis labels (HH:mm for today; MMM DD + HH:mm for other days)
- Configurable smooth lines, legend, and grid
- Time period indicator shown in widget header

### Temporal Area Chart
- All Temporal Line Chart features, with gradient-fill areas instead of lines
- Overlapping (non-stacked) areas for direct comparison of trends
- Transparency effects preserve visibility of overlapping series

### Area Rainfall Chart
- Stacked area chart styled to represent cumulative or layered data
- Suitable for showing rainfall, load, or accumulation over time

### Scatter Effect Chart
- Scatter plot of current item values across a virtual X axis
- Min and max values highlighted with a pulsing ripple (`effectScatter`) effect
- Symbol size scales with value magnitude

### Vertical Column Chart
- Vertical bar chart (column chart) for comparing current item values side-by-side
- Distributed colors per bar; formatted value labels above each bar

### Stacked Bar Chart
- Horizontal stacked bars grouped by host
- Each item within a host forms a segment of the stack
- Suitable for showing part-to-whole relationships across hosts

### Doughnut Chart
- Ring/donut variant of the pie chart
- Inner hole emphasizes relative proportions
- Supports multiple items with distributed colors

### Bullet Graph
- Linear gauge (bullet chart) showing actual value against a target range
- Useful for KPI tracking with reference markers

### Radar Chart
- Spider/web chart for comparing multiple items on a common scale
- Each item maps to one axis; polygon area filled with the primary color

### Heat Map
- Grid heat map with hosts on one axis and items on the other
- Cell color intensity encodes value magnitude; visual calendar-like layout

### Candlestick Chart
- OHLC candlestick chart plotted from historical data points
- Open / High / Low / Close computed from the time-series data per period
- Suitable for monitoring metrics with high variance over time

### Bubble Chart
- Scatter plot where bubble radius encodes a third dimension (value magnitude)
- Distributed colors per item; tooltip shows name, host, and formatted value

### Gantt Chart
- Horizontal timeline chart representing item values as task durations
- Useful for visualizing maintenance windows or event spans

### Tree Diagram
- Hierarchical tree layout (top-down) grouping items under their host nodes
- Collapsible nodes; labels show item name and formatted value

### Network Diagram
- Force-directed graph with hosts as hub nodes and items as leaf nodes
- Edge weight reflects value; useful for topology-style visualizations

### Chord Diagram
- Circular chord chart showing relationships between items
- Arc width and chord thickness encode relative values

### Calendar Heat Map
- GitHub-style calendar heat map driven by historical time-series data
- One year of daily aggregates displayed as colored cells
- Color intensity encodes daily value magnitude; tooltip shows date and value
- Uses the native Zabbix time period field for data retrieval

## 🔧 Configuration

All configuration is done through the standard Zabbix widget edit dialog.

| Field | Description |
|-------|-------------|
| **Host groups** | Filter items by one or more host groups |
| **Hosts** | Filter items by specific hosts (normal dashboards) |
| **Host** (override) | Template dashboard: select the host to use |
| **Item patterns** | One or more wildcard patterns matching item names |
| **Chart Type** | Select one of the 27 available chart types |
| **Color** | Primary color (hex); drives the multi-series palette |
| **Time period** | Time range for temporal/calendar charts (supports dashboard time filter) |
| **Show Legend** | Toggle legend visibility (temporal and calendar charts) |
| **Show Grid** | Toggle grid lines (temporal and calendar charts) |
| **Smooth Lines** | Enable Bézier-smoothed lines (temporal charts) |

> **Note**: The Time period, Show Legend, Show Grid, and Smooth Lines fields are only visible in the form when a temporal or calendar chart type is selected (Temporal Line, Temporal Area, Calendar Heat Map).

### How the widget populates data

1. The PHP backend (`WidgetView`) queries `API::Item()->get()` using the configured host/group/item-pattern and tag filters.
2. For temporal chart types (Temporal Line, Temporal Area, Area Rainfall, Calendar Heat Map, Candlestick), it additionally queries `API::History()->get()` for up to **1 000 data points per item** within the configured time period.
3. All item values, metadata, and history are passed to the JavaScript frontend (`WidgetEcharts`), which renders the appropriate ECharts configuration.

## 📈 Value Formatting

The widget automatically formats values based on item units:

| Unit pattern | Formatted as |
|---|---|
| `bps`, `Kbps`, `Mbps`, `Gbps` | Auto-scaled bits per second |
| `B`, `KB`, `MB`, `GB`, `TB` | Binary byte scale (÷ 1024) |
| `/s` suffix | Rate, formatted as bits/sec |
| `%` | Percentage with 2 decimal places |
| Large numbers (≥ 1 K) | K / M / G suffixes |
| Very small numbers (< 0.01) | Scientific notation |

## ⏰ Temporal Chart Details

- **Native Zabbix Integration**: Uses `CWidgetFieldTimePeriod` for time range management and `API::History()->get()` for data retrieval
- **Dashboard Time Filter Integration**: Seamlessly follows dashboard global time settings
- **Time Period Display**: Shows the active time range in the widget header (clock icon)
- **Performance**: Up to 1 000 history points per item per refresh cycle
- **Real-time Updates**: Refreshes on the standard dashboard widget interval
- **Multi-item Support**: Each item rendered as a separate series with a distinct color
- **Interactive Navigation**: ECharts built-in zoom, pan, and tooltip interactions
- **Responsive Design**: Adapts to widget size changes and screen resolutions

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests.

## 📄 License

This project is licensed under the AGPL-3.0 license
