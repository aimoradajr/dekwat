# DEKWAT - Government Contracts Evaluation System

**Project Name:** DEKWAT (Derived from Filipino "dekwat" meaning "to assess/evaluate")  
**Version:** 0.1.0  
**Status:** Planning & Design Phase  
**Last Updated:** September 21, 2025

## 🎯 Project Overview

DEKWAT is an AI-powered government contracts evaluation system designed to analyze publicly available Philippine government contract data. The system automatically assesses contracts for potential irregularities, red flags, overpricing, and compliance issues to promote transparency and accountability in government procurement.

## 🚀 Project Goals

### Primary Objectives

- **Transparency Enhancement**: Provide public access to government contract analysis
- **Corruption Detection**: Identify potential irregularities and red flags in procurement processes
- **Data Aggregation**: Centralize scattered government contract information
- **AI-Powered Assessment**: Automated evaluation of contract fairness and compliance
- **Public Accountability**: Enable citizens to monitor government spending

### Success Metrics

- Number of contracts analyzed and assessed
- Accuracy of red flag detection
- User engagement and feedback
- Media/public attention to identified irregularities
- Government response to highlighted issues

## 🏗 System Architecture

### Technology Stack

#### Frontend

- **Framework**: Angular (Latest LTS)
- **UI Library**: Angular Material or PrimeNG
- **State Management**: NgRx (for complex state)
- **Charts/Visualization**: Chart.js or D3.js

#### Backend

- **Primary Database**: Supabase (PostgreSQL)
- **API Layer**: Python FastAPI or Node.js/Express
- **AI/ML Processing**: Python with scikit-learn, pandas, numpy
- **Data Processing**: Python scripts for ETL operations

#### Infrastructure

- **Hosting**: Vercel/Netlify (Frontend), Railway/Heroku (Backend)
- **Storage**: Supabase Storage for documents
- **Monitoring**: Supabase Analytics + Custom logging

### Data Flow

```
Government Sources → Data Scraping/API → Python ETL → Supabase → API → Angular Frontend
                                                    ↓
                                            AI Assessment Engine
```

## 📊 Data Requirements

### Contract Data Fields

#### Basic Information

- Contract ID/Reference Number
- Project Title/Description
- Contracting Agency/Department
- Contract Type (Goods, Services, Infrastructure)
- Date Awarded
- Contract Duration
- Status (Active, Completed, Terminated, Under Review)

#### Financial Data

- Approved Budget for Contract (ABC)
- Winning Bid Amount
- Final Contract Value
- Payment Status/History
- Budget Source/Funding

#### Location Data

- Region
- Province
- City/Municipality
- Specific Project Location (if applicable)

#### Bidding Process

- Procurement Method (Public Bidding, Direct Contracting, etc.)
- Number of Bidders
- Bid Opening Date
- Pre-qualification Requirements
- Technical Specifications

#### Stakeholders

- Winning Contractor/Supplier
- Contractor's Previous Government Contracts
- Authorizing Official
- Project Manager/Focal Person
- End User Department

#### Documents

- Contract Document (PDF)
- Technical Specifications
- Bidding Documents
- Performance Reports
- Delivery/Completion Reports

### Data Sources (Philippines)

- **PhilGEPS** (Philippine Government Electronic Procurement System)
- **DBM** (Department of Budget and Management) Reports
- **COA** (Commission on Audit) Annual Reports
- **Department/Agency Websites**
- **Transparency Portals**
- **GPPB** (Government Procurement Policy Board) Data
- **FOI** (Freedom of Information) Portal

## 🤖 AI Assessment Framework

### Red Flag Detection Categories

#### Pricing Irregularities

- Significantly above market price (>20% deviation)
- Below cost pricing (potential quality issues)
- Inconsistent pricing patterns
- Unusual price jumps between similar contracts

#### Procurement Process Issues

- Single bidder scenarios
- Frequent contract amendments
- Rushed procurement timelines
- Questionable technical specifications

#### Contractor Patterns

- High concentration of contracts to single entity
- Newly registered companies winning large contracts
- Companies with poor performance history
- Suspicious ownership patterns

#### Administrative Red Flags

- Incomplete documentation
- Missing required reports
- Delayed project delivery
- Frequent contract modifications

### Assessment Scoring System

- **Green (0-30)**: Clean, no significant issues
- **Yellow (31-60)**: Minor concerns, requires monitoring
- **Orange (61-80)**: Moderate red flags, needs investigation
- **Red (81-100)**: Serious irregularities, immediate attention required

### AI Models Required

1. **Price Analysis Model**: Market price comparison and deviation detection
2. **Pattern Recognition**: Identify suspicious bidding patterns
3. **Document Analysis**: Extract and validate key information from PDFs
4. **Anomaly Detection**: Statistical analysis of procurement data
5. **Risk Scoring**: Combine multiple factors into risk assessment

## 🎨 User Interface Design

### Main Dashboard

