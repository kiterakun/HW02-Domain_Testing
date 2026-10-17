# domain-bva-testing

## Description
When the user provides a feature name and the requirements or description for a software feature (e.g., "Perform Domain Testing and Boundary Value Analysis for the Login feature"), trigger this skill to execute the combined Domain Testing and Boundary Value Analysis workflow. The agent will systematically analyze the feature, generate test cases following the structured steps, and output results to organized directories. This skill is specifically for software testing of each feature in a product.

## Compatibility
- Requires no external tools beyond standard shell and file operations.
- Works with any text-based requirements provided by user.

## Workflow

The skill will guide the AI agent through the following steps in order. At each major step, the agent will:
1. Perform the step.
2. Present the results to the user.
3. Wait for user confirmation or corrections.
4. Iterate until the user approves the output for that step.
5. Proceed to the next step only after approval.

If the user provides incomplete, unclear, or contradictory information at any point, the agent will proactively ask clarifying questions before proceeding.

### Step 1: Read Requirements and Survey (Domain Testing Foundation)
**Action:** Read the provided feature name and requirements. Identify the main user flows, preconditions, input data, expected outputs, and business rules that can be tested.

**Output to user:** Summary of identified flows, preconditions, inputs, outputs, and testable rules. Ask for confirmation or corrections. If incomplete, ask questions.

**Example Output:**
```
Feature: Login
Requirements: User enters username and password. Valid credentials allow access to dashboard. Invalid credentials show error message.
Flows:
- Successful login: username/password correct → dashboard
- Failed login: invalid credentials → error message
Preconditions: None
Inputs: username (string), password (string)
Outputs: dashboard page, error message
Testable rules:
- Length of username: 3-20 chars
- Password: min 8 chars, with uppercase, number
```

**Approval required:** User must confirm or provide corrections.

---

### Step 2: Identify Influencing Variables and Conditions
**Action:** List all input fields (variables) and possible states/conditions that can affect the test results.

**Output to user:** List of variables and conditions with descriptions. Ask for confirmation or additions.

**Example Output:**
```
Variables:
- username: string, length 3-20
- password: string, min length 8, mixed case, number
- login_attempt: number of attempts (1-3)
- session_state: logged_in / not_logged_in

Conditions:
- Valid data vs invalid data
- Multiple login attempts
- Edge cases like empty fields
```

**Approval required.**

---

### Step 3: Define Value Domains (Valid and Invalid Values)
**Action:** Based on requirements and any code or UI provided, describe valid and invalid values for each variable, with evidence references.

**Output to user:** Domain descriptions for each variable. Ask for confirmation or corrections. If source unclear, ask user for more info.

**Example Output:**
```
username domain:
- Valid: 3-20 alphanumeric characters
- Invalid: <3 chars, >20 chars, special chars
- Evidence: Requirements say 3-20 chars

password domain:
- Valid: min 8 chars, uppercase, lowercase, number
- Invalid: <8, no uppercase, etc.
- Evidence: Business rule in spec

login_attempt:
- Valid: 1-3
- Invalid: 0, 4+
```

**Approval required.**

---

### Step 4: Partition into Equivalence Classes
**Action:** Group values that are expected to produce similar behavior into equivalence classes. Separate classes that may lead to different outcomes.

**Output to user:** List of equivalence classes for each variable. Ask for confirmation.

**Example Output:**
```
username classes:
- Class 1 (Valid): 3-20 chars
- Class 2 (Too short): 1-2 chars
- Class 3 (Too long): 21+ chars
- Class 4 (Invalid chars): special chars

password classes:
- Class 1: valid format
- Class 2: too short
- Class 3: no uppercase
- Class 4: no number
```

**Approval required.**

---

### Step 5: Select Important Condition Combinations
**Action:** Choose critical test combinations, considering interactions between conditions.

**Output to user:** Selected combinations and rationale. Ask for confirmation.

**Example Output:**
```
Important combinations:
1. username valid + password valid
2. username too short + password valid
3. username valid + password too short
4. username valid + password no uppercase
5. login_attempt = 3 + invalid credentials
```

**Approval required.**

---

### Step 6: Select Representative Values
**Action:** Choose at least one representative value per important class; add negative cases and boundary states.

**Output to user:** Representative values and rationale. Ask for confirmation.

**Example Output:**
```
Selected values:
- username: "ab" (2 chars), "abcde" (5 chars), "abcde123456789012345" (21 chars)
- password: "Abc12345" (min valid), "Abc" (too short), "abc123" (no uppercase), "Abc123" (no number)
```

