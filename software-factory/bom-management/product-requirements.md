# Product Requirements Document: Bill of Materials (BOM) Management

## 1. Feature Overview

### Feature Name
Bill of Materials (BOM) Management

### Problem Statement
Manufacturing organizations struggle to maintain accurate production recipes, leading to material waste, incorrect cost calculations, production delays, and quality issues. Without structured BOM management, companies face:

- BOMs maintained in spreadsheets causing version control issues
- Incorrect material consumption leading to inventory discrepancies
- Inability to calculate accurate product costs
- No traceability from finished goods back to raw materials
- Changes to BOMs not communicated to production floor
- Multi-level product structures becoming unmanageable
- Engineering changes not reflected in production
- Duplicate or conflicting BOMs for same product

A robust BOM management system ensures that production recipes are accurate, versioned, traceable, and directly drive material consumption and cost calculations.

### Target Users

**Primary Users:**
- **Manufacturing Engineers**: Define and maintain BOMs, ensure technical accuracy
- **Production Planners**: Use BOMs to plan material requirements
- **Cost Accountants**: Use BOMs for cost rollup calculations
- **Quality Managers**: Ensure BOMs reflect quality standards

**Secondary Users:**
- **Purchasing Team**: Use BOM component lists for procurement planning
- **Inventory Managers**: Understand material consumption patterns
- **Production Supervisors**: Reference BOMs during production execution
- **Product Engineers**: Update BOMs based on design changes

### Business Value

**Cost Accuracy:**
- Accurate cost rollups from components to finished goods
- Visibility into cost drivers and opportunities for cost reduction
- Support make-vs-buy decisions with complete cost data
- Enable scenario planning with BOM variations

**Operational Efficiency:**
- Single source of truth for production recipes eliminates confusion
- Automated material requirement calculations save planning time
- Reduced material waste through accurate consumption standards
- Faster new product introduction with structured BOM creation

**Quality and Compliance:**
- Complete traceability from finished goods to raw materials
- Consistent production across shifts and sites
- Engineering change management with version control
- Audit trail supporting regulatory compliance (FDA, ISO, etc.)

**Production Planning:**
- Accurate material requirements for production schedules
- Visibility into component availability before production starts
- Multi-level BOM explosion for complex assemblies
- Capacity planning based on production standards

### Success Metrics

**Accuracy Metrics:**
- BOM accuracy (matches actual production): > 98%
- Cost calculation accuracy: > 99%
- Material consumption variance: < 2%

**Efficiency Metrics:**
- Time to create new BOM: < 15 minutes
- BOM explosion calculation time: < 5 seconds for 10-level deep structures
- Material requirement calculation time: < 10 seconds

**Operational Impact:**
- Material waste reduction: > 20%
- Production delays due to missing components: -50%
- Cost estimation accuracy improvement: > 30%

**Adoption Metrics:**
- BOM completeness for manufactured items: > 95%
- User satisfaction: > 4.0/5.0
- Engineering change cycle time reduction: > 40%

## 2. User Stories & Acceptance Criteria

### US-BOM-001: Create Bill of Materials

```
As a Manufacturing Engineer
I want to create a bill of materials for a manufactured item
So that production knows exactly what materials and quantities are required

Acceptance Criteria:
- System validates that parent item is classified as Manufactured or Both
- System allows adding multiple component items with quantities
- System validates that component items exist and are available
- System prevents circular BOM references (item as own component)
- System calculates total cost based on component costs and quantities
- System saves BOM with version number and effective date
- Created BOM is immediately available for production planning

Priority: Critical
User Impact: High
```

### US-BOM-002: Define Multi-Level BOM Structure

```
As a Manufacturing Engineer
I want to create multi-level BOM structures with sub-assemblies
So that complex products with intermediate components are accurately represented

Acceptance Criteria:
- System supports unlimited BOM levels (parent → sub-assembly → component)
- System displays BOM hierarchy visually (tree or indented list)
- System performs BOM explosion to show all levels
- System calculates total material requirements across all levels
- System detects and prevents circular references at any level
- System shows where-used relationships (which BOMs use a component)

Priority: Critical
User Impact: High
```

### US-BOM-003: Version BOM Changes

```
As a Manufacturing Engineer
I want to create new versions of BOMs when changes are needed
So that historical production uses old version while new production uses updated recipe

Acceptance Criteria:
- System creates new BOM version without deleting old version
- Each version has effective date and optional expiration date
- System allows selecting which version to use for production
- System prevents editing of historical BOM versions
- System tracks who created each version and when
- Active production orders reference specific BOM versions

Priority: Critical
User Impact: High
```

