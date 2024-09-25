
# Loan Approval and Repayment Workflow in DAML

## 1. Prerequisites: Installing DAML and Adding to Path

To begin working with DAML, you need to install the DAML SDK and add it to your system’s path:

1. Download the DAML SDK from the official [DAML website](https://docs.daml.com/getting-started/installation.html).
2. After installation, ensure that the `daml` command is added to your system's path.
    - For **Linux/macOS** users:
      ```bash
      export PATH=$PATH:$HOME/.daml/bin
      ```
    - For **Windows** users:
      Follow the [installation guide](https://docs.daml.com/getting-started/installation.html#adding-daml-to-your-path) for adding the `daml` command to your PATH.

3. Verify the installation by running:
   ```bash
   daml version
   ```

## 2. Running the Tests

To ensure the correctness of the workflow, we have provided test cases for each part of the loan approval, disbursement, and repayment system. To run all the test cases:

1. Navigate to the root of the repository.
2. Execute the following command:
   ```bash
   daml test
   ```
    This will run all the DAML test cases defined for the system.

3. To run a specific test case, you can use the following command:
   ```bash
   daml test --files <path-to-test-file>
   ```
   Replace `<path-to-test-file>` with the path to the test file you want to run.

## 3. Links to Specific Test Files

Here are the links to individual test files for each part of the workflow:

- **Loan Approval Workflow**:
  - [Loan Request can be created by a Borrower](./daml/1_loan_approval_workflow/Test/LoanApprovalTest.daml)
  - [Bank can approve the Loan Request](./daml/1_loan_approval_workflow/Test/LoanApprovalTest.daml)
  - [Loan Request is archived after approval](./daml/1_loan_approval_workflow/Test/LoanApprovalTest.daml)
  - [A new Loan contract is created after approval](./daml/1_loan_approval_workflow/Test/LoanApprovalTest.daml)
  
- **Loan Disbursement and Token Transfer Workflow**:
  - [Creation  of Loan Limit](./daml/2_token_disbursement_and_loan_limit/Test/LoanLimitTest.daml)
  - [Creation and Approval of Loan Request](./daml/2_token_disbursement_and_loan_limit/Test/LoanRequestTest.daml)
  - [Token Disbursement](./daml/2_token_disbursement_and_loan_limit/Test/LoanDisbursementTest.daml)
  - [Loan Limit Used Amount Update](./daml/2_token_disbursement_and_loan_limit/Test/LoanLimitUpdateTest.daml)
  - [Loan request can only be approved if the loan limit is not exceeded](./daml/2_token_disbursement_and_loan_limit/Test/LoanLimitExceededTest.daml)
  
- **Loan Repayment Workflow**:
  - [Creation of the Repayment Restriction Contract](./daml/3_loan_repayment_workflow/Test/RepaymentRestrictionTest.daml)
  - [Creation and Execution of the Repay Choice on the Loan Contract](./daml/3_loan_repayment_workflow/Test/LoanRepaymentTest.daml)
  - [Updating of the total repaid amount on the Loan Contract](./daml/3_loan_repayment_workflow/Test/LoanRepaymentTest.daml)
  - [Archiving of the loan contract once it is repaid](./daml/3_loan_repayment_workflow/Test/LoanRepaymentTest.daml)
  - [Updating of the utlilised loan amount on the LoanLimit Contract](./daml/3_loan_repayment_workflow/Test/LoanRepaymentTest.daml)

## 4. Design Documents for Each Workflow

Detailed design documents are available for each part of the workflow. You can access them from the `design_docs` folder:

- **Loan Approval Workflow**: [Loan Approval Design Document](./design_docs/1_loan_approval_flow.md)
- **Loan Disbursement Workflow**: [Loan Disbursement Design Document](./design_docs/2_token_disbursement_and_loan_limit.md)
- **Loan Repayment Workflow**: [Loan Repayment Design Document](./design_docs/3_loan_repayment_workflow.md)

## 5. Next Steps

- Interest Calcuation - Add interest calculations to the loan amount and adjust repayment amounts accordingly
- Repayment Schedule - Add a repayment schedule to the loan contract