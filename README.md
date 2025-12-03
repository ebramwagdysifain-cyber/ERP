Oracle Cloud ERP: Detailed Implementation Workshop Scripts
Scope: Comprehensive ERP Modules (Financials, Projects, Procurement, Risk)
Structure: Aligned strictly to Oracle Modern Best Practice flows
Question Style: Modeled after Acquire to Dispose Workshops (Detailed Discovery)
Context: Egypt Real Estate & Construction Examples
WORKSHOP STANDARD: PROCESS DIAGRAM REVIEW
Instruction: At the start of EVERY workshop below, you must project the "Scenario Board" (Process Flow) and validate the following points before asking detailed questions.
Agenda Item 0: Scenario Board & Happy Path Validation
Visual Walkthrough: Trace the line from Start (e.g., "Site Engineer requests Cement") to Finish (e.g., "Supplier Paid").
Identify Variations:
Happy Path: The standard 80% of transactions.
Exception Path: What happens if the budget is exceeded? What happens if the material is rejected?
System of Record: Confirm which steps happen outside Oracle (e.g., "Engineering Design in AutoCAD").
PART 1: FINANCIAL MANAGEMENT
Process 1.1: Period Close to Financial Reports
Target Audience: Controller, GL Manager, Reporting Team
Step 0: Process Diagram Review
Scenario: Validate the flow from "Sub-ledger Close" -> "GL Create Accounting" -> "Revaluation" -> "Consolidation" -> "Reporting".
graph LR
    A[Close Sub-Ledgers<br/>AP/AR/FA/Cash] -->|Create Accounting| B[General Ledger]
    B --> C{Period Tasks}
    C -->|Revaluation| D[Translation & Reval]
    C -->|Allocations| E[Cost Allocations]
    D --> F[Consolidation]
    F --> G[Financial Reporting]


Step 1: Close Subledgers
Objective: Monitor enterprise-wide close status and finalize sub-ledger transactions.
Q: Volume of Exceptions: How many "unaccounted transactions" (e.g., un-invoiced receipts, un-applied cash) do you typically have at month-end?
Egypt Real Estate Example: Do you struggle to close the AR module because of thousands of uncleared PDCs (Post-Dated Checks) that need manual reconciliation with the bank statements before closing?
Q: Dependency Management: Do you strictly enforce that AP and AR must close before the Inventory period closes?
Why this matters: In construction, if you close Inventory before AP, you might miss booking a "cost variance" from a supplier invoice that affects your project margins.
Step 2: Reconcile Accounts
Objective: Reconcile sub-ledgers to GL automatically.
Q: Matching Rules: What is your current manual effort for reconciling "Inter-company" balances?
Egypt Real Estate Example: When the "Construction Arm" bills the "Development Arm" for building the compound, do these invoices automatically knock each other out, or do you spend days fixing penny differences due to VAT rounding?
Q: Suspense Accounts: Do you use suspense accounts for import clearing (Letter of Credit payments)? How often are these reviewed?
Step 3: Close Ledgers & Consolidate
Objective: Route close tasks and transform subsidiary results.
Q: Secondary Ledgers: Do you need to close your "EAS" (Egyptian Accounting Standards) books on a different timeline than your "IFRS" books for international investors?
Q: Currency Translation: How do you handle the "Translation Adjustment" (CTA) for your foreign procurement offices (e.g., Dubai procurement office buying in AED)?
Step 4: Publish Financial Statements
Objective: Distribute secure statements.
Q: Segment Reporting: Do you require a full P&L and Balance Sheet for each Compound/Project (e.g., New Capital vs. North Coast)?
Configuration Impact: This requires balancing segments in the Chart of Accounts to ensure every journal entry (even cash) is tagged to a specific project.
Process 1.2: Bank Transactions to Cash Position
Target Audience: Treasury Manager
Step 0: Process Diagram Review
Scenario: Validate flow from "Bank Statement Import" -> "Auto-Reconcile" -> "Cash Position Dashboard".
graph LR
    A[Bank Statement File<br/>MT940/CAMT] -->|Import| B[Cash Management]
    B -->|Auto-Reconcile Rules| C{Match Found?}
    C -->|Yes| D[Reconciled]
    C -->|No| E[Manual Exception]
    D --> F[Cash Position Dashboard]
    E --> F


