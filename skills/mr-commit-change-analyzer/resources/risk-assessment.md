# Risk Assessment Guidelines

When analyzing changes, look for these potential risk factors:

## High Risk
- API signature changes that break backward compatibility
- Authentication/authorization logic modifications
- Data persistence layer changes (database schema, storage format)
- Critical path modifications (payment processing, user authentication, core business logic)
- Security-related code changes
- Changes that remove existing functionality without deprecation

## Medium Risk
- Performance-critical code modifications
- Third-party dependency updates (especially major version changes)
- Error handling and exception flow changes
- Changes to shared utility functions used across multiple modules
- Configuration changes that affect production environments

## Low Risk
- Documentation updates
- UI/UX changes that don't affect backend logic
- Test code modifications
- Code formatting and style changes
- Logging and monitoring improvements
- Non-critical feature additions that are isolated to specific modules

## Flags for Further Review
- Changes without corresponding test updates
- Large, unfocused commits that touch multiple unrelated modules
- Ambiguous or missing commit messages/PR descriptions
- Changes that introduce magic numbers or hardcoded values
- Code that deviates significantly from established project patterns