### US-BOM-004: Prevent Deletion of Active BOMs

```
As a System Administrator
I want the system to prevent deletion of BOMs referenced by production or inventory
So that operational integrity is maintained

Acceptance Criteria:
- System blocks deletion if BOM is referenced by active production orders
- System blocks deletion if BOM is current version for active item
- System provides detailed dependency report when deletion blocked
- System allows marking BOM as inactive instead of deletion
- System logs all deletion attempts
- Historical BOM versions cannot be deleted even if inactive

Priority: Critical
User Impact: High
```

### US-BOM-005: Calculate BOM Cost Rollup

```
As a Cost Accountant
I want the system to automatically calculate finished good costs from BOM components
So that product costs are accurate and reflect current material costs

Acceptance Criteria:
- System calculates total material cost from all component costs × quantities
- System performs multi-level cost rollup for sub-assemblies
- System updates finished good cost when component costs change
- System shows cost breakdown by component
- System identifies components missing cost data
- Cost rollup completes within 5 seconds for complex BOMs

Priority: High
User Impact: High
```

### US-BOM-006: Explode BOM for Material Requirements

```
As a Production Planner
I want to explode a BOM to see all required materials
So that I can ensure all components are available before starting production

Acceptance Criteria:
- System displays all components across all levels (flattened view)
- System calculates total quantities needed for specified production quantity
- System shows current inventory availability for each component
- System highlights components with insufficient inventory
- System accounts for component lead times
- Explosion completes within 5 seconds for 10+ level BOMs

Priority: High
User Impact: High
```

### US-BOM-007: Manage BOM by Site

```
As a Multi-Site Manufacturing Manager
I want to define different BOMs for the same item across different sites
So that regional production differences are supported

Acceptance Criteria:
- System allows site-specific BOM definitions
- System clearly indicates which BOMs apply to which sites
- System prevents use of wrong-site BOM in production
- System allows copying BOM from one site to another
- Site-specific BOMs can have different components and quantities
- Default BOM can be defined for items produced in multiple sites

Priority: High
User Impact: High
```

### US-BOM-008: View Where-Used for Components

```
As a Manufacturing Engineer
I want to see which BOMs use a specific component
So that I understand the impact of component changes or shortages

Acceptance Criteria:
- System displays all parent items that use selected component
- System shows quantity required in each BOM
- System indicates active vs inactive BOMs
- System supports multi-level where-used (grand-parent items)
- System allows filtering by site or BOM status
- Where-used results load within 3 seconds

Priority: High
User Impact: Medium
```

### US-BOM-009: Copy Existing BOM

```
As a Manufacturing Engineer
I want to copy an existing BOM to create a new one
So that similar products can be defined quickly without re-entering components

Acceptance Criteria:
- System provides "Copy BOM" function
- User specifies new parent item for copied BOM
- System copies all components and quantities
- System validates new parent item is manufacturable
- User can edit copied BOM before saving
- Copy operation completes within 10 seconds

Priority: Medium
User Impact: Medium
```

### US-BOM-010: Define Component Scrap/Waste Factor

```
As a Manufacturing Engineer
I want to define scrap or waste percentages for components
So that material requirements account for normal production losses

Acceptance Criteria:
- System allows scrap percentage per component (e.g., 5%)
- System calculates adjusted quantity required (base quantity + scrap)
- System displays both base and adjusted quantities
- Scrap factors are version-specific
- System uses adjusted quantities for material requirements
- Scrap factors supported for cost rollup calculations

Priority: Medium
User Impact: Medium
```

## 3. Functional Requirements

### REQ-BOM-001: BOM Creation and Structure Definition

**Priority**: Critical
**User Impact**: High

**Description**
Users must be able to create bills of materials that define how finished goods or sub-assemblies are manufactured. The system must enforce structural integrity, prevent invalid relationships, and support multi-level hierarchies.

**User Flow**
1. User navigates to BOM Management
2. User selects "Create New BOM"
3. System prompts for parent item selection
4. User selects parent item (must be Manufactured or Both classification)
5. System validates parent item eligibility
6. User adds component items one by one or via bulk add
7. For each component, user enters quantity required
8. System validates component availability and classification
9. User optionally defines scrap factors, operation sequences, notes
10. System performs BOM validation (circular reference check, duplicate components)
11. User saves BOM
12. System assigns version number (typically 1.0 for new BOM)
13. System displays confirmation and BOM summary