Step 1: Bank Validation & Integration
Objective: Daily interface with banks.
Q: Statement Formats: Which local banks (CIB, NBE, QNB) are you integrating with? Do they support MT940 or XML CAMT.053?
Q: Intraday Needs: Do you need "Intraday" statements (MT942) to make decisions on transferring funds to cover construction contractor checks?
Step 2: Cash Positioning
Objective: Real-time liquidity views.
Q: Pooling: Do you use physical cash pooling or just notional pooling for your SPVs (Special Purpose Vehicles)?
Egypt Real Estate Example: You have 10 SPVs. If SPV A is rich in cash from sales, and SPV B needs cash for construction, do you need the system to suggest the inter-company transfer automatically?
Process 1.3: Asset Acquisition to Retirement
Target Audience: Fixed Asset Manager
Step 0: Process Diagram Review
Scenario: Validate flow from "PO Receipt/Project Capitalization" -> "Asset Workbench" -> "Depreciation" -> "Retirement".
graph TD
    A[Source: AP Invoice / Project CIP] -->|Mass Additions| B[Asset Workbench]
    B -->|Categorize| C[Asset Book]
    C --> D[Monthly Depreciation]
    D --> E{Event?}
    E -->|Maintenance| F[Adjust Cost]
    E -->|Disposal| G[Retire/Sell]
    G --> H[Calc Gain/Loss]


Step 1: Recognize & Register Assets
Objective: Asset recognition upon purchase.
Q: Capitalization Thresholds: What is your policy for "Low Value Assets" (e.g., Site Laptops)? Do you expense them immediately or depreciate them over 1 year?
Q: Componentization: For a large asset like a "Tower Crane" or "Hotel Building," do you track the "Elevators" separately from the "Structure"?
Why this matters: The elevator has a useful life of 10 years, the structure 50 years. IFRS requires tracking them as separate sub-assets.
Step 2: Manage Asset Insights (IoT)
Objective: Monitoring asset health.
Q: Maintenance Integration: If a site generator breaks down and you issue a PO for repairs, should that cost increase the asset's value (Betterment) or just be an expense?
Process 1.4: Invoice to Receipt (Receivables)
Target Audience: Collections Manager, Sales Admin
Step 0: Process Diagram Review
Scenario: Validate flow from "Sales/Installment Generation" -> "Invoice Creation" -> "Receipt (Cash/PDC)" -> "Clearance".
graph LR
    A[Installment Schedule] -->|Due Date| B[Generate Invoice]
    B --> C[Send to Customer]
    C --> D[Receive Payment]
    D --> E{Type?}
    E -->|Cash/Wire| F[Apply to Invoice]
    E -->|PDC Check| G[Log to Custody]
    G -->|Maturity Date| H[Deposit to Bank]
    H --> F


Step 1: Create Transactions
Objective: Generate invoices from sales.
Q: Billing Triggers: Are your invoices triggered by "Milestones" (e.g., Foundation Complete) or strictly by "Dates" (e.g., Quarterly Installments)?
Egypt Real Estate Example: For off-plan sales, you generate "Installment Statements" rather than tax invoices. The Tax Invoice is only generated when cash is received or the unit is delivered. How do you handle this distinction?
Step 2: Process Payments (PDCs)
Objective: Match receipts to invoices.
Q: PDC Lifecycle: Please describe the exact lifecycle of a check. Receipt -> Safe Custody -> Bank Deposit -> Clearance -> NSF (Bounced).
Q: Bulk Processing: Do you deposit checks in batches (e.g., 500 checks at once)? Do you need a "Deposit Slip" report for the bank runner?
Step 3: Manage Billing Issues (Disputes)
Objective: Manage disputes and adjustments.
Q: Penalty Waivers: If a customer is late, the system calculates a penalty. Who has the authority to "Waive" this penalty? (Sales Director vs. CFO).
Process 1.5: Contract to Revenue (ASC 606 / IFRS 15)
Target Audience: Revenue Accountant
Step 0: Process Diagram Review
Scenario: Validate flow from "Sales Contract" -> "Identify POBs" -> "Allocate Price" -> "Recognize Revenue over time".
graph TD
    A[Sales Contract] --> B[Identify Performance Obligations<br/>(Unit + Maintenance + Club)]
    B --> C[Determine Transaction Price]
    C -->|Allocate SSP| D[Revenue Contract]
    D --> E{Trigger?}
    E -->|Percentage of Completion| F[Recognize Unit Rev]
    E -->|Over Time| G[Recognize Maint Rev]


