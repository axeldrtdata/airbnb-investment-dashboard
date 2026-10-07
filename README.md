# Which cities are worth investing in on Airbnb?

A three-page Tableau dashboard that takes an investor from the global market down to a single neighbourhood, built on 478,000 cleaned listings across 16 countries.

**Full case study:** [axeldrtdata.github.io/works/airbnb-investment-dashboard](https://axeldrtdata.github.io/works/airbnb-investment-dashboard/)

## Context

An investor wants to buy a property and rent it out on Airbnb, with no geographic constraint. Which city should they choose? The dashboard answers in three steps that follow an investor's reasoning: the global market, the factors that make a city attractive, then a detailed comparison between cities. Every indicator is read against a reference (the median city or the national average), never on its own.

## Dashboard

| Page | Question | Main visuals |
| ---- | -------- | ------------ |
| **World** | What does the global market look like? | Listings per country, share of multi-owners, room type, property type and capacity, average yearly income |
| **Countries** | What makes a city worth it? | Occupancy vs price per city, airport distance vs price, top 10 cities with a minimum-listings slider, income by country with a dimension switch |
| **Cities** | Which city should I choose? | New hosts per year, price distribution, table of frictions (cleaning fees, minimum stay, strict cancellation), national KPIs as reference |

A PDF export is available in [`dashboard/`](dashboard/), together with the Tableau packaged workbook (`.twbx`, opens with Tableau Desktop or Tableau Public).

## Data preparation (Python)

- **Cleaning**: abnormal minimum stays, missing and zero prices, exact duplicates, missing structural variables imputed with conditional medians based on their correlations.
- **City names harmonised by hand**, country by country, with explicit correspondence dictionaries (e.g. Hong Kong appeared under 26 labels). 4,889 raw city labels became 1,060 cities with at least 10 listings.
- **Enrichment**: 2017 World Bank exchange rates to convert every price into USD, and the distance from each listing to the nearest large international airport (OurAirports), computed with a BallTree on great-circle distances.

## Key modelling choices (Tableau)

- Country + city as the key in every calculation, so that homonymous cities never merge.
- Dormant listings (zero availability, about 20% of the data) excluded from profitability but kept in the market size.
- A minimum-listings slider to keep only cities where averages are reliable.
- Occupancy is estimated from calendar availability: the yearly income is an upper bound, designed to compare cities rather than forecast revenue.

## Repository structure

```
airbnb-investment-dashboard/
├── notebooks/
│   └── 01-data-cleaning-and-enrichment.ipynb   # cleaning, exchange rates, airport distances
├── dashboard/
│   ├── airbnb-investment-dashboard.twbx         # Tableau packaged workbook (data included)
│   └── airbnb-investment-dashboard.pdf          # static export of the three pages
├── data/
│   ├── taux_conversion.csv                      # World Bank 2017 exchange rates and income groups
│   ├── airports.csv                             # OurAirports
│   └── raw/                                     # raw listings, to download from Kaggle (see README inside)
├── requirements.txt
└── README.md
```

## How to run it

```bash
git clone https://github.com/axeldrtdata/airbnb-investment-dashboard.git
cd airbnb-investment-dashboard
pip install -r requirements.txt
```

Download the raw listings into `data/raw/` (see the README in that folder), then open the project in VS Code and run the notebook. To explore the dashboard, open the `.twbx` file with Tableau Desktop or the free Tableau Public app.

## Stack

Python · pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Tableau · VS Code

## Data sources

Inside Airbnb listings (via [Kaggle](https://www.kaggle.com/datasets/joebeachcapital/airbnb)), World Bank exchange rates (2017), [OurAirports](https://ourairports.com/data/).

## Note

The notebook was first written in French during my OpenClassrooms Data Analyst training. Its text and comments have since been translated into English; variable and column names were kept as in the original data.

---

Axel Derobert · Data Analyst · [Portfolio](https://axeldrtdata.github.io) · [LinkedIn](https://www.linkedin.com/in/axel-derobert-5717463b1/)
