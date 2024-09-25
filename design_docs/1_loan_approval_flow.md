
# Design Document: Loan Approval Flow in DAML

## 1. LoanRequest Template Design

- Path from Root of the Repo - `./daml/1_loan_approval_workflow/LoanRequest.daml`

### Design Explanation:

- **Contract Structure**:
   The `LoanRequest` template (see `LoanRequest.daml`) represents the borrower's request to the bank for a loan. It includes the following fields:
   - `borrower` (Party): The party requesting the loan.
   - `bank` (Party): The bank receiving the request.
   - `amount` (Decimal): The requested loan amount.

- **Signatories**:
   - **Borrower**: The borrower is the signatory since they initiate the loan request.
   - **Bank (Observer)**: The bank is an observer in this stage, allowing visibility of the loan request but not control over it until approval.

- **ApproveRequest Choice**:
   - The `ApproveRequest` choice allows the bank to approve the loan. The bank is the controller of this choice, meaning it has the authority to exercise this option.
   - **Action on Approval**: Once approved, the `LoanRequest` contract is archived, and a new `Loan` contract is created with the same details (borrower, bank, and amount). This design ensures that upon approval, the process moves forward to the creation of the formal loan agreement (`Loan` contract), automatically archiving the `LoanRequest` contract.

## 2. Loan Template Design

- Path from Root of the Repo - `./daml/1_loan_approval_workflow/Loan.daml`

### Design Explanation:

- **Contract Structure**:
   The `Loan` template (see `Loan.daml`) represents the formal loan agreement after the bank approves the request. It includes:
   - `borrower` (Party): The recipient of the loan.
   - `bank` (Party): The lender.
   - `amount` (Decimal): The loan amount agreed upon by both parties.

- **Signatories**:
   - Both the **borrower** and **bank** are signatories, ensuring mutual consent and legal enforcement of the contract. This ensures that both parties are equally responsible for the loan agreement and its terms.

- **Creation Trigger**:
   The `Loan` contract is created by the `ApproveRequest` choice exercised in the `LoanRequest` contract. This ensures a seamless transition from loan request approval to the establishment of a formal loan agreement between the two parties.



## Conclusion

This design satisfies the loan approval flow by structuring two templates (`LoanRequest` and `Loan`) that handle the process from loan request initiation to formal loan creation. By separating the request phase and the loan agreement, the system ensures a clear flow and preserves roles for both the borrower and the bank throughout the process.
