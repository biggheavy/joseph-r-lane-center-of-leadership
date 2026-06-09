# Contributing Guidelines

## Welcome Contributors!

Thank you for your interest in contributing to the Joseph R Lane Center Of Leadership. We welcome contributions from everyone, regardless of experience level.

---

## Ways to Contribute

### 1. Code Contributions
- **Bug fixes** - Help us fix issues
- **Features** - Build new functionality
- **Optimization** - Improve performance
- **Testing** - Enhance test coverage
- **Documentation** - Improve guides and examples

### 2. Content Contributions
- **Course materials** - Create or improve courses
- **Case studies** - Share real-world applications
- **Articles** - Write about leadership topics
- **Videos** - Create educational content
- **Translations** - Help us reach global audience

### 3. Community Contributions
- **Moderation** - Help maintain community standards
- **Mentoring** - Guide new members
- **Event organization** - Plan community events
- **Feedback** - Help us improve
- **Ideas** - Suggest new features

---

## Getting Started

### Prerequisites
- GitHub account
- Git installed locally
- Python 3.8+ (for code contributions)
- Familiarity with our Code of Conduct

### Setup for Development

```bash
# Clone the repository
git clone https://github.com/biggheavy/joseph-r-lane-center-of-leadership.git
cd joseph-r-lane-center-of-leadership

# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run tests
pytest
```

---

## Development Workflow

### 1. Fork the Repository
Click "Fork" on GitHub to create your own copy

### 2. Create a Branch
```bash
git checkout -b feature/your-feature-name
```

**Branch naming conventions:**
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation updates
- `test/` - Test additions

### 3. Make Changes

**Code style:**
- Follow PEP 8 for Python
- Use meaningful variable names
- Add comments for complex logic
- Keep functions small and focused

### 4. Write Tests

```bash
# Run existing tests
pytest

# Write new tests in tests/ directory
```

### 5. Commit Your Changes

```bash
git add .
git commit -m "feat: add quantum circuit testing framework"
```

**Commit message format:**
```
[type]: [subject]

[description]

[issue reference]
```

**Types:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `test:` - Test addition
- `refactor:` - Code refactoring
- `perf:` - Performance improvement

### 6. Push and Create Pull Request

```bash
git push origin feature/your-feature-name
```

Then create a Pull Request on GitHub with a clear description.

---

## Documentation Contributions

### Adding Documentation

**Update existing docs:**
1. Edit the markdown file
2. Follow existing formatting
3. Include examples
4. Submit PR

**Add new documentation:**
1. Create `.md` file in appropriate folder
2. Follow template structure
3. Include code examples
4. Update navigation links
5. Submit PR

---

## Testing Guidelines

### Writing Tests

```python
import pytest

class TestFeature:
    def test_basic_functionality(self):
        """Test that feature works"""
        result = function_under_test()
        assert result is not None
```

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src

# Run specific test
pytest tests/test_file.py::test_function
```

---

## Reporting Issues

### Before Submitting
- Check existing issues
- Verify on latest version
- Provide clear details

### Issue Template

```markdown
## Description
Brief description of the issue

## Steps to Reproduce
1. Step 1
2. Step 2

## Expected Behavior
What should happen

## Actual Behavior
What actually happened

## Environment
- Python version: 3.9
- OS: macOS/Windows/Linux

## Screenshots
[If applicable]
```

---

## Code Review Process

### What We Look For
- ✅ **Functionality** - Does it work correctly?
- ✅ **Quality** - Is code well-written?
- ✅ **Tests** - Is it well-tested?
- ✅ **Documentation** - Is it explained?
- ✅ **Performance** - Is it efficient?

### Addressing Feedback
1. Thank the reviewer
2. Make requested changes
3. Commit changes
4. Comment when ready for re-review

---

## Recognition

We deeply appreciate all contributions! Contributors are:
- Listed in README
- Mentioned in release notes
- Given special badges
- Recognized annually

---

## Code of Conduct

All contributors must follow our [Code of Conduct](CODE_OF_CONDUCT.md).

---

## Getting Help

### Questions?
- **GitHub Discussions:** Ask in the Q&A section
- **Email:** contributors@josephrlanecenter.com
- **Slack:** #contributors channel
- **Office Hours:** Thursday 3-5 PM EST

---

## Licensing

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

*Last Updated: May 25, 2026*
*For questions: contributors@josephrlanecenter.com*