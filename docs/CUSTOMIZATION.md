# Customization Guide

## Adapting squad-dev for your project

### 1. Define your modules

Edit the MODULES section in `skills/squad-dev/SKILL.md`:

```markdown
## MODULES (CUSTOMIZE THIS SECTION)

| Module | Description | Backend Path | Frontend Path |
|--------|-------------|--------------|---------------|
| api | REST API endpoints | src/api/ | - |
| web | React frontend | - | src/components/ |
| mobile | React Native app | - | mobile/src/ |
| shared | Shared utilities | packages/shared/ | packages/shared/ |
```

### 2. Define your Tech Leads

Update the Tech Leads table to match your modules:

```markdown
## Available Tech Leads

| Agent | Domain | Typical Paths |
|-------|--------|---------------|
| @api-lead | API endpoints, middleware | src/api/, src/middleware/ |
| @web-lead | React components, pages | src/components/, src/pages/ |
| @mobile-lead | React Native screens | mobile/src/screens/ |
| @shared-lead | Shared packages | packages/shared/ |
```

### 3. Customize EM prompt

In the Engineering Manager prompt, update the domain list:

```
## WHEN SPRINT STARTS
1. Analyze sprint stories
2. Identify which domains are affected:
   - API (endpoints, middleware, validation)
   - Web (components, pages, state management)
   - Mobile (screens, navigation)
   - Shared (utilities, types)
3. Send CEO a SPAWN REQUEST for each Tech Lead needed
```

## Creating module-specific squads

If you want separate squads per module (like Hivanced does), duplicate and adapt:

### Example: squad-api

1. Copy `squad-dev/SKILL.md` to `squad-api/SKILL.md`
2. Update frontmatter:
   ```yaml
   ---
   name: squad-api
   description: Launch the API squad
   argument-hint: "auto [optional pitch]"
   ---
   ```
3. Focus the MODULES section on API only
4. Update Tech Leads to API-specific roles:
   - @routes-lead
   - @middleware-lead
   - @validation-lead

### Example: squad-web

Same process, but focused on frontend:
- @components-lead
- @pages-lead
- @state-lead

## Customizing the Agile process

### Shorter sprints

By default, sprints run until features are done. To enforce time limits, add to SM prompt:

```
## SPRINT CONSTRAINTS
- Sprint duration: 2 hours max
- After 2 hours, force Sprint Review regardless of completion
```

### Different estimation

Add to EM prompt:
```
## ESTIMATION METHOD
Use T-shirt sizes (XS, S, M, L, XL) instead of story points.
XS = trivial, XL = needs to be broken down.
```

### Specific coding standards

Add to Tech Lead spawn prompts:
```
## CODING STANDARDS
- Use TypeScript strict mode
- All functions must have JSDoc comments
- Maximum file length: 200 lines
- Use functional components only (React)
```

## Adding new agent types

### Example: @devops-lead

Add to the Tech Leads table:
```markdown
| @devops-lead | CI/CD, infrastructure | .github/, terraform/, k8s/ |
```

Add to EM's domain list:
```
- DevOps (CI/CD pipelines, deployment configs, infrastructure)
```

### Example: @security-lead

```markdown
| @security-lead | Security, auth, encryption | src/auth/, src/crypto/ |
```

## Customizing QA

### Adding module-specific testers

Update QA Manager's TESTERS table:
```markdown
| Tester | Module | Frontend Path |
|--------|--------|---------------|
| @tester-api | API | - (uses curl/httpie) |
| @tester-web | Web | /app/* |
| @tester-mobile | Mobile | - (uses mobile emulator) |
```

### Custom test types

Add to QA Analyst prompt:
```
## TEST TYPES TO INCLUDE
- [ ] Smoke Testing
- [ ] Functional Testing
- [ ] Integration Testing
- [ ] Performance Testing (response times < 200ms)
- [ ] Security Testing (OWASP Top 10)
- [ ] Accessibility Testing (WCAG 2.1 AA)
```

### Different bug priorities

Customize the BACKLOG.md template:
```markdown
## Severity Definitions

| Severity | Definition | SLA |
|----------|------------|-----|
| Critical | App crash, data loss | 4 hours |
| Major | Feature broken, workaround exists | 24 hours |
| Minor | Cosmetic, edge case | Next sprint |
| Trivial | Nice to have | Backlog |
```

## Integration with your workflow

### GitHub Issues

Add to QA Manager prompt:
```
## BUG TRACKING
After creating bug reports, also create GitHub issues:
- Use `gh issue create` command
- Add labels: bug, [module], [priority]
- Assign to appropriate team
```

### Slack notifications

Add to SM prompt:
```
## NOTIFICATIONS
At end of each sprint, post summary to Slack:
- Use webhook: $SLACK_WEBHOOK_URL
- Include: sprint goal, completed stories, blockers
```

### Jira integration

Add to PO prompt:
```
## BACKLOG SYNC
Keep backlog in sync with Jira:
- Create Jira tickets for new stories
- Update status when stories complete
- Use labels for sprint assignment
```

## Multi-language projects

### Python backend + React frontend

```markdown
| Module | Description | Path | Language |
|--------|-------------|------|----------|
| api | FastAPI backend | backend/ | Python |
| web | React frontend | frontend/ | TypeScript |

| Agent | Domain | Language Focus |
|-------|--------|----------------|
| @api-lead | Backend endpoints | Python, FastAPI |
| @web-lead | React components | TypeScript, React |
```

### Monorepo setup

```markdown
| Module | Description | Path |
|--------|-------------|------|
| @apps/web | Main web app | apps/web/ |
| @apps/mobile | Mobile app | apps/mobile/ |
| @packages/ui | Shared UI | packages/ui/ |
| @packages/core | Core logic | packages/core/ |
```

## Tips

1. **Start simple** - Use the default config first, customize as needed
2. **Document changes** - Add comments explaining why you customized
3. **Test incrementally** - After each change, run a quick `/squad-dev auto` test
4. **Version control** - Keep skills in git, track what works
5. **Share learnings** - If you find good patterns, contribute back!