**Business Rules**
- Parent item must be classified as Manufactured or Both
- Component items must exist in item master
- Components can be Purchased, Manufactured, or Both
- Circular references prohibited (item cannot be own component, directly or indirectly)
- Duplicate components within same BOM not allowed (combine quantities)
- Component quantities must be positive numbers
- BOM must have at least one component
- Each BOM has version number and effective date
- New BOMs default to active status
- BOM creation tracked with user and timestamp

**Data Requirements**

*Data to Capture:*
- Parent item (manufactured item)
- Component items (materials, sub-assemblies)
- Component quantities (per unit of parent)
- Component sequence/order (optional)
- Scrap/waste factors per component (optional)
- BOM version number
- Effective date (when BOM becomes active)
- Expiration date (optional, when BOM becomes inactive)
- Active/Inactive status
- Site assignment (if site-specific)
- Creation and modification metadata (user, timestamp)

*Data to Display:*
- Parent item with description
- Component list with descriptions and quantities
- Total estimated cost (rollup)
- BOM hierarchy visualization
- Version information
- Effective/expiration dates
- Validation errors during creation

*Immutable Data:*
- Historical BOM versions (cannot be edited once new version created)
- BOM creation metadata

*Versioned Data:*
- Each BOM change creates new version
- Version history maintained indefinitely

**Integration & Dependencies**

*Inventory Impact:*
- BOMs drive material consumption during production
- Component availability checked during production planning
- Inventory transactions reference BOM versions used

*Costing Impact:*
- BOMs enable cost rollup calculations
- Component cost changes trigger parent cost recalculation
- BOM versions preserve historical cost accuracy

*Production Impact:*
- Production orders reference specific BOM versions
- Material requirements calculated from BOMs
- Production cannot proceed without valid BOM

*External ERP Considerations:*
- BOMs may synchronize to ERP systems
- ERP may have different BOM structure requirements
- BOM versions may need to map to ERP revisions

**Edge Cases & Error Scenarios**

*Circular BOM Reference:*
- User adds Item A to BOM for Item A (direct circular)
- System detects and blocks: "Circular reference detected. Item cannot be its own component."
- User adds Item B to BOM for Item A, and Item A to BOM for Item B (indirect circular)
- System detects multi-level circular reference and blocks

*Parent Item Not Manufacturable:*
- User attempts to create BOM for Purchased-only item
- System blocks: "Cannot create BOM. Parent item must be classified as Manufactured or Both."

*Component Item Does Not Exist:*
- User enters non-existent component item code
- System displays: "Component item [CODE] not found. Verify item code."

*Zero or Negative Quantity:*
- User enters quantity of 0 or -5 for component
- System prevents: "Component quantity must be positive number greater than zero."

*Duplicate Component:*
- User adds same component twice to BOM
- System detects and prompts: "Component already exists in BOM. Update quantity instead?"

### REQ-BOM-002: BOM Versioning and Change Management

**Priority**: Critical
**User Impact**: High

**Description**
Users must be able to version BOMs to manage engineering changes while preserving historical accuracy. The system must ensure production orders use correct BOM versions and support seamless version transitions.

**User Flow**
1. User opens existing BOM
2. User selects "Create New Version"
3. System copies current BOM to new version
4. System increments version number (e.g., 1.0 → 2.0)
5. User makes changes to new version (add/remove components, change quantities)
6. User sets effective date for new version
7. User optionally sets expiration date for old version
8. System validates new version
9. User saves new version
10. System marks old version as historical (read-only)
11. New production orders use new version after effective date
12. Existing production orders continue using their assigned version

**Business Rules**
- Each BOM version has unique version number (major.minor or sequential)
- Historical versions cannot be edited (read-only)
- Only one version can be "current" for production at any time
- Effective date determines when version becomes active
- Old versions remain accessible for historical reference and active production orders
- Version changes tracked with user and timestamp
- System can have multiple versions with different effective dates (future versions)
- Production orders lock to specific BOM version at creation time

**Data Requirements**

*Data to Capture:*
- Version number
- Effective date (when version becomes current)
- Expiration date (when version becomes historical)
- Version status (draft, active, historical)
- Changes from previous version (change log)
- User who created version
- Version creation timestamp
- Reason for change (optional note)

*Data to Display:*
- Version history timeline
- Current active version prominently marked
- Changes between versions (diff view)
- Effective and expiration dates
- Version usage (how many production orders used this version)

*Immutable Data:*
- Historical BOM versions (cannot be modified)
- Version creation metadata

