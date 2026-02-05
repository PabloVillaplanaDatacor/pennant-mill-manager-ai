# Product Requirements Document: Cost List Management

## 1. Feature Overview

### Feature Name
Cost List Management

### Problem Statement
Manufacturing organizations struggle to maintain accurate historical cost data, manage cost changes over time, and support multiple cost scenarios for decision-making. Without proper cost list management, companies face:

- Loss of historical cost data when prices are updated
- Inability to analyze cost trends over time
- Difficulty supporting financial audits requiring historical costs
- No mechanism to prepare future cost scenarios
- Inconsistent costs across sites or warehouses
- Inability to calculate accurate inventory valuations
- Spreadsheet-based cost management prone to errors

Cost List Management provides a structured, versioned approach to maintaining item costs that supports financial integrity, audit compliance, and operational planning.

### Target Users

**Primary Users:**
- **Cost Accountants**: Define and maintain item costs, create cost versions
- **Finance Managers**: Approve cost changes, analyze cost trends, support financial reporting
- **Purchasing Managers**: Update costs based on supplier pricing
- **System Administrators**: Configure cost list settings, manage cost data integrity

**Secondary Users:**
- **Production Planners**: Use costs for production estimates
- **Pricing Analysts**: Reference costs for pricing decisions
- **Inventory Managers**: Understand inventory valuation

### Business Value

**Financial Accuracy:**
- Accurate inventory valuation for financial statements
- Historical cost preservation for audit compliance
- Support FIFO, LIFO, or weighted average cost methods
- Eliminate cost data loss during updates

**Decision Support:**
- Scenario planning with future cost versions
- Cost trend analysis identifying cost drivers
- Make-vs-buy decisions based on accurate cost data
- Pricing strategy informed by true product costs

**Auditability:**
- Complete cost change history with user and timestamp
- Historical cost data retained indefinitely
- Support SOX and financial audit requirements
- Traceability from financial statements to source costs

**Operational Efficiency:**
- Bulk cost updates reduce manual effort
- Automated cost effective dating
- Multi-site cost management
- Integration with purchasing for cost updates

### Success Metrics

**Accuracy Metrics:**
- Cost data completeness: > 95%
- Cost update errors: < 1%
- Inventory valuation accuracy: 99.9%+

**Efficiency Metrics:**
- Time to create cost version: < 5 minutes
- Time to update 1,000 item costs: < 10 minutes
- Cost history retrieval: < 3 seconds

**Financial Impact:**
- Inventory valuation variance reduction: > 80%
- Financial close cycle time reduction: > 25%
- Cost estimation accuracy: > 95%

**Compliance:**
- Audit trail completeness: 100%
- Historical cost availability: 100%
- Cost change justification documentation: > 90%

## 2. User Stories & Acceptance Criteria

### US-CL-001: Create Cost List

```
As a Cost Accountant
I want to create a cost list with an effective date
So that item costs are organized by time period and purpose

Acceptance Criteria:
- System allows creating cost list with descriptive name
- System requires effective date (when costs become active)
- System optionally allows expiration date
- System supports multiple concurrent cost lists (standard, budget, scenario)
- System validates effective date logic (start before end)
- Created cost list is immediately available for item cost assignment
- System tracks who created cost list and when

Priority: Critical
User Impact: High
```

### US-CL-002: Define Item Costs in Cost List

```
As a Cost Accountant
I want to assign costs to items within a cost list
So that each item has a defined cost for the list's time period

Acceptance Criteria:
- System allows entering cost per item per cost list
- System supports costs by site (same item, different costs per site)
- System validates cost values (positive numbers)
- System displays items missing costs in list
- System allows bulk cost entry or import
- Cost changes within same version tracked in history
- System displays last cost update date and user per item

Priority: Critical
User Impact: High
```

### US-CL-003: Version Cost Lists

