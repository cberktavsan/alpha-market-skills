# Contributing to Alpha Vantage Toolkit

## Adding a New Skill

1. Fork the repository and create a feature branch
2. Place the skill under the appropriate plugin directory in `plugins/<plugin-name>/skills/<skill-name>/`
3. Create a `SKILL.md` with required YAML frontmatter:
   ```yaml
   ---
   name: skill-name
   description: One concise line describing what this skill does.
   metadata:
     version: "0.1.0"
   ---
   ```
4. Follow these conventions:
   - Keep SKILL.md under 500 lines; move detailed content to `references/`
   - Write in third person ("The assistant calls..." not "Call...")
   - Include a **Gotchas** section with real failure modes
   - Include a **Token Budget** section
   - Do NOT duplicate Global Rules (language, rate limits, premium exclusions) — reference the orchestrator
5. Update the plugin's orchestrator routing table
6. Submit a pull request

## Skill Quality Standards

- **Evidence-based**: Include specific thresholds, not vague guidance
- **Gotchas required**: Every skill must have 5-7 experience-derived failure modes
- **Token-conscious**: Challenge each paragraph — does the assistant really need this?
- **Platform-agnostic**: No vendor-specific tool names without abstraction

## Reporting Issues

- Describe the problem clearly
- Specify which plugin and skill are affected
- Include the API response or error if applicable

## Testing

Test skills manually with a free Alpha Vantage API key. Verify:
- Correct tool routing
- Rate limit awareness
- Output format quality
- Gotchas accuracy
