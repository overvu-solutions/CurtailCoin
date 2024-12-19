# CurtailCoin
A web application that converts curtailed wind energy data into potential Bitcoin mining revenue, illustrating how much BTC could have been earned if wasted electricity were used to power mining.

## Detailed description
Next.js application that visualizes wind energy curtailment data from the Elexon BMRS API.
It expands upon the concept demonstrated by [the Wasted Wind site](https://wastedwind.energy/). Whereas Wasted Wind shows MWh of curtailed wind energy, our app estimates how much Bitcoin could have been mined from that lost energy. By integrating real-time or recent curtailed wind data and applying a Bitcoin mining conversion algorithm, the application provides an "opportunity loss" metric in terms of BTC.

## Primary Goal:
Enable users to understand the Bitcoin mining opportunity forgone by curtailing wind energy instead of using it for Bitcoin mining.

## Why This Matters:
- to demonstrate a tangible use-case for otherwise wasted renewable energy.
- to educate users on the direct connection of energy intensity and profitability of Bitcoin mining.
- to provide a compelling narrative for renewable energy + Bitcoin mining synergy.

## Users & Use Cases
### Energy Enthusiasts / Environmentalists: 
Interested in understanding the opportunity cost of not using curtailed energy for productive economic activity.
### Bitcoin Miners & Investors: 
Want to see the potential BTC yield from a given amount of renewable energy.
### Policy Makers & Researchers: 
Assessing renewable energy utilization, curtailment, and novel use cases like on-site BTC mining.
### General Public: 
Curiosity about how “green” Bitcoin mining could be if it is integrated into wasted renewable resources.

### Primary Use Cases:
- Daily Overview: User selects a date and sees the total curtailed MWh and the corresponding BTC that could have been mined.
- Interval Detail: Show per hour (aggregate half-hour settlement period data), indicating MWh curtailed and BTC equivalent.

## Core Architecture:
- Next.js frontend framework
- React for UI components
- API calls to Elexon API (data.elexon.co.uk/bmrs/api/v1/)
- Vercel for deployment
- tailwind CSS for styling
- recharts for data visualization
- webpack for bundling

## Key Features:
### Date selection interface
### ASIC model selection (Future Enhancement)
Now a default S9 Bitmain model specs are used for conversion. In the future it will allow to select different models to understand how mining hardware efficiency corresponds to missed BTC opportunities.
### Real-time data fetching (48 half-hour settlement periods per day)
Each data point represents energy that was available but not used (Volume in MWh or Cost in GBP and BTC)
### Cost visualization with:
- Red bars showing costs of turning off wind turbines
- Orange bars showing potential Bitcoin mining revenue (converts MWh to an estimated BTC amount)
### Data comparison and analysis functionality

## Main API Endpoints:
- /bmrs/api/v1/balancing/settlement/stack/all/bid/{date}/{period}
- /bmrs/api/v1/balancing/settlement/stack/all/offer/{date}/{period}

## Bitcoin Conversion Calculation:
- Input: MWh curtailed.
- Calculation: 
  - Calculate total hash power producible by curtailed energy.
  - Estimate BTC mined given network difficulty and block reward.

- Involve standard assumptions for Bitcoin mining:
  - Hashrate per unit energy based on the reference miner (specification-based).
  - Bitcoin network difficulty for the selected date.
  - Block reward for selected date.

## Data Sources & Integrations
### Primary Data Source:
Elexon BMRS API (https://bmrs.elexon.co.uk/api-documentation):
Endpoints:
- GET https://data.elexon.co.uk/bmrs/api/v1/balancing/settlement/stack/all/bid/{YYYY-MM-DD}/{SettlementPeriod}
- GET https://data.elexon.co.uk/bmrs/api/v1/balancing/settlement/stack/all/offer/{YYYY-MM-DD}/{SettlementPeriod}

Returns JSON data containing volumes (both negative and positive), original/ final prices, and acceptance volumes from different generators and load points.

### Data Transformation:
- Filter only curtailed wind assets (negative volumes from wind generators).
- Aggregate per settlement period the total curtailed MWh.
- Convert MWh → BTC:
  - Calculate total hash power producible by that energy.
  - Estimate BTC mined given network difficulty and block reward assumptions.

### Backend Requirements:
- A simple server-side call in Next.js to fetch the required data from Elexon.
- Preprocessing to sum up curtailed MWh.
- A utility function to perform BTC calculation.

## Technical Requirements

### Front-End (Next.js):
Next.js 13+ with App Router for server-side rendering and static generation.
Tailwind CSS or styled-components.
Use Next.js server components or server-side API routes to fetch Elexon data.
Caching daily data for performance if needed.

### Back-End (Within Next.js):
- API route to fetch and aggregate Elexon data for a given date.
- Perform MWh to BTC calculation server-side to ensure consistency.

## UX / UI Requirements
### Home:
- Simple date picker (or a default to “today”).
- Button or form to request data.
### Result:
- Headline: "On {Date} (or Today if selected today), {X} MWh of wind energy was curtailed."
- Subtitle: "This could have mined approximately {Y} BTC."
- Graph of each houjr (summing half-hour periods period (1–48), showing:
  - Time interval (00, 01, 02 ... 24)
  - Curtailed energy in MWh
  - Estimated BTC from that energy.
- Footer/Explanations:
  - Short explanation of methodology.
  - Disclaimers (approximate calculation, static assumptions).
  - Link to Bitcoin Mining Calculator

