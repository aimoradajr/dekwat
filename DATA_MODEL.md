# DEKWAT - Database Model

**Version:** 0.1.0  
**Database:** Supabase (PostgreSQL)  
**Last Updated:** September 21, 2025

## 🎯 Design Principles

- **Keep It Simple**: Only essential tables and fields
- **Easy to Query**: Optimized for filtering and sorting
- **Future-Proof**: Room for expansion without breaking changes
- **AI-Ready**: Dedicated space for assessment data

## 📊 Core Tables

### 1. `projects` (Main Table)

The primary table containing all project information for display and filtering.

```sql
CREATE TABLE projects (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,

  -- Basic Information
  project_reference VARCHAR(100) UNIQUE NOT NULL, -- PhilGEPS reference, etc.
  project_title TEXT NOT NULL,
  description TEXT,

  -- Agency & Location (for filtering)
  agency VARCHAR(255) NOT NULL,
  region VARCHAR(100) NOT NULL,
  province VARCHAR(100) NOT NULL,
  city_municipality VARCHAR(100) NOT NULL,

  -- Financial (for sorting & assessment)
  approved_budget DECIMAL(15,2), -- ABC (Approved Budget for Contract)
  winning_bid DECIMAL(15,2),
  final_project_value DECIMAL(15,2),

  -- Dates (for sorting & filtering)
  date_awarded DATE NOT NULL,
  date_started DATE,
  date_completed DATE,
  project_duration_days INTEGER,

  -- Project Details
  project_type VARCHAR(50) NOT NULL, -- 'Goods', 'Services', 'Infrastructure'
  procurement_method VARCHAR(100), -- 'Public Bidding', 'Direct Contracting', etc.
  project_status VARCHAR(50) NOT NULL, -- 'Active', 'Completed', 'Terminated', 'Under Review'

  -- Contractor Information
  contractor_name VARCHAR(255) NOT NULL,
  contractor_registration VARCHAR(100), -- Business registration number

  -- Key Officials (for tracking)
  authorizing_official VARCHAR(255),
  project_manager VARCHAR(255),

  -- Bidding Process
  number_of_bidders INTEGER,
  bid_opening_date DATE,

  -- System Fields
  data_source VARCHAR(100), -- 'PhilGEPS', 'Manual', 'COA', etc.
  source_url TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### 2. `ai_assessments` (AI Analysis Results)

Separate table for AI assessment results to keep the main table clean.

```sql
CREATE TABLE ai_assessments (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  project_id UUID REFERENCES projects(id) ON DELETE CASCADE,

  -- Overall Assessment
  risk_score INTEGER CHECK (risk_score >= 0 AND risk_score <= 100),
  risk_level VARCHAR(20), -- 'Green', 'Yellow', 'Orange', 'Red'
  assessment_status VARCHAR(20) DEFAULT 'Pending', -- 'Pending', 'Complete', 'Failed'

  -- Red Flags (JSON array for flexibility)
  red_flags JSONB DEFAULT '[]'::jsonb,

  -- Specific Assessments
  price_assessment JSONB, -- Price analysis results
  procurement_assessment JSONB, -- Bidding process analysis
  contractor_assessment JSONB, -- Contractor history analysis
  document_assessment JSONB, -- Document completeness check

  -- Assessment Metadata
  ai_model_version VARCHAR(50),
  assessment_date TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  confidence_score DECIMAL(3,2), -- 0.00 to 1.00

  -- Human Review
  human_reviewed BOOLEAN DEFAULT FALSE,
  reviewer_notes TEXT,
  reviewed_by VARCHAR(255),
  reviewed_at TIMESTAMP WITH TIME ZONE,

  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### 3. `project_documents` (Document Storage)

Track documents associated with projects.

```sql
CREATE TABLE project_documents (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  project_id UUID REFERENCES projects(id) ON DELETE CASCADE,

  document_type VARCHAR(100) NOT NULL, -- 'Contract', 'Technical_Specs', 'Bidding_Doc', etc.
  document_name VARCHAR(255) NOT NULL,
  file_path TEXT, -- Supabase storage path
  file_url TEXT, -- Public URL if available
  file_size INTEGER, -- in bytes
  mime_type VARCHAR(100),

  -- Document Status
  is_available BOOLEAN DEFAULT TRUE,
  extraction_status VARCHAR(20) DEFAULT 'Pending', -- 'Pending', 'Extracted', 'Failed'
  extracted_text TEXT, -- For search functionality

  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

## 📋 Lookup Tables (Optional - Start Simple)

You can start without these and add them later if needed:

### `agencies` (Normalize agency names)

```sql
CREATE TABLE agencies (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name VARCHAR(255) UNIQUE NOT NULL,
  abbreviation VARCHAR(20),
  department VARCHAR(255),
  region VARCHAR(100),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### `contractors` (Track contractor performance)

```sql
CREATE TABLE contractors (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name VARCHAR(255) UNIQUE NOT NULL,
  registration_number VARCHAR(100),
  business_type VARCHAR(100),
  address TEXT,
  contact_info JSONB,

  -- Performance Metrics (calculated)
  total_projects INTEGER DEFAULT 0,
  avg_performance_score DECIMAL(3,2),
  red_flag_count INTEGER DEFAULT 0,

  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

## 🔍 Essential Indexes

For fast querying and sorting:

```sql
-- Core filtering indexes
CREATE INDEX idx_projects_region ON projects(region);
CREATE INDEX idx_projects_province ON projects(province);
CREATE INDEX idx_projects_city ON projects(city_municipality);
CREATE INDEX idx_projects_agency ON projects(agency);
CREATE INDEX idx_projects_date_awarded ON projects(date_awarded);
CREATE INDEX idx_projects_status ON projects(project_status);
CREATE INDEX idx_projects_type ON projects(project_type);

-- Financial sorting
CREATE INDEX idx_projects_budget ON projects(approved_budget);
CREATE INDEX idx_projects_value ON projects(final_project_value);

-- Assessment filtering
CREATE INDEX idx_assessments_risk_level ON ai_assessments(risk_level);
CREATE INDEX idx_assessments_score ON ai_assessments(risk_score);
CREATE INDEX idx_assessments_status ON ai_assessments(assessment_status);

-- Text search
CREATE INDEX idx_projects_title_search ON projects USING gin(to_tsvector('english', project_title));
CREATE INDEX idx_projects_contractor_search ON projects USING gin(to_tsvector('english', contractor_name));
```

## 🔧 Supabase Setup Commands

### 1. Create Tables

Run these SQL commands in Supabase SQL Editor:

```sql
-- 1. Create projects table
CREATE TABLE projects (
  -- [Copy the projects table SQL from above]
);

-- 2. Create ai_assessments table
CREATE TABLE ai_assessments (
  -- [Copy the ai_assessments table SQL from above]
);

-- 3. Create project_documents table
CREATE TABLE project_documents (
  -- [Copy the project_documents table SQL from above]
);
```

### 2. Create Indexes

```sql
-- [Copy all the index commands from above]
```

### 3. Enable Row Level Security (RLS)

```sql
-- Enable RLS for public access (read-only)
ALTER TABLE projects ENABLE ROW LEVEL SECURITY;
ALTER TABLE ai_assessments ENABLE ROW LEVEL SECURITY;
ALTER TABLE project_documents ENABLE ROW LEVEL SECURITY;

-- Allow public read access
CREATE POLICY "Allow public read" ON projects FOR SELECT USING (true);
CREATE POLICY "Allow public read" ON ai_assessments FOR SELECT USING (true);
CREATE POLICY "Allow public read" ON project_documents FOR SELECT USING (true);
```

## 📝 Sample Data Structure

### Project Record Example:

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "project_reference": "PG-2025-001234",
  "project_title": "Construction of Classroom Building",
  "agency": "Department of Education - Region IV-A",
  "region": "CALABARZON",
  "province": "Laguna",
  "city_municipality": "Los Baños",
  "approved_budget": 5000000.0,
  "winning_bid": 4850000.0,
  "final_project_value": 4850000.0,
  "date_awarded": "2025-01-15",
  "project_type": "Infrastructure",
  "project_status": "Active",
  "contractor_name": "ABC Construction Corp"
}
```

### AI Assessment Example:

```json
{
  "project_id": "123e4567-e89b-12d3-a456-426614174000",
  "risk_score": 35,
  "risk_level": "Yellow",
  "red_flags": [
    "Single bidder scenario",
    "Project value 3% below approved budget"
  ],
  "assessment_status": "Complete"
}
```

## 🚀 Getting Started

### Phase 1: Basic Setup

1. Create the three core tables in Supabase
2. Add the essential indexes
3. Set up RLS policies for public read access

### Phase 2: Initial Data

1. Start with manual data entry or CSV import
2. Test querying and filtering functionality
3. Verify the table structure works for your UI

### Phase 3: AI Integration

1. Populate `ai_assessments` table with sample data
2. Test the assessment display functionality
3. Implement the AI scoring logic

### Phase 4: Optimization

1. Add lookup tables if needed
2. Optimize queries based on usage patterns
3. Add more indexes as needed

---

**Next Steps:**

1. Create these tables in your Supabase project
2. Import some sample project data
3. Test basic querying and filtering
4. Start building the Angular frontend to display this data

_Keep it simple - you can always add more fields and tables later as your needs evolve!_
