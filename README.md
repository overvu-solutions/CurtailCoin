# CurtailCoin
A web application that converts curtailed wind energy data into potential Bitcoin mining revenue, illustrating how much BTC could have been earned if wasted electricity were used to power mining.

## Detailed desctiption
Next.js application that visualizes wind energy curtailment data from the Elexon API.

## Core Architecture:
Next.js frontend framework
React for UI components
API calls to Elexon API (data.elexon.co.uk/bmrs/api/v1/)
Vercel for deployment
tailwind CSS for styling
recharts for data visualization
webpack for bundling

## Key Features:
### Date selection interface
### Real-time data fetching (48 half-hour settlement periods per day)
Each data point represents energy that was available but not used (Volume in MWh or Cost in GBP and BTC)
### Cost visualization with:
- Red bars showing costs of turning off wind turbines
- Orange bars showing potential Bitcoin mining revenue
### Data comparison and analysis functionality

## Main API Endpoints:
/bmrs/api/v1/balancing/settlement/stack/all/bid/{date}/{period}
/bmrs/api/v1/balancing/settlement/stack/all/offer/{date}/{period}

https://bmrs.elexon.co.uk/api-documentation
