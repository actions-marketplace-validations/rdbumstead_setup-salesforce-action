# Contributing to Setup Salesforce CLI Action

Thank you for your interest in contributing!

## How to Contribute

### Reporting Bugs

1. Check existing issues
2. Create new issue with:
   - Clear title
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details
   - Sanitized workflow logs

### Suggesting Features

1. Open issue labeled `enhancement`
2. Describe:
   - Problem you're solving
   - Proposed solution
   - Alternatives considered
   - Impact on existing users

### Pull Requests

1. Fork repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Make changes following existing patterns
4. Test thoroughly
5. Commit: `git commit -m "feat: add custom CLI version support"`
6. Push and create PR

## Development Guidelines

### Code Style

- Use clear variable names
- Add comments for complex logic
- Keep steps focused
- Use `set -e` for error handling

### Testing Checklist

Before submitting PR:

- [ ] Test with scanner enabled/disabled
- [ ] Test with delta enabled/disabled
- [ ] Test with all dev tools enabled
- [ ] Test minimal setup (with auth)
- [ ] Test CLI-only setup (`skip_auth: 'true'` without jwt_key, client_id, username)
- [ ] Test with Dev Hub
- [ ] Test cache hit scenario
- [ ] Test cache miss scenario

### Documentation

Update docs for:

- New inputs/outputs
- Changed behavior
- New use cases
- Breaking changes

### Commit Messages

    type(scope): short description

    Longer description if needed

    Fixes #123

**Types:** feat, fix, docs, chore, refactor, perf

## Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Accept constructive criticism
- Focus on what's best for the community

## Questions?

- Open issue labeled `question`
- Email: ryan@ryanbumstead.com

## License

By contributing, you agree your contributions will be licensed under MIT License.
