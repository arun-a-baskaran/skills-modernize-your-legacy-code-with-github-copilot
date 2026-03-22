# Account Management System - Test Plan

## Overview
This test plan documents all test cases for the COBOL-based Account Management System. The system provides functionality to view account balance, credit (deposit) funds, and debit (withdraw) funds from an account with an initial balance of 1000.00.

---

## Test Cases

| Test Case ID | Test Case Description | Pre-conditions | Test Steps | Expected Result | Actual Result | Status | Comments |
|---|---|---|---|---|---|---|---|
| TC-001 | Display main menu | System started | 1. Run the application | Menu is displayed with options: 1. View Balance, 2. Credit Account, 3. Debit Account, 4. Exit | | | |
| TC-002 | View balance - Initial state | Application running, no transactions performed | 1. At main menu, select option 1 (View Balance) | Display "Current balance: 001000.00" | | | Initial balance should be 1000.00 |
| TC-003 | Credit account - Valid credit amount | Account balance is 1000.00 | 1. Select option 2 (Credit Account) 2. Enter credit amount: 500 | Display "Amount credited. New balance: 001500.00" and balance is updated | | | Balance should increase by credit amount |
| TC-004 | Credit account - Multiple credits | Account balance is 1000.00 | 1. Select option 2, enter 100 2. Select option 2, enter 200 3. Select option 1 to verify | Final balance should be 1300.00 | | | Multiple credits should accumulate correctly |
| TC-005 | Credit account - Large credit amount | Account balance is 1000.00 | 1. Select option 2 2. Enter credit amount: 999999.99 | Amount is credited, new balance calculated correctly | | | System should handle large numeric values |
| TC-006 | Credit account - Zero credit amount | Account balance is 1000.00 | 1. Select option 2 2. Enter credit amount: 0 | Amount credited (0), balance unchanged (1000.00) | | | System accepts zero as valid input |
| TC-007 | Debit account - Valid debit amount | Account balance is 1000.00 | 1. Select option 3 (Debit Account) 2. Enter debit amount: 300 | Display "Amount debited. New balance: 000700.00" and balance is updated | | | Balance should decrease by debit amount |
| TC-008 | Debit account - Exact balance debit | Account balance is 1000.00 | 1. Select option 3 2. Enter debit amount: 1000 | Display "Amount debited. New balance: 000000.00" | | | Debit entire balance should result in zero balance |
| TC-009 | Debit account - Insufficient funds | Account balance is 1000.00 | 1. Select option 3 2. Enter debit amount: 1500 | Display "Insufficient funds for this debit." and balance remains 1000.00 | | | Debit should not proceed if amount exceeds balance |
| TC-010 | Debit account - After credit (sufficient funds) | Account balance is 1000.00, then credited 500 | 1. Select option 2, enter 500 (balance = 1500) 2. Select option 3, enter 800 | Display "Amount debited. New balance: 000700.00" | | | Debit should work correctly after credit operation |
| TC-011 | Debit account - After credit (insufficient funds) | Account balance is 1000.00, then credited 200 | 1. Select option 2, enter 200 (balance = 1200) 2. Select option 3, enter 1500 | Display "Insufficient funds for this debit." and balance remains 1200.00 | | | Insufficient funds check should use current balance after credit |
| TC-012 | Debit account - Zero debit amount | Account balance is 1000.00 | 1. Select option 3 2. Enter debit amount: 0 | Amount debited (0), balance unchanged (1000.00) | | | System accepts zero as valid input |
| TC-013 | Invalid menu choice - Option 5 | Application running at main menu | 1. Select option 5 | Display error message "Invalid choice, please select 1-4." and menu is re-displayed | | | Invalid choices should be rejected with error message |
| TC-014 | Invalid menu choice - Option 0 | Application running at main menu | 1. Select option 0 | Display error message "Invalid choice, please select 1-4." and menu is re-displayed | | | Invalid choices should be rejected with error message |
| TC-015 | Exit program | Application running at main menu | 1. Select option 4 | Display "Exiting the program. Goodbye!" and application terminates | | | Exit should cleanly terminate the program |
| TC-016 | Data persistence - Balance after credit | Account balance is 1000.00 | 1. Select option 2, enter 250 (balance = 1250) 2. Select option 1 to view balance | Display "Current balance: 001250.00" | | | Updated balance should persist and be retrievable |
| TC-017 | Data persistence - Balance after debit | Account balance is 1000.00 | 1. Select option 3, enter 150 (balance = 850) 2. Select option 1 to view balance | Display "Current balance: 000850.00" | | | Updated balance after debit should persist and be retrievable |
| TC-018 | Multiple operations sequence | Account balance is 1000.00 | 1. Select 2, enter 300 (balance = 1300) 2. Select 3, enter 200 (balance = 1100) 3. Select 1 to verify 4. Select 2, enter 50 (balance = 1150) 5. Select 1 to verify | Final balance should be 1150.00 | | | System should correctly handle mixed credit and debit operations in sequence |
| TC-019 | Debit followed by insufficient funds attempt | Account balance is 1000.00 | 1. Select 3, enter 900 (balance = 100) 2. Select 3, enter 150 | Display "Insufficient funds for this debit." and balance remains 100.00 | | | Insufficient funds check after debit should work correctly |
| TC-020 | Menu loop continues after operation | Account has balance of 1000.00 | 1. Select option 1 (View Balance) 2. Menu should reappear | Menu is displayed again with all options available | | | After each operation, main menu should reappear for next action |

---

## Test Execution Guidelines

### Entry Criteria
- COBOL application successfully compiles without errors
- Application can be executed from command line: `./accountsystem`
- Initial balance is set to 1000.00

### Exit Criteria
- All test cases have been executed
- All Pass/Fail statuses have been recorded
- Any failures have been documented with root cause analysis

### Notes for Testers
- All monetary values are displayed with 6 digits total and 2 decimal places (e.g., 001000.00, 000250.50)
- The system maintains balance state throughout the session
- Invalid menu selections should not execute any transaction
- Balance cannot go negative through debit operations
- Program exits cleanly when option 4 is selected

---

## Business Rules Summary

1. **Initial Balance**: Account starts with 1000.00
2. **Credit Operation**: Adds amount to current balance
3. **Debit Operation**: Subtracts amount from current balance only if sufficient funds exist
4. **Insufficient Funds Check**: Debit operation fails if amount exceeds current balance
5. **Menu Validation**: Only options 1-4 are valid; other inputs show error message and re-display menu
6. **Data Persistence**: Balance changes persist across operations within the same session
7. **Program Exit**: User can cleanly exit via menu option 4