*Versioned Data:*
- All BOM components and quantities
- BOM header information
- Site assignments

**Integration & Dependencies**

*Inventory Impact:*
- Material consumption uses BOM version from production order
- Inventory traceability links to specific BOM versions

*Costing Impact:*
- Cost calculations use appropriate BOM version
- Historical costs reference historical BOM versions
- Cost rollup recalculated for new versions

*Production Impact:*
- Production orders lock to BOM version at creation
- Active production continues with original version even after new version effective
- New production orders use current version based on effective date

*External ERP Considerations:*
- ERP BOM revisions synchronized with PMM versions
- Version transitions may trigger ERP updates
- Version history maintained for audit compliance

**Edge Cases & Error Scenarios**

*Editing Historical Version:*
- User attempts to edit old BOM version
- System prevents: "Cannot edit historical BOM version. Create new version to make changes."

*Effective Date in Past:*
- User sets effective date before today
- System allows but warns: "Effective date is in the past. New version will become current immediately."

*Multiple Active Versions:*
- System configuration error causes two versions to be active simultaneously
- System detects conflict and alerts administrator
- User must resolve by setting correct effective/expiration dates

*Production Order with Expired BOM:*
- Production order created with version 1.0
- Version 2.0 becomes active before production completes
- Production continues with version 1.0 (locked version)
- System displays indicator that order uses historical version

*Version Numbering Conflicts:*
- User manually enters version number that already exists
- System detects and suggests next available version number

### REQ-BOM-003: BOM Deletion Protection

**Priority**: Critical
**User Impact**: High

**Description**
The system must prevent deletion of BOMs that have dependencies to protect operational and financial integrity. Users must understand why deletion is blocked and what alternatives exist.

**User Flow**
1. User selects BOM to delete
2. System performs dependency check
3. If dependencies exist:
   - System blocks deletion
   - System displays dependency report (production orders, cost calculations)
   - System suggests marking BOM as inactive
4. If no dependencies:
   - System displays confirmation with warning
   - User confirms deletion intent
   - System soft-deletes BOM (retains for audit)
5. System logs deletion attempt with outcome

**Business Rules**
- BOMs referenced by active production orders cannot be deleted
- BOMs referenced by historical production cannot be deleted
- Current/active BOM versions cannot be deleted
- Historical BOM versions cannot be deleted
- All deletion attempts logged even when blocked
- Soft deletion preferred over hard deletion
- "Inactive" status recommended alternative to deletion

**Data Requirements**

*Data to Capture:*
- Deletion request timestamp and user
- Deletion outcome (successful, blocked, cancelled)
- Blocking dependencies
- Alternative action taken

*Data to Display:*
- Clear deletion blocked message
- List of dependencies preventing deletion
- Production order count using BOM
- Alternative actions (mark inactive)

*Immutable Data:*
- Deletion attempt log records
- Historical BOMs

*Versioned Data:*
- Not applicable

**Integration & Dependencies**

*Inventory Impact:*
- Check for inventory transactions referencing BOM

*Costing Impact:*
- Check for cost calculations using BOM
- Historical cost data depends on BOM versions

*Production Impact:*
- Check for production orders using BOM
- Check for pending production schedules

*External ERP Considerations:*
- Deletion may require ERP synchronization
- ERP may prevent BOM deletion independently
- Cross-system referential integrity maintained

**Edge Cases & Error Scenarios**

*Delete BOM with Completed Production:*
- User attempts deletion
- BOM was used in 100 completed production orders
- System blocks: "Cannot delete. BOM used in 100 historical production orders."

*Delete Inactive Old Version:*
- User attempts to delete old inactive BOM version
- No active production references it
- System still blocks: "Historical BOM versions cannot be deleted to preserve audit trail."

*Multi-Version Deletion:*
- User attempts to delete parent item which deletes all BOM versions
- System blocks based on any version having dependencies

### REQ-BOM-004: BOM Cost Rollup Calculation

**Priority**: High
**User Impact**: High

**Description**
The system must automatically calculate finished good costs by rolling up component costs, supporting multi-level structures and handling missing cost data gracefully.

**User Flow**
1. System detects BOM change or component cost change
2. System initiates cost rollup calculation
3. System retrieves current costs for all components
4. System calculates total material cost (component cost × quantity)
5. For sub-assembly components, system performs recursive cost rollup
6. System identifies components missing cost data
7. System calculates total BOM cost
8. System optionally adds labor, overhead, or other costs (if configured)
9. System updates parent item's calculated cost
10. System displays cost breakdown
11. System logs cost calculation with timestamp