```
As a Finance Manager
I want to create new versions of cost lists
So that cost changes are tracked and historical costs are preserved

Acceptance Criteria:
- System creates new cost list version without deleting old version
- System copies costs from previous version to new version as starting point
- System increments version number automatically
- Old versions become read-only upon new version activation
- System clearly indicates which version is current
- Historical versions remain accessible for reference and audit
- Version changes tracked with user and timestamp

Priority: Critical
User Impact: High
```

### US-CL-004: Prevent Cost List Deletion

```
As a System Administrator
I want the system to prevent deletion of cost lists with dependencies
So that financial integrity and audit trails are protected

Acceptance Criteria:
- System blocks deletion if cost list used in inventory valuation
- System blocks deletion if cost list referenced in production orders
- System blocks deletion if cost list is historical (has transactions)
- System provides dependency report when deletion blocked
- System allows marking cost list as inactive instead of deletion
- All deletion attempts logged for audit
- Historical cost list versions cannot be deleted

Priority: Critical
User Impact: High
```

### US-CL-005: Bulk Update Item Costs

```
As a Cost Accountant
I want to update multiple item costs at once
So that cost updates from suppliers or cost changes can be applied efficiently

Acceptance Criteria:
- System supports CSV import for cost updates
- System validates all costs before applying any
- System displays preview of changes before committing
- System shows old vs new costs for review
- System allows percentage-based increases (e.g., +5% across items)
- Bulk update completes within 10 minutes for 1,000 items
- All changes logged with bulk update identifier

Priority: High
User Impact: High
```

### US-CL-006: View Cost History by Item

```
As a Cost Accountant
I want to view cost history for a specific item
So that I can analyze cost trends and support audit inquiries

Acceptance Criteria:
- System displays chronological cost changes for item
- Each record shows: effective date, cost value, cost list, user
- System supports filtering by date range or cost list
- System displays cost trend visualization (graph)
- System calculates cost change percentage between periods
- History export available for analysis
- History retrieval within 3 seconds

Priority: High
User Impact: Medium
```

### US-CL-007: Compare Cost Lists

```
As a Finance Manager
I want to compare costs between two cost lists
So that I can analyze cost variances or prepare budget scenarios

Acceptance Criteria:
- System allows selecting two cost lists for comparison
- System displays items with cost differences
- System calculates variance amount and percentage
- System highlights items with significant variances (e.g., >10%)
- System allows filtering comparison by item category or site
- Comparison results exportable to Excel
- Comparison completes within 5 seconds for 10,000 items

Priority: Medium
User Impact: Medium
```

### US-CL-008: Set Default Cost List

```
As a System Administrator
I want to designate a default cost list
So that cost calculations use consistent costs unless otherwise specified

Acceptance Criteria:
- System allows setting one cost list as default
- Default cost list used for inventory valuation by default
- Default cost list used for production cost estimation by default
- System clearly indicates which cost list is default
- Default can be changed with appropriate permissions
- Default change takes effect immediately
- Default cost list setting logged for audit

Priority: Medium
User Impact: Medium
```

### US-CL-009: Manage Site-Specific Costs

```
As a Multi-Site Cost Accountant
I want to define different costs for the same item across sites
So that regional cost differences are accurately reflected

Acceptance Criteria:
- System allows cost entry per item per site per cost list
- System displays site-specific costs clearly
- System uses site-specific cost for site-based transactions
- System allows bulk cost updates per site
- System supports copying costs from one site to another
- Site cost differences highlighted in reports

Priority: Medium
User Impact: High
```

### US-CL-010: Identify Items Missing Costs

```
As a Cost Accountant
I want to identify items without costs in a cost list
So that cost data completeness can be improved

Acceptance Criteria:
- System generates report of items missing costs
- Report filterable by site, item category, or classification
- Report shows impact (e.g., inventory value unknown)
- Report exportable for offline work
- System displays cost completeness percentage
- Missing costs flagged in inventory valuation reports

Priority: Medium
User Impact: Medium
```

## 3. Functional Requirements

### REQ-CL-001: Cost List Creation and Structure

**Priority**: Critical
**User Impact**: High

**Description**
Users must be able to create cost lists that organize item costs by time period, purpose, or scenario. Cost lists must support effective dating, versioning, and multi-site operations.