- **Overview Cards**: Total contracts, red flags, provinces covered
- **Interactive Map**: Philippines map with contract distribution
- **Recent Assessments**: Latest analyzed contracts
- **Trending Issues**: Most common red flags identified

### Contracts Table

#### Columns

- Project Title (searchable)
- Agency
- Province/City
- Contract Value
- Date Awarded
- Status
- **Risk Score** (color-coded badge)
- **Red Flags** (expandable list)
- **Assessment Status** (Complete, Pending, Failed)

#### Filtering & Sorting

- Location (Region, Province, City)
- Date range
- Contract value range
- Risk level
- Contract status
- Agency/Department
- Contractor

### Contract Detail View

- Complete contract information
- AI assessment breakdown
- Red flags explanation
- Historical contractor performance
- Similar contracts comparison
- Document viewer
- Report/Flag functionality

### Assessment Dashboard (Admin)

- Data ingestion status
- AI model performance metrics
- False positive/negative reports
- System health monitoring

## 🔧 Technical Implementation

### Phase 1: Foundation (Months 1-2)

- [ ] Set up development environment
- [ ] Initialize Angular frontend project
- [ ] Set up Supabase database
- [ ] Create basic data models
- [ ] Implement authentication system

### Phase 2: Data Pipeline (Months 2-3)

- [ ] Build data scraping tools
- [ ] Create ETL processes
- [ ] Implement data validation
- [ ] Set up automated data collection
- [ ] Basic contract display functionality

### Phase 3: AI Assessment (Months 3-4)

- [ ] Develop price analysis algorithms
- [ ] Implement pattern detection
- [ ] Create risk scoring system
- [ ] Build assessment dashboard
- [ ] Integrate AI results with frontend

### Phase 4: Enhancement (Months 4-5)

- [ ] Advanced filtering and search
- [ ] Data visualization and charts
- [ ] Performance optimization
- [ ] User feedback system
- [ ] Mobile responsiveness

### Phase 5: Deployment & Monitoring (Month 5-6)

- [ ] Production deployment
- [ ] Performance monitoring
- [ ] User testing and feedback
- [ ] Bug fixes and improvements
- [ ] Documentation and training

## ❓ Key Questions & Decisions

### Data Collection

1. **Legal Compliance**: What are the legal requirements for scraping government data?
2. **Data Access**: Which government portals provide API access vs. web scraping?
3. **Data Quality**: How to handle incomplete or inconsistent data?
4. **Update Frequency**: How often should data be refreshed?
5. **Historical Data**: How far back should we collect contract data?

### AI & Assessment

6. **Training Data**: What constitutes "normal" vs. "irregular" contracts for training?
7. **Model Accuracy**: What's the acceptable false positive/negative rate?
8. **Assessment Criteria**: Who validates the AI assessment criteria?
9. **Bias Prevention**: How to ensure AI doesn't discriminate against specific regions/agencies?
10. **Model Updates**: How often should AI models be retrained?

### Legal & Ethical

11. **Data Protection**: How to handle sensitive information in contracts?
12. **Liability**: What disclaimers are needed for AI assessments?
13. **Verification**: How to verify accuracy before publishing red flags?
14. **Appeal Process**: How can agencies respond to negative assessments?
15. **Transparency**: How much of the AI logic should be public?

### Technical

16. **Scalability**: How many contracts can the system handle?
17. **Performance**: What are acceptable response times for queries?
18. **Backup Strategy**: How to ensure data availability and recovery?
19. **Security**: How to protect against attacks and unauthorized access?
20. **Integration**: Should the system integrate with existing government systems?

### Business & Sustainability

21. **Funding**: How will the project be funded long-term?
22. **Governance**: Who will oversee the project and its decisions?
23. **Public Engagement**: How to encourage public use and feedback?
24. **Government Relations**: How to maintain cooperation with government agencies?
25. **Impact Measurement**: How to measure the system's effectiveness?

## 🛡 Risk Assessment

### Technical Risks

- **Data Quality**: Inconsistent or incomplete government data
- **System Reliability**: High availability requirements for public system
- **Scalability**: Growing dataset and user base
- **AI Accuracy**: False positives damaging reputations

### Legal Risks

- **Data Privacy**: Handling sensitive contract information
- **Defamation**: Incorrect red flag assessments
- **Government Relations**: Potential pushback from agencies
- **Compliance**: Changing regulations and requirements

### Operational Risks

- **Funding**: Sustainable financing model
- **Expertise**: AI/ML and government domain knowledge
- **Public Interest**: Maintaining user engagement
- **Political**: Changes in government attitudes toward transparency

## 🎯 Success Criteria

### Technical Success

- 95% system uptime
- <3 second page load times
- Successful analysis of 10,000+ contracts
- <5% false positive rate on red flags

### Impact Success

- Regular media coverage of identified issues
- Government responses to highlighted contracts
- Public engagement metrics (views, reports, feedback)
- Academic or research citations

### Sustainability Success

- Established funding model
- Active user community
- Government cooperation/data access
- Continuous improvement and updates

---

_This specification is a living document and should be updated as the project evolves and new requirements are identified._
