
# Design Document: Loan Repayment Workflow in DAML

## 1. RepaymentRestriction Template Design

- Path from Root of the Repo - `./daml/3_loan_repayment_workflow/RepaymentRestriction.daml`

### Design Explanation:

- **Contract Structure**:
   The `RepaymentRestriction` template defines the restrictions around loan repayments, specifically the minimum repayment amount. It includes:
   - `bank` (Party): The bank that sets the repayment restriction.
   - `borrower` (Party): The borrower associated with the loan.
   - `minRepaymentAmount` (Decimal): The minimum amount that must be repaid for each repayment.

- **Signatories**:
   - The **bank** is the signatory because it controls the repayment terms, while the **borrower** is an observer who must adhere to these terms.

- **Minimum Repayment Amount**:
   - The current minimum repayment is set to 10% of the loan amount by default. The bank has the option to modify this amount as needed, but it cannot be changed by the borrower, ensuring that repayment restrictions are solely managed by the bank.

## 2. LoanWithRepayment Template Design

- Path from Root of the Repo - `./daml/3_loan_repayment_workflow/LoanWithRepayment.daml`

### Design Explanation:

- **Contract Structure**:
   The `LoanWithRepayment` template represents the formal loan agreement with repayment functionality. It includes:
   - `borrower` (Party): The borrower repaying the loan.
   - `bank` (Party): The lender.
   - `amount` (Decimal): The total loan amount approved by the bank.
   - `disbursedAmount` (Decimal): The amount of the loan that has been disbursed to the borrower.
   - `repaidAmount` (Decimal): The amount that has been repaid so far.
   - `repaymentRestrictionCid` (ContractId RepaymentRestriction): A reference to the repayment restriction contract.

- **Signatories**:
   - Both the **borrower** and the **bank** are signatories, ensuring that both parties agree to the terms of the loan and repayment.

- **Choices**:
   - **Disburse**: This choice allows the bank or borrower to disburse tokens to the borrower's account, similar to the previous workflows.
   - **Repay**: This choice allows the borrower to make incremental repayments. It ensures that:
     - The repayment amount is positive.
     - The repayment amount meets the minimum requirement set in the `RepaymentRestriction` contract.
     - The total repaid amount is updated each time the `Repay` choice is exercised.
     - If the total repaid amount equals the disbursed loan amount, the `LoanWithRepayment` contract can be archived, and the bank can update the utilized loan amount in the `LoanLimit` contract.

## 3. LoanLimitWithRepayment Template Design

- Path from Root of the Repo - `./daml/3_loan_repayment_workflow/LoanLimitWithRepayment.daml`

### Design Explanation:

- **Contract Structure**:
   The `LoanLimitWithRepayment` template manages the loan limit for the bank and keeps track of the amount of loans disbursed. It includes:
   - `bank` (Party): The bank managing the loan limit.
   - `totalLimit` (Decimal): The maximum loan amount that can be approved.
   - `usedAmount` (Decimal): The amount of loans that have been approved and disbursed.

- **Signatories**:
   - The **bank** is the signatory, responsible for managing loan approvals and limits.

- **Choices**:
   - **UpdateUsedAmount**: This choice allows the bank to update the used loan amount after a loan approval or repayment.


## Design Decisions 

This design adds loan repayment functionality to the existing loan approval and disbursement workflow. It ensures that borrowers can make incremental repayments while adhering to the minimum repayment amount set by the bank. The loan repayment workflow is tightly integrated with the loan limit system, ensuring that the loan contract can be archived once the loan is fully repaid. The repayment restriction gives control to the bank, ensuring that repayment terms are strictly enforced.

- The **minimum repayment amount** is set to **10% of the loan amount**.
- The **repayment restriction** is enforced by setting the repayment restriction contract ID in the loan contract. 
- The **repayment restriction contract ID** in the loan contract can **only be changed by the bank**, ensuring that the bank controls the minimum repayment amount while the borrower cannot alter it.
- Once the loan is **closed**, the **used amount** from the loan limit is **released**, allowing it to be reused for future loan requests.