**User Flow**
1. User navigates to Cost List Management
2. User selects "Create New Cost List"
3. System prompts for cost list details
4. User enters name, description, effective date, optional expiration date
5. User selects cost list type (standard, budget, scenario)
6. User optionally specifies site (or global)
7. System validates inputs
8. User saves cost list
9. System creates empty cost list structure
10. User proceeds to add item costs

**Business Rules**
- Cost list name must be unique within site
- Effective date required, expiration date optional
- Effective date must be before expiration date
- Multiple cost lists can be effective simultaneously (for different purposes)
- Cost lists can be site-specific or global
- Cost list type determines usage (standard for valuation, budget for planning, scenario for analysis)
- Cost list creation tracked with user and timestamp
- Empty cost lists allowed (costs added subsequently)

**Data Requirements**

*Data to Capture:*
- Cost list name
- Description
- Effective date
- Expiration date (optional)
- Cost list type (standard, budget, scenario)
- Site assignment (specific site or global)
- Status (active, inactive, historical)
- Creation metadata (user, timestamp)

*Data to Display:*
- All cost list attributes
- Item count in cost list
- Cost completeness percentage
- Current/historical indicator
- Last updated date and user

*Immutable Data:*
- Cost list creation metadata
- Historical cost list versions

*Versioned Data:*
- Cost list versions tracked separately
- Each version has own effective date range

**Integration & Dependencies**

*Inventory Impact:*
- Cost lists used for inventory valuation
- Inventory value calculations depend on cost list data

*Costing Impact:*
- Cost lists are source of truth for item costs
- Production cost estimates use cost list data
- BOM cost rollups use costs from cost lists

*Production Impact:*
- Production order costs estimated from cost lists
- Actual costs compared to cost list standards

*External ERP Considerations:*
- Cost lists may synchronize to ERP
- ERP may have different cost list structures
- Effective dates must align with ERP fiscal periods

**Edge Cases & Error Scenarios**

*Overlapping Effective Dates:*
- User creates cost list with effective date overlapping existing list
- System allows (multiple lists can coexist) but warns if same type
- System provides clear guidance on which list applies when

*Effective Date in Past:*
- User sets effective date before today
- System allows but warns: "Effective date is in the past. Cost list will be historical immediately."

*Missing Expiration Date:*
- User creates cost list without expiration
- System assumes indefinite duration
- Cost list remains active until explicitly expired or superseded

### REQ-CL-002: Item Cost Assignment and Maintenance

**Priority**: Critical
**User Impact**: High

**Description**
Users must be able to assign and maintain costs for items within cost lists, supporting individual updates, bulk changes, and site-specific costs.

**User Flow**
1. User opens cost list
2. User searches for or selects item
3. User enters cost value for item
4. User optionally enters cost by site
5. System validates cost (positive number)
6. User saves cost
7. System records cost with effective date from cost list
8. System tracks cost change in history

**Business Rules**
- Costs must be positive numbers (zero allowed for no-cost items)
- Costs can be site-specific within cost list
- Items can have costs in multiple concurrent cost lists
- Cost updates within active cost list create history records
- Missing costs do not prevent cost list creation
- Cost precision supports at least 4 decimal places (e.g., $0.0123)
- Cost currency based on site or system default

**Data Requirements**

*Data to Capture:*
- Item identifier
- Cost value
- Currency
- Site (if site-specific)
- Effective date (from cost list)
- Last updated user and timestamp
- Cost source (manual, import, system calculated)

*Data to Display:*
- Current cost prominently
- Previous cost for comparison
- Cost change percentage
- Effective date
- Last update information
- Site assignment if applicable

*Immutable Data:*
- Historical cost values (in audit trail)

*Versioned Data:*
- Costs versioned with cost list versions

**Integration & Dependencies**

*Inventory Impact:*
- Item costs used for inventory valuation
- Inventory value recalculated when costs change

*Costing Impact:*
- BOM rollup uses component costs from cost list
- Cost variances calculated against cost list standards

*Production Impact:*
- Production order estimates use cost list costs