**Business Rules**
- Cost rollup uses current component costs by default
- Multi-level cost rollup calculates sub-assembly costs first
- Missing component costs flagged but don't prevent calculation
- Scrap/waste factors included in cost calculation (quantity × (1 + scrap%))
- Cost rollup triggered automatically when component costs change
- Manual cost rollup can be initiated by user
- Cost rollup uses costs from same site as BOM
- Historical cost rollup uses component costs from specific date

**Data Requirements**

*Data to Capture:*
- Component costs used in calculation
- Calculated total cost
- Cost calculation timestamp
- Components missing cost data
- Scrap factors applied
- Labor/overhead additions (if applicable)

*Data to Display:*
- Total BOM cost prominently
- Cost breakdown by component
- Component cost × quantity for each
- Missing cost warnings
- Cost calculation date
- Comparison to previous calculated cost

*Immutable Data:*
- Historical cost rollup results

*Versioned Data:*
- Cost calculations tied to BOM versions

**Integration & Dependencies**

*Inventory Impact:*
- Inventory valuation may use BOM cost rollups

*Costing Impact:*
- Component costs from cost lists
- Parent item standard cost may be set from BOM rollup
- Cost variance analysis uses BOM costs

*Production Impact:*
- Production order costs estimated from BOM rollup
- Actual production costs compared to BOM standard

*External ERP Considerations:*
- Rolled up costs may synchronize to ERP
- ERP may have different costing methods

**Edge Cases & Error Scenarios**

*Component Missing Cost:*
- Component has no cost defined
- System calculates partial cost and warns: "3 components missing cost data. Total may be incomplete."

*Circular BOM in Cost Rollup:*
- Should not occur if circular BOMs prevented, but safety check included
- System detects circular reference and aborts: "Cost rollup failed. Circular BOM reference detected."

*Extreme Cost Changes:*
- Component cost increased 500%
- BOM cost rollup increases dramatically
- System flags large variance and alerts user

*Multi-Currency Costs:*
- Components have costs in different currencies (multi-site scenario)
- System converts to common currency before rollup
- Exchange rate and conversion displayed

### REQ-BOM-005: BOM Explosion and Material Requirements

**Priority**: High
**User Impact**: High

**Description**
Users must be able to explode BOMs to view all required materials across all levels and calculate total quantities needed for production. The system must support complex multi-level structures and perform calculations efficiently.

**User Flow**
1. User selects BOM to explode
2. User enters production quantity (e.g., 100 units)
3. System initiates BOM explosion
4. System recursively traverses all BOM levels
5. System calculates total quantities for each unique component
6. System accounts for scrap/waste factors
7. System optionally checks inventory availability
8. System displays flattened material requirements list
9. System highlights components with insufficient inventory
10. User exports material requirements or initiates procurement

**Business Rules**
- BOM explosion traverses all levels recursively
- Quantities multiplied at each level (parent qty × component qty)
- Duplicate components consolidated (total quantity summed)
- Scrap factors applied to quantities
- Explosion uses current BOM versions by default (or specified version)
- Explosion must complete within 5 seconds for 10+ level BOMs
- Inventory availability check optional but recommended
- Explosion results can be exported for procurement

**Data Requirements**

*Data to Capture:*
- Production quantity requested
- BOM version used for explosion
- Explosion timestamp

*Data to Display:*
- Flattened component list
- Total quantity required per component
- BOM level where component appears
- Current inventory availability
- Shortage quantities
- Lead times (if available)
- Estimated total cost

*Immutable Data:*
- Not applicable (explosion is real-time calculation)

*Versioned Data:*
- Explosion tied to specific BOM versions

**Integration & Dependencies**

*Inventory Impact:*
- Explosion checks inventory availability
- Material requirements drive purchase requisitions
- Inventory reservations may be created

*Costing Impact:*
- Explosion used for production order cost estimation

*Production Impact:*
- Material requirements used for production planning
- Component shortages delay production

*External ERP Considerations:*
- Material requirements may synchronize to ERP MRP
- ERP may have separate explosion logic

**Edge Cases & Error Scenarios**

*Deep BOM Structure:*
- BOM has 15 levels of sub-assemblies
- System performs explosion within performance target
- If too slow, system displays progress indicator

*Large Quantity Explosion:*
- User requests explosion for 1,000,000 units
- Quantities become very large
- System handles large numbers correctly

*Phantom/Transient Components:*
- Sub-assembly exists in BOM but not tracked in inventory
- System explodes through phantom to raw components

