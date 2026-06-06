## Development Conventions

### Predictable Structure
Organize files and directories in a logical, navigable layout.

### Up-to-Date Documentation
Keep README files current with setup steps, architecture overview, and contribution guidelines.

### Clean Version Control
Write clear commit messages, use feature branches, and add meaningful descriptions to pull requests.

### Environment Variables
Store configuration in environment variables; never commit secrets or API keys.

### Minimal Dependencies
Keep dependencies lean and up-to-date; document why major ones are included.

### Consistent Reviews
Follow a defined code review process with clear expectations for reviewers and authors.

### Testing Standards
Define required test coverage (unit, integration, etc.) before merging.

### Feature Flags
Use flags for incomplete features instead of long-lived branches.

### Changelog Updates
Maintain a changelog or release notes for significant changes.

### Build What's Needed
Avoid speculative code and "just in case" additions (see minimal-implementation.md).

### Flowbit Workflow Integration
- Read `@.flowbit/docs/INDEX.md` before starting any development task — it indexes the project's coding standards, tech stack, and architecture decisions.
- Follow the standards in `.flowbit/docs/standards/` when writing code. If any standard conflicts with the task at hand, raise it with the user rather than silently overriding.
- When a `/flowbit-*` command is invoked, execute it via the Skill tool immediately — do not skip or simplify workflows.