Step 1: Identify Accounting Contracts
Objective: Group documents into contracts.
Q: Contract Grouping: If a customer buys a Unit today, and a Parking Spot next month, should these be treated as one revenue contract or two?
Step 2: Determine Transaction Price & POBs
Objective: Identify Performance Obligations.
Q: Variable Consideration: Do you offer "Early Payment Discounts" or "Referral Fees" that reduce the transaction price?
Q: Bundled Services: Does the sales price include "Club Membership" or "Maintenance Deposit"?
Egypt Real Estate Example: The "Maintenance Deposit" (Wade3a) is a liability, not revenue. It must be separated and tracked to spend on compound upkeep later.
Process 1.6: Customer Deductions to Settlement
Target Audience: Collections Manager, AR Accountant
Step 0: Process Diagram Review
Scenario: Validate flow from "Short Payment Receipt" -> "Create Claim" -> "Research" -> "Settle (Credit Memo/Write-off)".
graph LR
    A[Receipt Created] -->|Short Payment| B[Deduction/Claim]
    B --> C[Assign to Researcher]
    C --> D{Valid?}
    D -->|Yes| E[Issue Credit Memo]
    D -->|No| F[Chargeback Customer]


Step 1: Receive Short Payment & Match
Objective: Identify and classify payments that are less than the invoice amount.
Q: Short Payment Thresholds: Do you have a tolerance rule? (e.g., If a payment is within 50 EGP of the invoice, automatically write off the difference).
Egypt Real Estate Example: Customers paying via international wire transfer (from the Gulf/Europe) often have bank charges deducted, resulting in a shortfall of $20-$50. Do you research these or auto-expense them to "Bank Fees"?
Step 2: Assign & Research Claim
Objective: Investigate the validity of the deduction.
Q: Deduction Reasons: What are the top reasons customers short-pay? (e.g., "Late Delivery Penalty", "Disputed Finishing Quality", "Unrecognized Admin Fee").
Egypt Real Estate Example: A customer refuses to pay the "Club House Fees" included in the installment because the Club House isn't operational yet. They pay the Unit Installment but deduct the Club fee. How do you split this receipt and track the dispute?
Q: Netting (Contra-Charging): Do you have customers who are also suppliers?
Example: A contractor builds your compound and also buys a unit in it. They might deduct their "Construction Invoice" from their "Unit Installment." We need to configure "Netting Agreements" to handle this.
Step 3: Settle Claim
Objective: Resolve the dispute via Credit Memo or Chargeback.
Q: Resolution Authority: If a customer claims "The sales agent promised me a 5% discount" which isn't in the contract, who has the authority to approve this deduction (Credit Memo) vs. rejecting it (Chargeback)?
Process 1.7: Lease Management (Lessee & Lessor)
Target Audience: Leasing Manager, Property Manager
Step 0: Process Diagram Review
Scenario: Validate flow from "Lease Abstract" -> "Payment Schedule" -> "Interest/Amortization" -> "Termination".
graph LR
    A[Lease Contract] --> B[Create Lease Abstract]
    B --> C[Generate Schedules]
    C --> D{Monthly Process}
    D -->|AP/AR| E[Generate Invoice]
    D -->|GL| F[Amortization/Accrual]
    E --> G[Payment/Receipt]


Step 1: Capture Lease Data (Lessor - Retail)
Objective: Managing Commercial/Mall Leasing.
Q: Variable Rent: Do you have retail tenants (e.g., Starbucks in your mall) who pay "% of Sales" instead of fixed rent?
Q: CAM Charges: How do you bill "Common Area Maintenance" (CAM)? Is it a fixed monthly fee or reconciled at year-end based on actual electricity/cleaning costs?
Step 2: Manage Accounting (Lessee - Land)
Objective: IFRS 16 Compliance for Land Leases.
Q: Land Installments: If you bought land from the government (NUCA) on a 7-year installment plan, do you track this as a Lease Liability (Right of Use Asset) or simple Debt?
Q: Interest Rate: What "Incremental Borrowing Rate" do you use to calculate the Present Value of these lease payments?
Process 1.8: Joint Venture Accounting
Target Audience: Chief Accountant, Partner Relations
Step 0: Process Diagram Review
Scenario: Validate flow from "JV Agreement" -> "Identify Distributable Trans" -> "Generate Partner Invoice".
graph TD
    A[Operational Transactions] --> B{JV Rule Check}
    B -->|Yes| C[Distributable Cost]
    C --> D[Apply Partner Split %]
    D --> E[Create Partner Invoice AR/AP]


