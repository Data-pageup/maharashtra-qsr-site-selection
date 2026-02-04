# Maharashtra QSR Site Selection Analysis

A data-driven approach to identifying optimal locations for Quick Service Restaurant (QSR) expansion in Maharashtra, India.

##  Overview

This project combines pincode-level census data with Point of Interest (POI) competition data to identify high-potential areas for QSR expansion across Maharashtra. The analysis uses demographic indicators, population density, and competitive landscape mapping to score and rank potential sites.

##  Project Objectives

- **Data Integration**: Merge multiple data layers (census, POI, competition data) at pincode granularity
- **Market Analysis**: Identify underserved markets with high population density
- **Competition Mapping**: Assess competitive intensity across different pincodes
- **Site Scoring**: Develop a multi-factor scoring system for site selection
- **Actionable Insights**: Generate ranked recommendations for expansion

##  Data Layers

### Layer 1: Pincode/Census Data
- **Source**: Census demographic data
- **Granularity**: Pincode level
- **Key Fields**:
  - Pincode
  - District
  - Population/Households
  - State information

### Layer 2: POI/Competition Data
- **Source**: Points of Interest database
- **Granularity**: Pincode level
- **Key Fields**:
  - Pincode
  - POI locations (restaurants, competitors, etc.)
  - Competition density

##  Getting Started

### Prerequisites

```python
pandas
numpy
matplotlib
seaborn
scikit-learn
```

### Installation

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Data Requirements

Place the following CSV files in your working directory:
- `5c2f62fe-5afa-4119-a499-fec9d604d5bd.csv` - Pincode/Census data
- `consolidated_pincode_census.csv` - POI/Competition data

##  Project Structure

```
maharashtra-qsr-site-selection/
│
├── README.md
├── requirements.txt
├── data/
│   ├── raw/
│   │   ├── pincode_census.csv
│   │   └── poi_competition.csv
│   
├── notebooks/
│   ├── 01_qsr_model.ipynb
│  
│
│
└── outputs/
    ├── reports/

```

##  Analysis Pipeline

### Phase 1: Data Acquisition and Preparation

```python
# Load and filter Maharashtra data
df_pincode = pd.read_csv('pincode_census.csv')
df_poi = pd.read_csv('poi_competition.csv')

# Filter for Maharashtra (pincodes starting with '4')
df_pincode_mh = df_pincode[df_pincode['Pincode'].astype(str).str.startswith('4')]
df_poi_mh = df_poi[df_poi['Pincode'].astype(str).str.startswith('4')]
```

**Key Steps**:
1. Load raw datasets
2. Filter Maharashtra records (pincode prefix '4')
3. Standardize pincode format (6-digit strings)
4. Remove invalid/missing pincodes
5. Merge datasets on pincode

**Output**: Clean master dataset with standardized pincodes

### Phase 2: Feature Engineering

- **Demographic Features**: Population density, household counts
- **Competition Features**: POI density, competitive intensity
- **Market Features**: Population-to-POI ratio, market saturation
- **Geographic Features**: District-level aggregations

### Phase 3: Scoring and Ranking

Multi-factor scoring based on:
- Population potential (40% weight)
- Competition intensity (30% weight)
- Market gaps (20% weight)
- Accessibility indicators (10% weight)

### Phase 4: Visualization and Reporting

- Heatmaps of opportunity scores
- District-level comparisons
- Top-N site recommendations
- Interactive dashboards

##  Key Metrics

| Metric | Description | Calculation |
|--------|-------------|-------------|
| **Population Density** | People per pincode area | Total Population / Area |
| **POI Density** | Competition concentration | POI Count / Pincode |
| **Market Gap Score** | Underserved market indicator | Population / POI_Count |
| **Opportunity Score** | Overall site potential | Weighted combination |

##  Sample Outputs

### Top 10 Recommended Pincodes

```
Rank | Pincode | District  | Population | POI_Count | Opportunity_Score
-----|---------|-----------|------------|-----------|------------------
1    | 400001  | Mumbai    | 125,000    | 15        | 92.5
2    | 411014  | Pune      | 98,000     | 12        | 88.3
3    | 422001  | Nashik    | 87,500     | 8         | 85.7
...
```

##  Data Processing Steps

### 1. Pincode Standardization

```python
def standardize_pincode(df, pincode_col):
    """Standardize pincode to 6-digit string format"""
    df[pincode_col] = df[pincode_col].astype(str).str.replace('.0', '', regex=False)
    df[pincode_col] = df[pincode_col].str.strip()
    df[pincode_col] = df[pincode_col].apply(
        lambda x: x if len(x) == 6 and x.isdigit() else np.nan
    )
    return df.dropna(subset=[pincode_col])
```

### 2. Data Aggregation

```python
# Aggregate by pincode
df_master = df_pincode.groupby('Pincode').agg({
    'District': 'first',
    'Population': 'sum',
    'POI_Count': 'sum'
}).reset_index()
```

##  Expected Results

- **Coverage**: ~15,000-20,000 unique Maharashtra pincodes
- **Analysis Ready**: Merged dataset with population and competition metrics
- **Actionable Insights**: Ranked list of expansion opportunities
- **Visual Reports**: Maps and charts highlighting optimal locations

##  Data Quality Checks

✓ Pincode format validation (6 digits)  
✓ State filtering (Maharashtra only)  
✓ Duplicate removal  
✓ Missing value handling  
✓ Data type consistency  
✓ Outlier detection  

##  Use Cases

1. **QSR Chain Expansion**: Identify next 10-50 locations for expansion
2. **Market Entry Strategy**: Prioritize districts for new market entry
3. **Competitive Analysis**: Understand market saturation by area
4. **Investment Planning**: Allocate resources based on opportunity scores
5. **Performance Benchmarking**: Compare actual vs. predicted performance

##  Notes and Assumptions

- **Pincode Prefix**: Maharashtra pincodes start with '4'
- **Population Estimation**: When unavailable, estimated at 5,000 people per post office
- **POI Count**: Missing values filled with 0 (assumes no competition)
- **Geographic Scope**: Analysis limited to Maharashtra state
- **Data Currency**: Results depend on data freshness; recommend quarterly updates

##  Contributing

This is an analytical framework. To adapt for your needs:

1. Update file paths to match your data sources
2. Adjust scoring weights based on business priorities
3. Add additional data layers (income, traffic, real estate)
4. Customize visualization preferences
5. Extend feature engineering based on domain knowledge

##  Contact & Support

For questions about the methodology or implementation:
- Review the code comments for detailed explanations
- Check data quality reports in the outputs folder
- Consult domain experts for market-specific insights

##  Version History

- **v1.0**: Initial data acquisition and preparation pipeline

##  License

This project is designed for internal business analysis and site selection purposes.

---

**Last Updated**: February 2026  