*External ERP Considerations:*
- Cost updates may synchronize to ERP
- ERP may push cost updates to PMM

**Edge Cases & Error Scenarios**

*Negative Cost Entry:*
- User enters negative cost
- System prevents: "Cost must be positive number or zero."

*Extremely High Cost:*
- User enters cost 1000x higher than previous cost
- System warns: "Cost increase is unusually large. Verify entry."

*Decimal Precision Limits:*
- User enters cost with 6 decimal places
- System rounds to 4 decimals and notifies user

*Site-Specific Cost Conflicts:*
- Item has cost in global cost list and site-specific cost list
- System uses most specific cost (site-specific takes precedence)

### REQ-CL-003: Cost List Versioning

**Priority**: Critical
**User Impact**: High

**Description**
Users must be able to create new versions of cost lists to manage cost changes over time while preserving historical cost data for audit and analysis.

**User Flow**
1. User opens active cost list
2. User selects "Create New Version"
3. System copies all item costs from current version to new version
4. System increments version number
5. User sets effective date for new version
6. User makes cost changes in new version
7. User activates new version
8. System marks previous version as historical (read-only)
9. New version becomes current for cost calculations

**Business Rules**
- Each cost list can have multiple versions
- Only one version can be active (current) at a time
- Historical versions are read-only
- New version copies costs from previous version as baseline
- Version numbering can be automatic (1, 2, 3) or semantic (2024.1, 2024.2)
- Effective date determines when version becomes active
- Historical versions retained indefinitely for audit
- Version transitions logged with user and timestamp

**Data Requirements**

*Data to Capture:*
- Version number
- Effective date
- Expiration date (optional)
- Status (draft, active, historical)
- Changes from previous version
- Version creation user and timestamp

*Data to Display:*
- Version history timeline
- Current active version highlighted
- Cost changes between versions (diff view)
- Version usage statistics

*Immutable Data:*
- Historical cost list versions
- Version creation metadata

*Versioned Data:*
- All item costs
- Cost list header information

**Integration & Dependencies**

*Inventory Impact:*
- Inventory valuations may use different versions for different periods
- Historical valuations reference historical versions

*Costing Impact:*
- Cost calculations use appropriate version based on date

*Production Impact:*
- Production orders may lock to cost list version at creation

*External ERP Considerations:*
- Version transitions may trigger ERP updates

**Edge Cases & Error Scenarios**

*Editing Historical Version:*
- User attempts to edit costs in historical version
- System prevents: "Cannot edit historical cost list version. Create new version to make changes."

*Activating Future Version:*
- User creates version with effective date in future
- System allows, version remains draft until effective date
- System automatically activates version on effective date

*Gap Between Versions:*
- Version 1 expires Jan 31, Version 2 effective Feb 15
- Gap of 2 weeks with no active cost list
- System warns about gap and suggests adjacent dates

### REQ-CL-004: Cost List Deletion Protection

**Priority**: Critical
**User Impact**: High

**Description**
The system must prevent deletion of cost lists with dependencies to protect financial integrity and audit trails.

**User Flow**
1. User selects cost list to delete
2. System performs comprehensive dependency check
3. If dependencies exist:
   - System blocks deletion
   - System displays dependency report
   - System suggests marking as inactive instead
4. If no dependencies:
   - System displays confirmation warning
   - User confirms deletion
   - System soft-deletes cost list
5. System logs deletion attempt with outcome

**Business Rules**
- Cost lists used in inventory valuation cannot be deleted
- Cost lists referenced in production orders cannot be deleted
- Historical cost lists cannot be deleted
- Cost lists with transaction history cannot be deleted
- Deletion attempts logged even when blocked
- Soft deletion preferred (retains data, marks as deleted)
- Inactive status recommended alternative to deletion

**Data Requirements**

*Data to Capture:*
- Deletion request timestamp and user
- Deletion outcome
- Blocking dependencies
- Alternative action taken

*Data to Display:*
- Clear deletion blocked message
- Dependency details
- Alternative options

*Immutable Data:*
- Deletion attempt log records