Step 1: Define JV Terms
Objective: Partnering with Land Owners.
Q: Revenue Sharing: Do you have a "Revenue Share" agreement with the Land Owner (e.g., Ministry of Housing gets 40% of sales)?
Q: Cash Calls: How do you request funding from partners if the project runs out of cash?
PART 2: PROJECT MANAGEMENT
Process 2.1: Project Plan to Delivery
Target Audience: Project Director, Planning Engineer
Step 0: Process Diagram Review
Scenario: Validate flow from "Project Initiation" -> "WBS Creation" -> "Budgeting" -> "Execution" -> "Monitoring".
graph LR
    A[Initiate Project] --> B[Define WBS Structure]
    B --> C[Assign Budget]
    C --> D[Baseline Plan]
    D --> E[Execution & Costs]
    E --> F[Monitor Forecast vs Actual]


Step 1: Initiate Project & WBS
Objective: Establish timeline and budget.
Q: WBS Standardization: Can we standardize the WBS (Work Breakdown Structure) across all compounds? (e.g., Phase -> Building -> Floor -> Item).
Q: Budget Granularity: Do you budget at the "Cost Code" level (e.g., Concrete Grade 40) or just "Skeleton Works"?
Step 2: Monitor Progress
Objective: Analyze project health.
Q: Physical Percent Complete: How do you measure progress? (Engineer estimate vs. BOQ Quantity Surveying).
Integration Point: Do you need to upload "Physical %" from Primavera/MS Project to drive the "Earned Value" analysis in Oracle?
Process 2.2: Project Costs to Accounting
Target Audience: Project Controller
Step 0: Process Diagram Review
Scenario: Validate flow from "Cost Incurred (PO/Time)" -> "Burdening" -> "Capitalization/Expense" -> "GL Posting".
graph TD
    A[Costs: PO / Time / Expense] --> B[Import to Project]
    B --> C[Validate Control Budget]
    C --> D[Apply Overhead/Burden]
    D --> E[Generate Accounting]
    E --> F[Post to GL]


Step 1: Capture Project Costs
Objective: Standardize cost collection.
Q: Inventory Issues: When rebar is moved from the "Central Site Store" to "Tower A," is that an immediate project cost?
Q: Subcontractor Payment Certs: Do you use "Payment Certificates" (Mustakhlas) to pay contractors?
Egypt Construction Example: A payment certificate usually includes: Work Done - Retention - Advance Recovery - Taxes = Net Payment. We need to configure a custom form or "Pay Item" structure to handle this calculation.
Step 2: Capital Project to Asset
Objective: CIP (Construction in Progress) capitalization.
Q: Interest Capitalization: Do you capitalize bank loan interest into the project cost?
Q: Asset Split: When a compound is finished, how do you split the single "Project Cost" into 500 distinct "Unit Assets" for inventory/sales?
Process 2.3: Project Contract Billing
Target Audience: Billing Manager (Contracting Arm)
Step 0: Process Diagram Review
Scenario: Validate flow from "Contract Setup" -> "Event/Progress Entry" -> "Generate Invoice" -> "Revenue Rec".
Step 1: Manage Project Contracts
Objective: Billing external or internal clients.
Q: Billing Method: Is your billing "Rate Based" (Hours * Rate) or "Fixed Price" (Milestone %)?
Q: Inter-Company Billing: Does your "Construction Company" bill your "Real Estate Company"? If so, do you want to automate this invoice generation to avoid manual data entry?
Process 2.4: Resource Analysis to Utilization
Target Audience: Resource Manager, HR
Step 0: Process Diagram Review
Scenario: Validate flow from "Resource Request" -> "Search & Match" -> "Assignment" -> "Utilization Analysis".
Step 1: Analyze Resource Demand
Objective: Staffing optimization.
Q: Site Assignment: Do you track which engineers are assigned to "New Capital Site" vs. "Head Office"? Does this affect their payroll (e.g., Site Allowance)?
Q: Equipment as Resource: Do you want to schedule "Tower Cranes" or "Bulldozers" as resources in the system to track their utilization?
PART 3: PROCUREMENT
Process 3.1: Insight to Sourcing
Target Audience: Procurement Director
Step 0: Process Diagram Review
Scenario: Validate flow from "Identify Opportunity" -> "Create RFQ" -> "Supplier Bids" -> "Analyze & Award".
graph LR
    A[Identify Opportunity] --> B[Create RFQ/RFP]
    B --> C[Invite Suppliers]
    C --> D[Suppliers Submit Bids]
    D --> E[Score & Analyze]
    E --> F[Award Business]


