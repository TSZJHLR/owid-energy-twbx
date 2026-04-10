# owid-energy-twbx

## Setup

Download Tableau Public from `public.tableau.com` and install it like any standard Mac app.
When you open it, connect to a data source using the left panel. For a CSV like the OWID
energy dataset, click "Text File" and select your file.

Tableau Public saves workbooks to the Tableau Public server, not locally. Use
`File > Save` to save without login. You cannot save a .twbx offline unless
you use Tableau Desktop.

On Mac, keyboard shortcuts follow the Command key instead of Ctrl.
`command + Z` to undo, `command + S` to save/publish.

## Calculated Fields

Open a calculated field from the Data pane by right-clicking any blank area
and selecting "Create Calculated Field".

### Projected Scenario Demand

```
[Electricity Generation] * (1 + [Projected Demand Growth Rate])
```

This multiplies actual generation by a parameter-driven growth rate to simulate
future demand scenarios.

### Renewables Share Electricity

```
([Hydro Electricity] + [Solar Electricity] + [Wind Electricity])
/ NULLIF([Electricity Generation], 0) * 100
```

`NULLIF` prevents division by zero for countries with no recorded generation.

### Select View (Regional Group Filter)

```
CASE [Select View]
    
    WHEN "NEPAL" THEN [Country] = "Nepal"

    WHEN "ECONOMIC PEER" THEN 
        ([Country] = "Nepal" OR 
        (([Population]) >= [Nepal Population (Bench Marks)] * (1 - [Population Buffer %]) 
         AND ([Population]) <= [Nepal Population (Bench Marks)] * (1 + [Population Buffer %])
         AND ([Gdp]) >= [Nepal GDP (Bench Marks)] * (1 - [GDP Buffer %]) 
         AND ([Gdp]) <= [Nepal GDP (Bench Marks)] * (1 + [GDP Buffer %])))

    WHEN "LANDLOCKED ASIA" THEN 
        [Country] IN ("Afghanistan", "Armenia", "Azerbaijan", "Bhutan", "Kazakhstan", "Kyrgyzstan", "Laos", "Mongolia", "Nepal", "Tajikistan", "Turkmenistan", "Uzbekistan")
    
    WHEN "LANDLOCKED AFRICA" THEN 
        [Country] IN ("Botswana", "Burkina Faso", "Burundi", "Central African Republic", "Chad", "Eswatini", "Ethiopia", "Lesotho", "Malawi", "Mali", "Nepal", "Niger", "Rwanda", "South Sudan", "Uganda", "Zambia", "Zimbabwe")
    
    WHEN "LANDLOCKED EUROPE" THEN 
        [Country] IN ("Andorra", "Austria", "Belarus", "Czechia", "Hungary", "Liechtenstein", "Luxembourg", "Moldova", "Nepal","North Macedonia", "San Marino", "Serbia", "Slovakia", "Switzerland", "Vatican City")
    
    WHEN "LANDLOCKED SOUTH AMERICA" THEN 
        [Country] IN ("Bolivia", "Nepal", "Paraguay")

    WHEN "POPULATION PEERS" THEN 
        [Country] IN ("Angola", "Ghana", "Madagascar", "Malaysia", "Mozambique", "Nepal", "Ivory Coast", "Uzbekistan", "Venezuela", "Yemen")

    WHEN "GDP PEERS" THEN 
        [Country] IN ("Azerbaijan", "Bahrain", "Bolivia", "Cameroon", "Jordan", "Latvia", "Libya", "Nepal", "Paraguay", "Slovenia", "Uganda")

    WHEN "LAND SIZE PEERS" THEN 
        [Country] IN ("Bangladesh", "Greece", "Nepal", "Nicaragua", "Suriname", "Tajikistan", "Tunisia")

    WHEN "SAARC" THEN 
        [Country] IN ("Afghanistan", "Bangladesh", "Bhutan", "India", "Maldives", "Nepal", "Pakistan", "Sri Lanka")
    WHEN "ASEAN" THEN 
        [Country] IN ("Brunei", "Cambodia", "Indonesia", "Laos", "Malaysia", "Myanmar", "Philippines", "Singapore", "Thailand", "Vietnam")
    WHEN "BRICS" THEN 
        [Country] IN ("Brazil", "Russia", "India", "China", "South Africa")
    WHEN "G7" THEN 
        [Country] IN ("Canada", "France", "Germany", "Italy", "Japan", "United Kingdom", "United States")
    WHEN "EU" THEN 
        [Country] IN ("Austria", "Belgium", "Bulgaria", "Croatia", "Cyprus", "Czechia", "Denmark", "Estonia", "Finland", "France", "Germany", "Greece", "Hungary", 
    "Ireland", "Italy", "Latvia", "Lithuania", "Luxembourg", "Malta", "Netherlands", "Poland", "Portugal", "Romania", "Slovakia", "Slovenia", "Spain", "Sweden") 
    WHEN "OECD" THEN
        [Country] IN ("Australia", "Austria", "Belgium", "Canada", "Chile", "Colombia", "Costa Rica", "Czechia", "Denmark", "Estonia", "Finland", "France", 
    "Germany", "Greece", "Hungary", "Iceland", "Ireland", "Israel", "Italy", "Japan", "South Korea", "Latvia", "Lithuania", "Luxembourg", "Mexico", "Netherlands", "New Zealand", "Norway", 
    "Poland", "Portugal", "Slovakia", "Slovenia", "Spain", "Sweden", "Switzerland", "Turkey", "United Kingdom", "United States") 
    WHEN "OPEC" THEN
        [Country] IN ("Algeria", "Congo", "Equatorial Guinea", "Gabon", "Iran", "Iraq", "Kuwait", "Libya", "Nigeria", "Saudi Arabia", "United Arab Emirates", "Venezuela") 
    WHEN "NORTHERN AFRICA" THEN 
        [Country] IN ("Algeria", "Egypt", "Libya", "Morocco", "Sudan", "Tunisia")
    WHEN "WESTERN AFRICA" THEN 
        [Country] IN ("Benin", "Burkina Faso", "Cape Verde", "Cote d'Ivoire", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Liberia", 
    "Mali", "Mauritania", "Niger", "Nigeria", "Senegal", "Sierra Leone", "Togo")
    WHEN "CENTRAL AFRICA" THEN 
        [Country] IN ("Angola", "Cameroon", "Central African Republic", "Chad", "Congo", "Democratic Republic of Congo", 
    "Equatorial Guinea", "Gabon", "Sao Tome and Principe")
    WHEN "EASTERN AFRICA" THEN 
        [Country] IN ("Burundi", "Comoros", "Djibouti", "Eritrea", "Ethiopia", "Kenya", "Madagascar", "Malawi", "Mauritius", "Mozambique", 
    "Rwanda", "Seychelles", "Somalia", "South Sudan", "Tanzania", "Uganda", "Zambia", "Zimbabwe") 
    WHEN "SOUTHERN AFRICA" THEN 
        [Country] IN ("Botswana", "Eswatini", "Lesotho", "Namibia", "South Africa") 
    WHEN "NORTH AMERICA" THEN 
        [Country] IN ("Bermuda", "Canada", "United States")
    WHEN "CENTRAL AMERICA" THEN 
        [Country] IN ("Belize", "Costa Rica", "El Salvador", "Guatemala", "Honduras", "Mexico", "Nicaragua", "Panama") 
    WHEN "CARIBBEAN" THEN 
        [Country] IN ("Antigua and Barbuda", "Aruba", "Bahamas", "Barbados", "Cuba", "Curacao", "Dominica", "Dominican Republic", "Grenada", 
    "Haiti", "Jamaica", "Martinique", "Puerto Rico", "Trinidad and Tobago") 
    WHEN "SOUTH AMERICA" THEN 
        [Country] IN ("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador", "Guyana", "Paraguay", "Peru", "Suriname", "Uruguay", "Venezuela") 
    WHEN "CENTRAL ASIA" THEN 
        [Country] IN ("Kazakhstan", "Kyrgyzstan", "Tajikistan", "Turkmenistan", "Uzbekistan") 
    WHEN "EAST ASIA" THEN 
        [Country] IN ("China", "Hong Kong", "Japan", "Macao", "Mongolia", "North Korea", "South Korea", "Taiwan") 
    WHEN "SOUTH ASIA" THEN 
        [Country] IN ("Afghanistan", "Bangladesh", "Bhutan", "India", "Maldives", "Nepal", "Pakistan", "Sri Lanka") 
    WHEN "SOUTH EAST ASIA" THEN 
        [Country] IN ("Brunei", "Cambodia", "Indonesia", "Laos", "Malaysia", "Myanmar", "Philippines", "Singapore", "Thailand", "East Timor", "Vietnam") 
    WHEN "WEST ASIA" THEN 
        [Country] IN ("Armenia", "Azerbaijan", "Bahrain", "Cyprus", "Georgia","Iran", "Iraq", "Israel", "Jordan", "Kuwait", "Lebanon", "Oman", 
    "Palestine", "Qatar", "Saudi Arabia", "Syria", "Turkey", "United Arab Emirates", "Yemen") 
    WHEN "EAST EUROPE" THEN 
        [Country] IN ("Belarus", "Bulgaria", "Czechia", "Hungary", "Moldova", "Poland", "Romania", "Russia", "Slovakia", "Ukraine") 
    WHEN "NORTH EUROPE" THEN 
        [Country] IN ("Denmark", "Estonia", "Finland", "Iceland", "Ireland", "Latvia", "Lithuania", "Norway", "Sweden", "United Kingdom") 
    WHEN "SOUTH EUROPE" THEN 
        [Country] IN ("Albania", "Bosnia and Herzegovina", "Croatia", "Gibraltar", "Greece", "Italy", "Malta", "Montenegro", "North Macedonia", "Portugal", "Serbia", "Slovenia", "Spain") 
    WHEN "WEST EUROPE" THEN 
        [Country] IN ("Austria", "Belgium", "France", "Germany", "Luxembourg", "Netherlands", "Switzerland") 
    WHEN "AUSTRALIA & NEW ZEALAND" THEN 
        [Country] IN ("Australia", "New Zealand") 
    WHEN "MELANESIA" THEN 
        [Country] IN ("Fiji", "Papua New Guinea", "Solomon Islands", "Vanuatu") 
    WHEN "POLYNESIA" THEN 
        [Country] IN ("American Samoa", "Samoa", "Tonga", "Tuvalu") 

ELSE FALSE
END
```