### REQ-BOM-006: Multi-Site BOM Management

**Priority**: High
**User Impact**: High

**Description**
Users must be able to define site-specific BOMs to support regional production differences while maintaining clear site boundaries and preventing cross-site BOM misuse.

**User Flow**
1. User creates or edits BOM
2. User assigns BOM to specific site
3. System validates that parent and component items exist in site
4. System saves site-specific BOM
5. Production in site can only use BOMs assigned to that site
6. User can optionally copy BOM from one site to another
7. Each site maintains independent BOM versions

**Business Rules**
- BOMs assigned to specific sites (not global by default)
- Parent item must be active in BOM's site
- Component items must be available in BOM's site
- Production orders can only use BOMs from same site
- BOM copy between sites creates independent BOM
- Site-specific BOMs can have different components and quantities
- Cross-site BOM visibility configurable (view only vs hidden)

**Data Requirements**

*Data to Capture:*
- Site assignment
- Site-specific component availability
- Cross-site BOM differences

*Data to Display:*
- Clear site indicator on BOM
- Site filter in BOM lists
- Warning when viewing BOM from different site

*Immutable Data:*
- Site assignment in historical BOM versions

*Versioned Data:*
- BOM versions are site-specific

**Integration & Dependencies**

*Inventory Impact:*
- Component availability checked per site
- Inter-site transfers not automatically triggered by BOM

*Costing Impact:*
- Cost rollup uses site-specific component costs
- Different sites may have different production costs

*Production Impact:*
- Production orders tied to site
- Material consumption from same site as BOM

*External ERP Considerations:*
- Sites may map to separate ERP entities
- BOM synchronization per site

**Edge Cases & Error Scenarios**

*Component Not Available in Site:*
- User adds component to BOM
- Component not configured for BOM's site
- System blocks: "Component not available in this site. Configure component for site first."

*Cross-Site Production Attempt:*
- User attempts to create production order in Site A using Site B's BOM
- System prevents: "BOM is assigned to different site. Cannot use in this production order."

### REQ-BOM-007: Where-Used Analysis

**Priority**: High
**User Impact**: Medium

**Description**
Users must be able to identify which BOMs use a specific component to understand impact of component changes, shortages, or discontinuation.

**User Flow**
1. User selects component item
2. User initiates "Where-Used" query
3. System searches all BOMs for component
4. System displays parent items that use component
5. System shows quantity required in each BOM
6. User optionally expands to multi-level where-used (grand-parents)
7. User filters by site, BOM status (active/inactive)

**Business Rules**
- Where-used search covers all BOM versions by default
- Where-used can be filtered to active versions only
- Multi-level where-used shows indirect usage (component → sub-assembly → finished good)
- Where-used results sorted by parent item or usage quantity
- Where-used search must complete within 3 seconds
- Results include BOM version information

**Data Requirements**

*Data to Capture:*
- Search component
- Search filters applied

*Data to Display:*
- Parent items using component
- Quantity required per parent
- BOM version information
- Active/inactive status
- Site assignments
- Multi-level relationships

*Immutable Data:*
- Not applicable (real-time query)

*Versioned Data:*
- Where-used references specific BOM versions

**Integration & Dependencies**

*Inventory Impact:*
- Where-used helps assess impact of component shortages

*Costing Impact:*
- Component cost changes impact all parent items shown

*Production Impact:*
- Component availability issues affect all parents

*External ERP Considerations:*
- Where-used may need to query ERP for complete picture

**Edge Cases & Error Scenarios**

*Component Used in Many BOMs:*
- Component used in 500 BOMs
- System paginates results
- Displays: "Showing 1-100 of 500 BOMs. Filter for specific results."

*No Usage Found:*
- Component not used in any BOM
- System displays: "Component not used in any BOM. Item may be purchased only."

### REQ-BOM-008: BOM Copy Function

**Priority**: Medium
**User Impact**: Medium

**Description**
Users must be able to copy existing BOMs to accelerate creation of similar product recipes.

**User Flow**
1. User selects existing BOM to copy
2. User initiates "Copy BOM"
3. System prompts for new parent item
4. User selects new parent item
5. System validates new parent is manufacturable
6. System copies all components and quantities
7. User reviews and edits copied BOM
8. User saves new BOM

