
# Design Document: Expanded Loan System with Token Disbursement and Loan Limit Validation

## 1. TokenAccount Template Design

- Path from Root of the Repo - `./daml/2_token_disbursement_and_loan_limit/TokenAccount.daml`

### Design Explanation:

- **Contract Structure**:
   The `TokenAccount` template represents the token balance of a party in the system. It includes:
   - `owner` (Party): The account holder.
   - `issuer` (Party): The issuer of the tokens.
   - `balance` (Decimal): The current balance of tokens in the account.

- **Signatories**:
   - Both the **owner** and **issuer** are signatories to ensure that both parties agree to token balance updates.

- **Choices**:
   - **Credit**: This choice allows the issuer to add tokens to the account. The action is controlled by the issuer, and an assertion ensures that only positive amounts can be credited.
   - **Debit**: This choice allows the owner to withdraw tokens. The action is controlled by the owner, and assertions ensure that the balance is sufficient and that the amount being debited is positive.

## 2. LoanLimit Template Design

- Path from Root of the Repo - `./daml/2_token_disbursement_and_loan_limit/LoanLimit.daml`

### Design Explanation:

- **Contract Structure**:
   The `LoanLimit` template stores the total loan limit available to the bank and the amount already used. It includes:
   - `bank` (Party): The bank holding the loan limit.
   - `totalLimit` (Decimal): The maximum loan amount that the bank can approve.
   - `usedAmount` (Decimal): The current amount of loans that have been approved by the bank.

- **Signatories**:
   - The **bank** is the sole signatory, as it controls and manages the loan limits.

- **Choices**:
   - **UpdateUsedAmount**: This choice allows the bank to update the `usedAmount` after a loan approval. The bank controls this action, and an assertion ensures that the new `usedAmount` does not exceed the `totalLimit`.

## 3. LoanRequestWithLoanLimit Template Design

- Path from Root of the Repo - `./daml/2_token_disbursement_and_loan_limit/LoanRequestWithLoanLimit.daml`

### Design Explanation:

- **Contract Structure**:
   The `LoanRequestWithLoanLimit` template represents the borrower's request for a loan, validated against the bank's loan limit. It includes:
   - `borrower` (Party): The borrower requesting the loan.
   - `bank` (Party): The bank that processes the loan request.
   - `amount` (Decimal): The requested loan amount.

- **Signatories**:
   - The **borrower** is the signatory since they are the one initiating the loan request.
   - The **bank** is an observer, having the authority to approve or reject the request.

- **Choices**:
   - **ApproveRequest**: This choice allows the bank to approve the loan request, but only if the `amount` does not exceed the remaining loan limit. The bank must provide a reference to the `LoanLimit` contract, which is fetched and updated. Upon approval, a `LoanWithDisbursement` contract is created, and the `LoanRequestWithLoanLimit` contract is archived.

## 4. LoanWithDisbursement Template Design

- Path from Root of the Repo - `./daml/2_token_disbursement_and_loan_limit/LoanWithDisbursement.daml`

### Design Explanation:

- **Contract Structure**:
   The `LoanWithDisbursement` template represents the formal loan agreement between the borrower and the bank. It includes:
   - `borrower` (Party): The borrower receiving the loan.
   - `bank` (Party): The bank providing the loan.
   - `amount` (Decimal): The total loan amount approved by the bank.
   - `disbursedAmount` (Decimal): The amount of the loan that has been disbursed to the borrower so far.

- **Signatories**:
   - Both the **borrower** and the **bank** are signatories, ensuring mutual agreement on the loan terms.

- **Choices**:
   - **Disburse**: This choice allows either the bank or the borrower to disburse tokens to the borrower’s account after the loan approval. The disbursement is capped by the approved loan amount, ensuring that no more than the agreed amount can be disbursed. The disbursement also updates the borrower’s `TokenAccount` by crediting tokens to their balance.


## Conclusion

This design extends the original loan approval system by introducing a loan limit and token disbursement mechanism. It ensures that loan requests are validated against the bank's predefined loan limits and that tokens are disbursed within the approved loan amount. By separating concerns into multiple templates (loan request, loan, token account, and loan limit), the design provides a clear and secure workflow.