Use this as a dimension filter across all sheets so one dropdown controls every chart
in the dashboard simultaneously via dashboard actions.

### Nepal Highlight Filter

```
IF [Country] = "Nepal" THEN TRUE ELSE FALSE END
```

Used in the bubble cluster chart to visually separate Nepal from peer countries
without removing others from the view.

## Filters

### Temporal Filter (Year)
- Drag Year to the Filters shelf
- Select Range of Values
- Set minimum 2000, maximum 2025
- Show filter as a slider for interactivity

### Country/Entity Filter
- Drag Country to the Filters shelf
- Use the Condition tab or a custom list to exclude regional aggregates
  like "World", "Africa", "Asia Pacific" etc.
- In this project 116 non-sovereign entries were excluded leaving sovereign UN members only

### Dashboard-Level Filter
- In the dashboard, click a sheet, go to the dropdown arrow on the sheet header
- Select "Use as Filter" so clicking a country on the map filters all other sheets

## Parameters

### Projected Demand Growth Rate
- Right-click the Data pane > Create Parameter
- Name: Projected Demand Growth Rate
- Data type: Float
- Current value: 0.1
- Allowable values: Range, Min -0.5, Max 1.0, Step 0.05
- Show the parameter control on the dashboard as a slider

Connect it to the calculated field above so stakeholders can drag the slider and
watch the scenario demand line update in real time across the chart.