**Business Rules**
- New parent item must be Manufactured or Both
- All components copied with same quantities
- Scrap factors and other attributes copied
- Copied BOM is independent (changes don't affect original)
- Copied BOM starts at version 1.0
- Copy can be cross-site if components available

**Data Requirements**

*Data to Capture:*
- Source BOM
- New parent item
- Copy timestamp and user

*Data to Display:*
- Copied components for review
- Differences if cross-site copy

*Immutable Data:*
- Not applicable

*Versioned Data:*
- New BOM is independent versioned entity

**Integration & Dependencies**

*Inventory Impact:*
- None (copy doesn't affect inventory)

*Costing Impact:*
- New BOM gets own cost rollup

*Production Impact:*
- New BOM available for production immediately

*External ERP Considerations:*
- New BOM may synchronize to ERP

**Edge Cases & Error Scenarios**

*Copy to Purchased Item:*
- User selects purchased-only item as new parent
- System blocks: "Target item must be manufacturable to have BOM."

*Cross-Site Copy with Missing Components:*
- User copies BOM from Site A to Site B
- Some components don't exist in Site B
- System warns: "3 components not available in target site. Remove or configure them."

### REQ-BOM-009: Component Scrap/Waste Factor Management

**Priority**: Medium
**User Impact**: Medium

**Description**
Users must be able to define scrap or waste percentages for components to account for normal production losses in material requirements and cost calculations.

**User Flow**
1. User adds component to BOM
2. User optionally enters scrap percentage (e.g., 5%)
3. System calculates adjusted quantity (base quantity × (1 + scrap%))
4. System displays both base and adjusted quantities
5. Adjusted quantity used for material requirements
6. Scrap factor included in cost rollup

**Business Rules**
- Scrap percentage is 0-100% (default 0%)
- Adjusted quantity = base quantity × (1 + scrap%)
- Scrap factors are component-specific within BOM
- Scrap factors versioned with BOM
- Material requirements use adjusted quantities
- Cost rollup includes scrap factor

**Data Requirements**

*Data to Capture:*
- Scrap percentage per component
- Base quantity
- Calculated adjusted quantity

*Data to Display:*
- Base quantity
- Scrap percentage
- Adjusted quantity
- Cost impact of scrap

*Immutable Data:*
- Historical scrap factors

*Versioned Data:*
- Scrap factors versioned with BOM

**Integration & Dependencies**

*Inventory Impact:*
- Material consumption uses adjusted quantities

*Costing Impact:*
- Scrap increases component cost contribution

*Production Impact:*
- Production instructions show adjusted quantities

*External ERP Considerations:*
- Scrap factors may map to ERP yield/waste fields

**Edge Cases & Error Scenarios**

*Extreme Scrap Percentage:*
- User enters 90% scrap
- System allows but warns: "Scrap percentage is unusually high. Verify entry."

*Scrap on Low-Quantity Components:*
- Base quantity is 0.5, scrap is 10%
- Adjusted quantity is 0.55
- System handles fractional quantities correctly

## 4. Non-Functional Requirements (Business-Level)

### Performance Expectations

**Response Time:**
- BOM list view load: < 2 seconds
- BOM detail view load: < 1 second
- BOM save operation: < 3 seconds
- BOM explosion: < 5 seconds for 10-level structures
- Cost rollup calculation: < 5 seconds for complex BOMs
- Where-used query: < 3 seconds

**Scalability:**
- Support 50,000+ BOMs
- Support BOMs with 1,000+ components
- Support 20+ levels of BOM hierarchy
- Support 100+ concurrent users

**Availability:**
- BOM access required during business hours: 99.5% uptime
- Critical for production planning and execution

### Data Integrity and Auditability

**Data Accuracy:**
- BOM structure integrity: 100% (no circular references)
- Cost calculation accuracy: 99.9%+
- Material requirement calculation accuracy: 100%

**Audit Compliance:**
- Complete BOM version history retained
- All changes tracked with user and timestamp
- Deletion attempts logged
- Cost rollup calculations logged
- Support regulatory traceability (FDA, ISO)

**Data Retention:**
- All BOM versions retained indefinitely
- Historical production references maintained
- Audit trail per regulatory requirements (7+ years)

### Security and Access Control

**Role-Based Access:**
- Manufacturing Engineer: Create, edit, version BOMs
- Production Planner: View BOMs, explode for requirements
- Cost Accountant: View BOMs, trigger cost rollup
- System Administrator: All BOM operations including deletion
- Read-Only User: View BOMs only

**Permission Granularity:**
- Create new BOMs
- Edit active BOMs
- Create BOM versions
- Delete BOMs (highly restricted)
- View cost information
- Trigger cost rollup
- Copy BOMs
- Export BOM data

### Compliance and Reporting

**Standard Reports:**
- BOM Master Report (all BOMs with components)
- BOM Change Report (version changes in date range)
- Where-Used Report (component usage across BOMs)
- BOM Cost Report (cost breakdowns)
- Missing Cost Report (components without costs)
- BOM Comparison Report (differences between versions)

**Export Capabilities:**
- Export BOM to CSV, Excel
- Export material requirements
- Export cost breakdown
- Export where-used results

## 5. Content & UX Copy Requirements

### Warning Messages

**Circular Reference Prevention:**
```
Circular Reference Detected

Item [ITEM-CODE] cannot be added as a component because it would create a circular reference. An item cannot be a component of itself, directly or indirectly.

Please select a different component.

[OK]
```

**BOM Deletion Blocked:**
```
Cannot Delete BOM

This BOM cannot be deleted because:
- Used in 45 active production orders
- Used in 230 historical production orders
- Current active version for parent item

Alternative: Mark BOM as Inactive to prevent use in new production while preserving history.

[View Dependencies] [Mark Inactive] [Cancel]
```

**Version Edit Prevention:**
```
Cannot Edit Historical BOM Version

Version 1.0 is historical and cannot be modified. Create a new version to make changes.

[Create New Version] [Cancel]
```

### Inline Guidance

**BOM Creation Form:**
```
Create Bill of Materials

Define the components required to manufacture this item. Add components one at a time or use bulk add for efficiency.

Tips:
- Ensure component quantities are accurate to avoid waste
- Use scrap factors for normal production losses
- BOMs can be versioned when changes are needed

[Help: BOM Best Practices]
```

**Cost Rollup Display:**
```
BOM Cost Breakdown

Total Material Cost: $125.50

This cost is calculated from component costs × quantities. Costs are current as of [DATE]. Components missing cost data are highlighted below.

[Recalculate Cost] [View Details]
```

## 6. Dependencies & Constraints

### Existing PMM Entities

**BOMs Depend On:**
- Items (parent and components must exist)
- Sites (BOMs assigned to sites)
- Cost Lists (for cost rollup calculations)
- Units of Measure (quantity calculations)

**BOMs Are Referenced By:**
- Production Orders (specify which BOM version to use)
- Cost Calculations (cost rollup)
- Material Requirements (explosion for planning)
- Inventory Transactions (track component consumption)

### Live Production Constraints

**Data Volume:**
- 50,000+ BOMs possible
- BOMs with 1,000+ components
- Multi-level structures 20+ levels deep

**Concurrent Usage:**
- Multiple engineers editing different BOMs
- Production orders referencing BOMs continuously
- Cost rollup calculations running frequently

**Integration Points:**
- Production systems consume BOM data
- Cost systems use BOM for calculations
- Planning systems explode BOMs for MRP

## 7. Rollout & Change Management Strategy

### Recommended Approach: Phased Rollout

**Phase 1: BOM Migration (Week 1-2)**
- Import existing BOMs from legacy systems
- Validate BOM accuracy
- Establish version baselines

**Phase 2: Engineering Pilot (Week 3-4)**
- Manufacturing engineers create and edit BOMs
- Test versioning workflows
- Validate cost rollup calculations

**Phase 3: Production Integration (Week 5-6)**
- Production orders begin using new BOMs
- Material requirements driven by BOMs
- Monitor accuracy and adjust

**Phase 4: Full Deployment (Week 7+)**
- All users access BOM system
- Legacy BOM systems retired
- Ongoing optimization

### User Training

**Manufacturing Engineers:** 4 hours comprehensive training
**Production Planners:** 2 hours on BOM explosion and requirements
**Cost Accountants:** 2 hours on cost rollup and analysis
**All Users:** 1 hour overview

## 8. Handoff Notes

This PRD defines business requirements for BOM Management. Key considerations for architecture and development:

**Critical Business Rules:**
- Prevent circular BOM references (both direct and indirect)
- Maintain BOM version history immutably
- Support multi-level cost rollup and explosion efficiently
- Protect BOMs with production dependencies from deletion

**Performance Targets:**
- BOM explosion < 5 seconds for 10+ levels
- Cost rollup < 5 seconds
- Where-used query < 3 seconds

**Integration Needs:**
- Production orders must reference specific BOM versions
- Cost calculations must use BOM rollup
- Material requirements must explode BOMs accurately

---

**Document Version**: 1.0
**Date**: 2026-02-05
**Author**: PMM Product Management Team
**Status**: Ready for Architecture Review
