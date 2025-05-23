# 🚀 Aligna AI Usage Guide: Implementing Review Guidelines for AI Agent Teams

This guide explains **how** to effectively implement the Aligna AI review guidelines for teams of AI agents performing code and content reviews.

## 🏁 Getting Started

### Prerequisites

Before you begin, ensure you have the following:

- Supported languages/frameworks: Python 3.8-3.10, Node.js 14-16, or equivalent
- Required tools: Git, a text editor (e.g., VS Code), and a terminal
- API keys or credentials for any integrated services
- [Python installation guide](https://www.python.org/downloads/)
- [Node.js installation guide](https://nodejs.org/en/download/)

### For AI Agent Teams

1. **Initial Configuration**

   - Configure AI agents with the [Review Guidelines](REVIEW_GUIDELINES.md)
   - Specify which aspects are most relevant to your project context
   - Set up metrics tracking from [METRICS.md](METRICS.md) for continuous evaluation

2. **Integration Steps**

   - Incorporate the [Review Checklist](templates/review-checklist.md) into your AI agents' review protocol
   - Verify that the checklist file can be accessed correctly in both CI and local environments (path may need adjustment based on your setup)
   - Create domain-specific versions of the checklist if needed
   - To establish baseline metrics for comparison

   ```bash
   # Validate file existence in CI
   if [ ! -f templates/review-checklist.md ]; then
     echo "Error: review-checklist.md not found!"
     exit 1
   fi
   ```

### For Individual AI Agents

1. **As an Author Agent**

   - Apply the guidelines when generating content for submission
   - Perform self-review based on the principles before requesting peer review
   - Include context about areas where specific feedback is needed

2. **As a Reviewer Agent**

   - Reference the [Review Checklist](templates/review-checklist.md) during review processes
   - Complete the checklist systematically during evaluation
   - Balance thoroughness with efficiency in feedback generation

### Prompt Templates

#### Author Agent

```markdown
# Author Agent Prompt Template

## Context
- Describe the purpose of the content or code being generated.
- Highlight any specific areas where feedback is needed.

## Self-Review Checklist
- [ ] Have I followed the guidelines?
- [ ] Is the content clear and well-structured?
- [ ] Are there any potential edge cases or issues?

## Submission
- Provide any additional context or notes for the reviewer.
```

#### Reviewer Agent

```markdown
# Reviewer Agent Prompt Template

## Initial Assessment
- [ ] Do I understand the purpose of the submission?
- [ ] Is the scope appropriate for a single review?

## Technical Review
- [ ] Is the code or content clear and well-documented?
- [ ] Are there any potential edge cases or issues?
- [ ] Is the performance acceptable?

## Communication
- [ ] Are there any must-fix issues?
- [ ] Are there any suggestions for improvement?
- [ ] Is the feedback clear and actionable?

## Final Thoughts
- Provide an overall impression and key recommendations.

## Audit Trail
- [ ] Have I logged or exported the checklist results for audit purposes?
```

## 💡 Practical Examples

### Code Review Example

```markdown
# Review of PR #42: Add user authentication

## Initial Assessment
- [x] I understand this adds JWT-based authentication
- [x] Scope seems appropriate for a single PR
- [x] Approach aligns with our security practices

## Technical Review
- [x] Code is clear with good comments in complex sections
- [ ] Edge case: What happens when the token expires during an active session?
- [x] Performance looks good, no N+1 queries

## Communication
- [x] Must fix: Add password strength validation
- [ ] Suggestion: Consider extracting the JWT logic to a separate service
- [x] Great job documenting the API endpoints!

## Final Thoughts
- Overall impression: Positive
- Key recommendation: Make the requested changes, then ready to approve

I especially like how you handled error states with clear user messages.
```

### Documentation Review Example

```markdown
# Review of the API Documentation Update

## Initial Assessment
- [x] I understand this updates our REST API docs
- [x] Scope includes all new endpoints from Q1
- [x] Follows our documentation structure

## Technical Review
- [x] Content is clear and examples work when tested
- [ ] Edge case: Missing rate limit documentation
- [x] Examples cover both success and error responses

## Communication
- [x] Must fix: Authentication section needs the new token format
- [ ] Suggestion: Adding a sequence diagram would help users understand the flow
- [x] The troubleshooting section is excellent!

## Final Thoughts
- Overall impression: Positive
- Key recommendation: Add the authentication details, then approve

The improved navigation structure makes the docs much more usable.
```

## 🔄 Adapting to Your Context

### For Open-Source Projects

Focus on clear contribution guidelines and community standards.

#### For Academic Papers

Emphasize clarity of methodology and strength of conclusions.

#### For Design Reviews

Adapt to include user-experience considerations and design principles.

Remember that Aligna is a framework, not a strict rulebook. Adapt these practices to your AI agents' specific review domains and capabilities.

## 🤔 Common Questions

**Q: How strict should AI agents be with the checklist?**  
**A:** The checklist is a guidance tool. Configure agents to prioritize elements most relevant to your quality standards.

**Q: How can this integrate with existing AI review systems?**  
**A:** Incorporate Aligna principles into your AI agents' prompt engineering or review protocols.

**Q: How should AI agents handle subjective judgments?**  
**A:** Program agents to clearly indicate reasoning for subjective assessments and provide evidence-based justifications.

**Q: How do I customize the checklist?**  
**A:** You can customize the checklist by modifying the `templates/review-checklist.md` file to include criteria specific to your project or domain.

**Q: How often should metrics be reviewed?**  
**A:** Metrics should be reviewed regularly, ideally after each review cycle, to ensure continuous improvement and alignment with quality standards.

## 📝 Conclusion

By following these guidelines, your AI agent teams can deliver more consistent, helpful, and effective reviews across various domains.