*Versioned Data:*
- Not applicable

**Integration & Dependencies**

*Inventory Impact:*
- Check inventory valuation usage
- Check historical inventory transactions

*Costing Impact:*
- Check cost calculations using cost list

*Production Impact:*
- Check production order cost references

*External ERP Considerations:*
- Deletion may require ERP synchronization

**Edge Cases & Error Scenarios**

*Delete Historical Cost List:*
- User attempts deletion
- Cost list used in 1,000 historical transactions
- System blocks: "Cannot delete. Cost list used in historical transactions."

*Delete with Future Effective Date:*
- Cost list not yet active but scheduled
- System allows deletion with confirmation

### REQ-CL-005: Bulk Cost Updates

**Priority**: High
**User Impact**: High

**Description**
Users must be able to update multiple item costs simultaneously through import or batch operations to support efficient cost maintenance.

**User Flow**
1. User opens cost list
2. User selects "Bulk Update Costs"
3. User chooses update method (import CSV or percentage change)
4. If import: user uploads CSV file with item codes and costs
5. If percentage: user enters percentage and item filter criteria
6. System validates all updates
7. System displays preview of changes (old vs new costs)
8. User reviews and confirms
9. System applies all changes
10. System displays update summary (items updated, errors)

**Business Rules**
- All costs validated before any applied (all-or-nothing)
- Invalid costs prevent entire update
- Update preview required before committing
- Percentage updates applied to filtered item set
- Bulk updates logged with batch identifier
- Update must complete within 10 minutes for 1,000 items
- Update failures rolled back completely

**Data Requirements**

*Data to Capture:*
- Update source (import file, percentage change)
- Items affected
- Old and new costs
- Update timestamp and user
- Batch identifier

*Data to Display:*
- Update preview table
- Validation errors
- Update progress indicator
- Success summary

*Immutable Data:*
- Bulk update log records

*Versioned Data:*
- Updated costs versioned with cost list

**Integration & Dependencies**

*Inventory Impact:*
- Inventory values recalculated after bulk cost update

*Costing Impact:*
- BOM rollups may need recalculation after update

*Production Impact:*
- Production estimates affected by cost changes

*External ERP Considerations:*
- Bulk updates may synchronize to ERP

**Edge Cases & Error Scenarios**

*Partial Validation Failure:*
- File contains 1,000 items, 10 invalid costs
- System rejects entire update
- Error report lists all invalid entries

*Percentage Update with Zero Base:*
- User applies +10% to items with zero cost
- Result still zero
- System warns about zero-cost items

*Large File Performance:*
- User uploads 10,000 item costs
- System displays progress indicator
- Validation may take several minutes

### REQ-CL-006: Cost History and Trend Analysis

**Priority**: High
**User Impact**: Medium

**Description**
Users must be able to view complete cost history for items and analyze cost trends over time to support decision-making and audit compliance.

**User Flow**
1. User selects item
2. User navigates to "Cost History"
3. System displays chronological cost changes
4. User optionally filters by date range or cost list
5. System displays cost trend visualization (graph)
6. User analyzes cost changes and identifies trends
7. User optionally exports history for further analysis

**Business Rules**
- Cost history includes all cost list versions
- History shows cost value, effective date, cost list, user
- History sorted chronologically (newest first or oldest first)
- Cost trend graph shows cost over time
- Percentage change calculated between periods
- History retrieval must complete within 3 seconds
- Export includes all metadata

**Data Requirements**

*Data to Capture:*
- Historical cost values
- Effective dates
- Cost lists
- Users who set costs

*Data to Display:*
- Chronological cost timeline
- Cost trend graph
- Percentage changes
- Cost list associations

*Immutable Data:*
- All historical cost records

*Versioned Data:*
- Not applicable (history is aggregation of versions)

**Integration & Dependencies**

*Inventory Impact:*
- Cost history supports inventory valuation analysis

*Costing Impact:*
- Cost trends inform future cost planning

*Production Impact:*
- Historical costs support variance analysis

*External ERP Considerations:*
- Cost history may include ERP-sourced costs