### Year Stepper
- Create a Year parameter with integer type
- Set range 2000 to 2025
- Display as a stepper control
- Wire it to sheets using a calculated field:
  [Year] = [Year Parameter]
- Useful for the animated Sources Map and the Renewable Share Heatmap

## Level of Detail Expressions

LOD expressions let you compute aggregations at a different granularity than the view.

### Average Energy Per Capita (fixed to country, independent of year in view)

```
{ FIXED [Country] : AVG([Energy Per Capita]) }
```

### Country-level GDP average for scatter plots

```
{ FIXED [Country] : AVG([GDP Per Capita]) }
```

These are useful when you have year on the rows/columns but want a single
representative dot per country rather than one per year.

## Dashboard Interactivity Notes

- Use "Select View" parameter + CASE calculated field as a universal region slicer
- Wire it via a filter action: Worksheet > Actions > Add Action > Filter
- Set Source Sheet as any map or list, Target Sheets as all others
- This creates the linked behavior where selecting a region updates all charts at once

Tooltips can reference calculated fields directly.
Edit the tooltip from the Marks card and insert fields using the Insert menu.

## Data Cleaning Summary (done before Tableau)

| Step | Action |
|---|---|
| Format | CSV loaded as flat text, UTF-8 encoding confirmed |
| Temporal | Filtered to 2000-2025 |
| Columns | Reduced from 130 to 42 by removing zero-filled fields |
| Entities | Excluded 116 regional aggregates, kept sovereign states only |
| Derived | Calculated renewables share, scenario demand, regional group fields |

Before: 130 fields, 23,232 rows
After: 42 fields, 4,866 rows

## Viewing the Story and Dashboards

The workbook is organized as a Tableau Story. A Story is a sequence of story points,
each containing a dashboard or a single sheet with optional annotations.

To navigate the story in Tableau Public (browser or desktop):

- Open the workbook on Tableau Public
- At the top you will see a row of captioned boxes, one per story point
- Click each box to move between Dashboard 1, Dashboard 2, and Dashboard 3
- Each dashboard has its own filter state so the region selector resets per story point

To build a story in Tableau Desktop:
- Go to the bottom tab bar and click the New Story tab (book icon)
- Drag a dashboard from the left panel into the story point canvas
- Add a caption by clicking the grey text box above the canvas
- Click "Blank" on the left to add a new story point and drag the next dashboard in

## Screenshots

### Dashboard 1 – Energy Footprint and Economic Overview
Northern Africa region shown. Charts include avg energy per capita choropleth,
waterfall of energy consumption change, demand vs GDP scatter, and stacked area
resource chart.

![Dashboard 1](/screenshots/dashboard1.png)

---

### Dashboard 2 – Carbon Intensity and Energy Independence
Central America region shown. Charts include carbon intensity map, net import
share map, solar and wind share scatter, hydro share area chart, and primary
energy consumption line.

![Dashboard 2](/screenshots/dashboard2.png)

---

### Dashboard 3 – South Asia Peer Comparison (Nepal Focus)
SAARC region shown. Charts include sources map, renewable share heatmap,
top generating and consuming countries, energy and GDP cluster, and population
forecast with energy consumption forecast.

![Dashboard 3](/screenshots/dashboard3.png)

## References

- Tableau Help: help.tableau.com
- OWID Energy Data: github.com/owid/energy-data
