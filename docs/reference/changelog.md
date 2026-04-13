# Changelog

Version history and release notes for Hermes Agent.

## [0.8.0] - 2026-04-01

### Added

- Profile support for multi-instance isolation
- Skin/theme engine for CLI customization
- MCP OAuth support for secure server connections
- Browser automation with CamoFox integration
- Real-time voice mode with transcription
- Subdirectory hint tracking for context files
- Budget configuration for API cost management
- Credential file management tool

### Changed

- Improved context compression algorithm
- Enhanced prompt caching efficiency
- Updated model metadata with provider-aware context
- Refined tool result storage with turn budget enforcement
- Better error classification for API failures

### Fixed

- Fixed context limit detection for local models
- Resolved memory plugin loading issues
- Corrected gateway session persistence
- Fixed tool availability checks for MCP tools

### Breaking Changes

- Config version bumped to 5 (automatic migration)
- HERMES_HOME path structure changed for profiles
- MCP tool registration API updated

## [0.7.0] - 2025-12-15

### Added

- Modal serverless terminal backend
- Daytona cloud environment support
- Singularity container backend
- RL training tool for Atropos integration
- Batch trajectory generation
- Trajectory compression for model training
- Evidence store for research

### Changed

- Improved terminal tool output handling
- Enhanced browser tool state management
- Better subagent delegation protocol

### Fixed

- Fixed background process notifications
- Resolved Docker backend permission issues
- Corrected SSH backend connection handling

## [0.6.0] - 2025-10-01

### Added

- Cron scheduling system
- Gateway hooks system
- Sticker cache for messaging platforms
- Webhook integration
- Email platform support
- Matrix protocol support
- Mattermost integration

### Changed

- Gateway session management improved
- Better platform delivery system
- Enhanced message mirroring

### Fixed

- Fixed Telegram long polling issues
- Resolved Discord rate limiting
- Corrected Slack message threading

## [0.5.0] - 2025-07-15

### Added

- Skills Hub integration
- Skill self-improvement during use
- FTS5 session search with LLM summarization
- Honcho dialectic user modeling
- Agent-curated memory with periodic nudges
- Autonomous skill creation after complex tasks

### Changed

- Memory system architecture overhaul
- Improved skill discovery mechanism
- Better conversation continuity across platforms

### Fixed

- Fixed memory persistence issues
- Resolved skill loading order
- Corrected session search performance

## [0.4.0] - 2025-05-01

### Added

- Browser automation tool (Browserbase)
- Web search with Firecrawl integration
- Image generation tool
- TTS (text-to-speech) tool
- Transcription tool for voice memos
- Multi-platform voice memo support

### Changed

- Tool system refactored to registry pattern
- Improved tool error handling
- Better tool result formatting

### Fixed

- Fixed tool timeout handling
- Resolved browser session cleanup
- Corrected web scraping edge cases

## [0.3.0] - 2025-03-01

### Added

- MCP (Model Context Protocol) integration
- Subagent delegation system
- Parallel tool execution
- Tool caching system
- Incremental context (delta context)
- Smart tool output summarization
- Memory decay system

### Changed

- Agent loop optimized for performance
- Better context management
- Improved tool orchestration

### Fixed

- Fixed context limit handling
- Resolved tool call parsing issues
- Corrected message formatting

## [0.2.0] - 2025-01-15

### Added

- Messaging gateway (Telegram, Discord, Slack)
- WhatsApp integration
- Signal protocol support
- Interactive CLI with Rich TUI
- Slash command system
- Conversation history persistence

### Changed

- CLI completely redesigned
- Better platform abstraction
- Improved session management

### Fixed

- Fixed gateway connection issues
- Resolved message delivery problems
- Corrected session state synchronization

## [0.1.0] - 2024-11-01

### Added

- Initial release
- Core AIAgent class
- Basic tool system
- CLI interface
- OpenAI-compatible API support
- Anthropic Claude support
- Local terminal execution
- File operations tools
- Web search tools

---

## Version Numbering

Hermes Agent follows semantic versioning:

- **MAJOR** version for incompatible changes
- **MINOR** version for backwards-compatible features
- **PATCH** version for backwards-compatible bug fixes

**Format:** `MAJOR.MINOR.PATCH`

## Release Schedule

- **Major releases:** Quarterly
- **Minor releases:** Monthly
- **Patch releases:** As needed

## Migration Guides

### Migrating to 0.8.0

**Profile Structure Changes:**

Old:
```
~/.hermes/
├── config.yaml
└── .env
```

New:
```
~/.hermes/
├── config.yaml (default profile)
├── .env
└── profiles/
    ├── work/
    │   ├── config.yaml
    │   └── .env
    └── personal/
        ├── config.yaml
        └── .env
```

**Migration:**
```bash
# Automatic migration on first run
hermes

# Manual migration
hermes profiles migrate
```

### Migrating to 0.5.0

**Memory System:**

Old memory format automatically migrated to new plugin architecture.

```bash
# Reinstall memory plugin
hermes plugins install honcho

# Rebuild memory index
hermes memory rebuild-index
```

## Deprecation Notices

### Deprecated in 0.8.0

- Direct `~/.hermes` access (use profiles instead)
- Manual tool registration (use registry pattern)

### Removed in 0.7.0

- Legacy context management
- Old tool system
- Deprecated gateway hooks

## Support

- **Documentation:** https://hermes-agent.nousresearch.com/docs/
- **Issues:** https://github.com/NousResearch/hermes-agent/issues
- **Discussions:** https://github.com/NousResearch/hermes-agent/discussions