**Approval required.**

---

### Step 7: Complete Test Cases
**Action:** Create detailed test cases including:
- Test ID
- Preconditions
- Input data
- Steps
- Expected results
- References

For BVA, apply the 3-point rule where applicable.

**Output to user:** Full list of test cases in Markdown format. Ask for confirmation or changes.

**Example Output:**
```
Test Case 1: ID=TC-001
Precondition: None
Input: username="ab", password="Abc12345"
Steps:
1. Enter credentials
2. Submit
Expected: Error message "Username too short"
Reference: Domain Testing step 6, Equivalence Class "Too short"
```

**Approval required.**

---

### Step 8: Coverage Evaluation
**Action:** Evaluate coverage of equivalence classes and boundaries. Note what has been covered and what is missing, with reasons.

**Output to user:** Coverage report in Markdown. Ask for final approval to proceed.

**Example Output:**
```
Coverage:
- Equivalence classes:
  - username: 3/4 classes covered
  - password: 4/4 classes covered
- Boundaries:
  - Length boundaries: min, min-1, min+1 covered
  - Missing: exact max length test (user to confirm if needed)

Missing:
- Test for max password length due to unclear requirement
```

**Final Approval:** User must confirm before ending.

## Output Directory Structure
Create the following structure in a new folder named after the feature (e.g., `login-feature`):

```
<feature-name>/
├── feature-overview.md                  # Thông tin chung về feature (yêu cầu, flows, variables)
├── domain-testing/
│   ├── test-cases.md                    # Domain Testing test cases
│   ├── coverage-report.md
│   ├── ai-mistake.md                    # Sai sót của agent
│   └── requirements-summary.md
├── bva/
│   ├── test-cases.md                    # BVA test cases
│   ├── bva-values.md                    # 3-point values
│   ├── coverage-report.md
│   ├── ai-mistake.md                    # Sai sót của agent
│   └── requirements-summary.md
└── ai-mistake.md                       # Aggregate tất cả mistakes từ 2 section
```

## Templates

### 1. Template cho `feature-overview.md`
```markdown
# Feature Overview

**Feature Name:** [Tên feature]
**Description:** [Mô tả ngắn gọn]
**Version:** [nếu có]

## Requirements
[Copy nguyên văn yêu cầu người dùng đưa ra]

## User Flows
- Flow 1: ...
- Flow 2: ...

## Input Variables
- username: ...
- password: ...

## Expected Outputs
- Dashboard
- Error message

## Business Rules
- ...
```

### 2. Template cho `domain-testing/test-cases.md`
```markdown
# Domain Testing - Test Cases

**Feature:** [Tên feature]

## Test Case 1
**ID:** TC-001  
**Precondition:** None  
**Input Data:** username="ab", password="Abc12345"  
**Steps:**  
1. Enter credentials  
2. Submit  
**Expected Result:** Error message "Username too short"  
**Reference:** Domain Testing Step 6, Equivalence Class "Too short"
```

### 3. Template cho `bva/test-cases.md`
```markdown
# Boundary Value Analysis - Test Cases

**Feature:** [Tên feature]

## Test Case 1
**ID:** BVA-001  
**Precondition:** None  
**Input Data:** username="ab" (length-1), "abc" (min), "abcde" (min+1)  
**Steps:**  
1. Enter username  
2. Submit  
**Expected Result:** Error message for boundary cases  
**Reference:** BVA - Length boundary (min-1 / min / min+1)
```

### 4. Template cho `coverage-report.md` (cả 2 section)
```markdown
# Coverage Report

## Equivalence Classes Coverage
- username: 3/4 classes covered
- password: 4/4 classes covered

## Boundaries Coverage
- Length: ✅ min, min-1, min+1
- Missing: exact max length (reason: unclear requirement)

## Summary
[Đánh giá tổng thể]
```

## Triggering
This skill triggers when the user explicitly says something like:
- "Perform Domain Testing and BVA for [feature name]"
- "Run the testing workflow for [feature]"

## Usage
1. User calls `/domain-bva-testing`
2. Provide feature name + requirements
3. Agent follows 8 steps, seeking approval at each step
4. All files are generated according to the templates above

## Error Handling
- All mistakes are logged in `ai-mistake.md` files.
- At the end, the agent summarizes all mistakes in the top-level `ai-mistake.md` file.