Step 1: Create Sourcing Event
Objective: RFQ/RFP creation.
Q: Sealed Bidding: For major construction packages (e.g., > 10M EGP), do you require "Blind Scoring" where the technical team cannot see the commercial price?
Q: Two-Stage RFQ: Do you have a "Technical Envelope" and "Financial Envelope" process?
Step 2: Evaluate and Award
Objective: Decision making.
Q: Best Available Price: Do you award based on "Lowest Price" or "Best Value" (Price + Technical Score + Delivery Time)?
Process 3.2: Requisition to Payment
Target Audience: Supply Chain Manager
Step 0: Process Diagram Review
Scenario: Validate flow from "Requisition (PR)" -> "Approval" -> "PO Creation" -> "Receipt (GRN)" -> "Invoice".
Step 1: Create Requisitions
Objective: Self-service ordering.
Q: Smart Forms: Do site engineers need simple forms for "Concrete Ordering" where they just select the Grade and Quantity, and the system picks the correct supplier contract?
Q: Budget Check: Do you require a "Hard Stop" if a Site Engineer tries to order material that is not in the project budget?
Process 3.3: Supplier Invoice to Payment
Target Audience: AP Manager
Step 0: Process Diagram Review
Scenario: Validate flow from "Invoice Scan/Entry" -> "3-Way Match" -> "Validation" -> "Payment".
graph LR
    A[Invoice Scan/OCR] --> B[Validate & Match PO]
    B --> C{Match?}
    C -->|Exception| D[Hold for Review]
    C -->|Success| E[Approve for Payment]
    E --> F[Payment Run (Check/EFT)]


Step 1: Manage Supplier Invoices
Objective: Automate invoice processing.
Q: Tax Handling: How do you handle the 1% (Goods) / 3% (Services) Withholding Tax?
Egypt Tax Rule: You must generate a "Form 41" report quarterly for the Tax Authority listing all these deductions.
Q: Retention Release: How do you track the "Retention Money" held from contractors? Do you need an alert 12 months after handover to release the final 5%?
Step 2: Process Payments
Objective: Settle liabilities.
Q: Payment Methods: Do you pay suppliers via "Checks" (requiring signature cycles) or "Bank Transfers" (ACH)?
Q: Letter of Credit (LC): Do you use LCs for importing elevators/finishing materials? We need to track the LC issuance, amendment, and final settlement.
Process 3.4: Contract Creation to Compliance
Target Audience: Contracts Manager, Legal
Step 0: Process Diagram Review
Scenario: Validate flow from "Terms Library" -> "Authoring" -> "Signature" -> "PO Execution".
Step 1: Author Contracts
Objective: Legal agreements.
Q: Terms Library: Do you have standard FIDIC clauses that you reuse for all construction contracts?
Q: Value Leakage: Do you want to prevent POs from being created if there is no valid contract in place?
PART 4: RISK & COMPLIANCE
Process 4.1: Secure Role Design
Target Audience: IT Security, Internal Audit
Step 0: Process Diagram Review
Scenario: Validate flow from "Role Config" -> "SoD Analysis" -> "User Assignment".
graph TD
    A[Draft Role Config] --> B[Simulate Security Rules]
    B --> C{SoD Violation?}
    C -->|Yes| D[Remediate/Redesign]
    C -->|No| E[Deploy Role]
    E --> F[Assign to User]


Step 1: Configure Roles (SoD)
Objective: Eliminate inherent risks.
Q: Critical Conflicts: What are your "Toxic Combinations"?
Example: Can a "Site Engineer" both Request material (PR) and Receive it (GRN)? In remote sites, this might be necessary, but it's a fraud risk. How do we mitigate this?
Process 4.2: Internal Controls Automation
Target Audience: Internal Control Manager
Step 0: Process Diagram Review
Scenario: Validate flow from "Control Logic" -> "Monitoring" -> "Incident Alert" -> "Remediation".
Step 1: Automate Monitoring
Objective: Monitor transaction fraud.
Q: Audit Trail: Do you need a "Change Log" for sensitive data? (e.g., Who changed the bank account number on a Vendor Master record?).
Q: Ghost Employees: Do you need controls to detect payroll payments to employees who have no "Site Attendance" records?