**Edge Cases & Error Scenarios**

*No Cost History:*
- Newly created item has no cost history
- System displays: "No cost history available for this item."

*Extremely Long History:*
- Item has 500 cost changes over 10 years
- System paginates history
- Graph aggregates by month or year for readability

### REQ-CL-007: Cost List Comparison

**Priority**: Medium
**User Impact**: Medium

**Description**
Users must be able to compare costs between two cost lists to identify variances, support budget analysis, and prepare cost scenarios.

**User Flow**
1. User navigates to Cost List Comparison
2. User selects first cost list (baseline)
3. User selects second cost list (comparison)
4. System calculates differences for all items
5. System displays items with cost differences
6. System shows variance amounts and percentages
7. User filters to significant variances (e.g., >10%)
8. User exports comparison for analysis

**Business Rules**
- Comparison shows items present in both cost lists
- Items only in one list flagged separately
- Variance calculated as (New - Old) / Old × 100%
- Comparison can filter by site, category, or threshold
- Comparison must complete within 5 seconds for 10,000 items
- Comparison results exportable to Excel

**Data Requirements**

*Data to Capture:*
- Cost lists being compared
- Comparison timestamp

*Data to Display:*
- Item code and description
- Cost in first list
- Cost in second list
- Variance amount
- Variance percentage
- Items only in one list

*Immutable Data:*
- Not applicable (comparison is real-time calculation)

*Versioned Data:*
- Comparison references specific cost list versions

**Integration & Dependencies**

*Inventory Impact:*
- Cost comparison supports inventory valuation analysis

*Costing Impact:*
- Comparison supports budget vs actual analysis

*Production Impact:*
- Comparison supports make-vs-buy decisions

*External ERP Considerations:*
- Comparison may include ERP cost data

**Edge Cases & Error Scenarios**

*Items Only in One List:*
- Baseline has 5,000 items, comparison has 5,200
- System displays 200 items unique to comparison list
- User can choose to include or exclude from analysis

*Zero Cost in Baseline:*
- Variance percentage undefined (divide by zero)
- System displays "N/A" or "New Cost"

*Identical Costs:*
- User compares cost list to itself
- System displays: "No variances found. Cost lists are identical."

## 4. Non-Functional Requirements (Business-Level)

### Performance Expectations
- Cost list view load: < 2 seconds
- Item cost update: < 1 second
- Bulk cost update (1,000 items): < 10 minutes
- Cost history retrieval: < 3 seconds
- Cost list comparison: < 5 seconds for 10,000 items

### Data Integrity and Auditability
- Cost history retention: indefinite
- All cost changes tracked with user and timestamp
- Audit trail export capability
- Financial statement support

### Security and Access Control
- Cost Accountant: Create, edit, version cost lists
- Finance Manager: Approve cost changes, view all costs
- Purchasing Manager: Update specific item costs
- Read-Only User: View costs only

### Compliance and Reporting
- Cost Master Report
- Cost Change Report
- Cost Variance Report
- Missing Cost Report
- Cost Trend Analysis Report

## 5. Dependencies & Constraints

**Cost Lists Depend On:**
- Items (costs assigned to items)
- Sites (costs may be site-specific)
- Currency definitions

**Cost Lists Referenced By:**
- Inventory Valuations
- BOM Cost Rollups
- Production Order Cost Estimates
- Financial Statements

## 6. Handoff Notes

**Critical Business Rules:**
- Historical cost lists are immutable
- Cost lists with transaction history cannot be deleted
- Cost versioning preserves financial integrity
- Multi-site costs support regional operations

**Performance Targets:**
- Bulk updates < 10 minutes for 1,000 items
- Cost history retrieval < 3 seconds
- Cost list comparison < 5 seconds for 10,000 items

**Integration Needs:**
- Inventory valuation calculations
- BOM cost rollup calculations
- Production cost estimation
- ERP synchronization

---

**Document Version**: 1.0
**Date**: 2026-02-05
**Author**: PMM Product Management Team
**Status**: Ready for Architecture Review
