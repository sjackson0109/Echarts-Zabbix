# ECharts Widget for Zabbix - [Monzphere](https://monzphere.com)

This module adds a customizable widget to Zabbix that allows creating interactive charts using the ECharts library.

![views](https://komarev.com/ghpvc/?username=matheusandrade&repo=https://github.com/Monzphere/Echarts-Zabbix) 
![Stars](https://img.shields.io/github/stars/Monzphere/Echarts-Zabbix?style=social)
![Forks](https://img.shields.io/github/forks/Monzphere/Echarts-Zabbix?style=social)
![Issues](https://img.shields.io/github/issues/Monzphere/Echarts-Zabbix)

![image](https://github.com/user-attachments/assets/0b321c1d-8993-477a-93df-a9ae55dbdb62)

> **This README describes Phase 1 of the echarts-zabbix visualisation roadmap.**
> Phase 1 introduces chart renderers that operate strictly on existing Zabbix
> item-value data without inference or fabrication.  Advanced visualisations
> requiring structured topology, LLD relationships, or geospatial data are
> deferred to Phase 2+.

## 🚀 Phase 1 Features

- **Supported chart types** (render correctly from Zabbix item-value data):
  - Vertical Column Chart
  - Stacked Bar Chart
  - Doughnut Chart
  - Radar Chart
  - Heat Map
  - Bubble Chart
  - Calendar Heat Map *(requires historical data)*

- **Pre-existing chart types** (unchanged from the original widget):
  - Gauge
  - Liquid Chart
  - Pie Chart
  - Horizontal Bar Chart
  - Multi-level Gauge
  - Treemap Chart
  - Nightingale Rose Chart
  - Funnel Chart
  - Treemap/Sunburst Chart
  - LLD Table
  - Temporal Line Chart
  - Temporal Area Chart
  - Area Rainfall Chart
  - Scatter Effect Chart

- **6 built-in color themes** for different environments and use cases
- Simplified configuration through Zabbix interface
- Automatic item unit detection
- Real-time updates
- Light/Dark theme support
- Interactive and responsive tooltips
- Interactive zoom and navigation
- Automatic value formatting based on item units
- **Input validation** — unsupported configurations display a clear message instead of rendering incorrectly

## 📊 Phase 1 Chart Types

### Vertical Column Chart
- **Input**: one or more Zabbix items (latest values)
- Each item is rendered as a vertical bar
- Item name on X-axis, value on Y-axis
- Colour per bar, auto-scaled labels

### Stacked Bar Chart
- **Input**: items from **multiple hosts** or with **multiple distinct names**
- Groups items by host; each host is a stacked series
- X-axis = item names, Y-axis = value
- **Validation**: fails with a message if only one host and one item name are present

### Doughnut Chart
- **Input**: one or more Zabbix items (latest values)
- Proportion of each item value relative to the total
- Percentage labels on each segment

### Radar Chart
- **Input**: items with **at least three distinct names** (metrics)
- Each host forms one radar polygon; each item name forms one axis
- **Validation**: fails with a message if fewer than three distinct metric names are present

### Heat Map
- **Input**: items from one or more hosts (latest values)
- Rows = item names, columns = hosts, cell colour = value intensity
- Visual map legend shown on the right

### Bubble Chart
- **Input**: one or more Zabbix items (latest values)
- X-axis = item index, Y-axis = value, bubble size proportional to value magnitude
- Grouped by host for multi-series display

### Calendar Heat Map *(optional)*
- **Input**: items with **historical data** (items_history)
- Plots aggregated daily values on a full-year calendar grid
- **Validation**: fails with a message if no historical data is available
- *Do not use this chart type if history retrieval is disabled*

## 📋 Input Requirements per Chart

| Chart Type | Latest Values | History | Min Items | Min Hosts |
|---|---|---|---|---|
| Vertical Column | ✅ | — | 1 | 1 |
| Stacked Bar | ✅ | — | 2+ (multi-host or multi-name) | 1 |
| Doughnut | ✅ | — | 1 | 1 |
| Radar | ✅ | — | 3+ distinct names | 1 |
| Heat Map | ✅ | — | 1 | 1 |
| Bubble | ✅ | — | 1 | 1 |
| Calendar Heat Map | — | ✅ | 1 | 1 |

## 🔒 Data Handling Constraints

- **Latest-value charts** use current item values only (`items_data`)
- **Historical charts** use `items_history` only
- No data is fabricated or inferred:
  - No synthetic targets or thresholds
  - No artificial OHLC values
  - No inferred topology edges
  - No fabricated durations or coordinates
- If required data is missing, the widget shows an explicit error message

## ⚠️ Known Limitations

- Stacked Bar requires at least two hosts or two distinct item names to produce a meaningful visualisation
- Radar chart axes are scaled independently per indicator; values of different units on the same radar are visually comparable but not mathematically equivalent
- Calendar Heat Map aggregates all selected items into a single daily total; per-item breakdown is not available in Phase 1
- All Phase 1 charts use the current (latest) item value only — temporal trends require the Temporal Line or Temporal Area chart types

## 🚫 Deferred to Phase 2+

The following chart types are **not available** in Phase 1. Selecting them will display an informational message explaining why they are not rendered.

| Chart Type | Reason for deferral |
|---|---|
| Bullet Graph | Requires explicit target/threshold values not present in item data |
| Candlestick Chart | Requires genuine OHLC data; synthetic generation is not permitted |
| Gantt Chart | Requires a structured task time model (start, end, duration) |
| Tree Diagram | Requires LLD hierarchy/relationship data |
| Network Diagram | Requires topology data (e.g. LLDP neighbours) |
| Chord Diagram | Requires explicit dependency/flow relationship data |

## 🎨 Color Themes

The module includes **6 carefully designed color themes** that ensure optimal visibility and accessibility:

### Available Themes:
1. **Default** - ECharts standard colors with excellent contrast
2. **Zabbix** - Native Zabbix trigger severity colors (Information, Warning, Average, High, Disaster)
3. **Pastel** - Soft, pastel colors for a subtle appearance
4. **Bright** - Vibrant, high-contrast colors for better visibility
5. **Dark** - Deep, saturated colors perfect for dark themes
6. **Blue Monochrome** - Various shades of blue for a professional look

## 🔧 Configuration

1. **Chart Type**: Select desired chart type (Phase 1 types only are shown)
2. **Color Theme**: Choose from 6 professional color themes
3. **Items**: Choose items to monitor
4. For temporal charts, configure:
   - **Time period** - uses native Zabbix time period selector with dashboard time filter integration
   - Show/hide legend
   - Show/hide grid
   - Enable/disable smooth lines
5. The widget automatically:
   - Detects item units
   - Formats values appropriately
   - Adjusts colors and scales
   - Configures tooltips and interactions
   - Validates input before rendering

## 📈 Value Formatting

The widget automatically formats values based on item units:
- Bytes (B, KB, MB, GB, TB)
- Percentages (%)
- Rates per second (B/s, KB/s, etc)
- Numeric values with appropriate precision
- Scientific notation for very large/small values

## 🎨 Visual Customization

- Adaptive colors based on values
- Light/Dark themes
- Responsive and always visible tooltips
- Smooth interactions and animations
- Responsive layout that adapts to widget size

## ⏰ Temporal Charts Features

- **Native Zabbix Integration**: Uses Zabbix's History API for data retrieval and `CWidgetFieldTimePeriod` for time management
- **Dashboard Time Filter Integration**: Seamlessly works with dashboard time filters and global time settings
- **Time Period Display**: Shows current time period in widget header with visual indicator
- **Performance Optimized**: Efficient data loading with configurable limits (max 1000 points per item)
- **Real-time Updates**: Automatic refresh following widget refresh intervals
- **Multi-item Support**: Display multiple items on the same temporal chart with distinct colors
- **Interactive Navigation**: Zoom, pan, and tooltip interactions with ECharts
- **Responsive Design**: Adapts to different widget sizes and screen resolutions
- **Professional Time Formatting**: Consistent with other Zabbix time-based widgets

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests.

## 📄 License

This project is licensed under the AGPL-3.0 license
