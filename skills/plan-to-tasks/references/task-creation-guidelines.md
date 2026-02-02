# Task Creation Guidelines

Task lists consist of two structures

Use the following JSON object format when creating a task:

```json psuedocode
{
  "tasks": [
    {
      "id": <FEATURENAME>-<INCREMENTING_ID>,
      "branchName": One of `<NOTICKET-featureName>` or `linear/<TICKETNAME>` depending on whether feature is available,
      "ticket": <hyphenated-ticket-name>,
      "title": task title,
      "implementationSteps": [array of strings representing steps to implement],
      "acceptanceCriteria": [array of strings representing verification tasks],
      "passes": boolean intially set to false and marked true after all acceptanceCriteria have been verified,
    },

  ]
}

```

Example tasks:

```json
{
  "tasks": [
    {
      "id": "login-form-001",
      "branchName": "linear/add-login-form",
      "title": "Add login form",
      "implementationSteps": [
        "Create form react component"
        "Create email field with validation"
        "Create password field"
      ],
      "acceptanceCriteria": [
        "Email/password fields",
        "Validates email format",
        "typecheck passes"
      ],
      "passes": false
    },
    {
      "id": "login-form-002",
      "branchName": "linear/add-login-form",
      "title": "Add password reset",
      "implementationSteps": [
        "Add reset password link",
        "Send email with reset link",
        "Create reset token with 1 hour ttl"
      ],
      "acceptanceCriteria": [
        "Reset link sent via email",
        "Token expires after 1 hour"
      ],
      "passes": false
    }
  ]
}
```
