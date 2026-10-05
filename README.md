# Unclass for Claude

Draft and revise language lessons in your Unclass school with Claude. This plugin pairs the existing Unclass MCP connector with a lesson-authoring skill: choose the right course, create level-appropriate activities, preserve unrelated content when editing, and return lesson preview and editor links for teacher review.

You need an active Unclass teacher account and a Claude account with access to plugins and remote connectors. Availability depends on your Claude plan and workspace settings.

## Connect and use

1. Install the plugin in Claude. For a local test, zip this repository's contents, including the hidden .claude-plugin directory and .mcp.json file, without the .git directory. Upload through **Customize → Plugins → Add → Upload plugin**.
2. Open the plugin's **Connectors** tab, add or connect Unclass, and sign in through Unclass's OAuth screen.
3. Check the teacher, school, and requested permissions before approving. Read access supports browsing and review; write access is needed to save.
4. Ask Claude to list your courses, then select the course you want to work in.

The plugin uses the same server URL as the [published Unclass connector](https://claude.ai/directory/connectors/unclass-mcp), so an existing connection can be reused rather than exposing duplicate tools. Installing the plugin alone does not authorize access or change lessons.

### Example prompts

- “List my Unclass courses with their IDs. Do not change anything.”
- “In the course I select, create a draft 45-minute A2 Spanish lesson about ordering food, with a dialogue, vocabulary, and gap-fill exercises. Return preview and editor links.”
- “Read lesson ID 456 in course ID 123 and suggest three improvements without saving changes.”
- “Shorten text section ID 789 on page ID 321 in lesson ID 456, course ID 123, for an A2 learner. Leave everything else unchanged.”

Open **Preview lesson** to review saved content. Use **Edit lesson** for adjustments, then mark the lesson ready when satisfied. **Images are added in the website editor**; Claude leaves empty image sections for you to fill. Audio uploads also use the editor.

## Permissions and safety

Read access covers your school's courses, lessons, pages, and sections, including teacher notes, authored exercise answers, and saved media references. It is not limited to the lesson you mention in chat.

Write access additionally permits creating and editing content, duplication, movement and reordering, publishing, archiving, and deletion. The skill keeps new lessons as drafts and requires an explicit request or confirmation for publishing, archiving, or deletion. This guidance does not replace server-side authorization. Deletion has no undo, and the latest successful save wins.

The integration does not expose student records, student submissions or progress, messages, personal dictionaries, classroom invitation links, or another school's content. Frozen classroom lesson copies cannot be edited. Personal information placed inside accessible lesson content or teacher notes can still be shared with Claude; avoid including it.

## What runs and where data goes

This package contains static JSON configuration, a Markdown skill, and an icon. It has no executable scripts, hooks, local MCP server, package downloads, additional telemetry, or local credential collection.

Claude connects over HTTPS to **https://school.unclass.app/mcp**. Tool requests send the identifiers, queries, lesson text, exercises, and other authoring fields needed for the requested operation to Unclass. Unclass returns the requested course and lesson content, media references, and result links to Claude. Saved changes are stored in your Unclass school. The plugin does not independently upload your conversation or scan local files.

Authentication happens through Unclass OAuth, not by pasting a password or token into chat. The client handles access and refresh tokens; Unclass stores token secrets as hashes. Access tokens last 15 minutes, refresh tokens rotate, and the connection expires 30 days after authorization. If approved, identity permissions also share your account identifier and optionally email address.

Unclass stores its data in the EU; Anthropic processes content it receives under its own policies and your account settings, potentially outside the EU. See the [Unclass privacy policy](https://unclass.app/privacy) and [Anthropic privacy policy](https://www.anthropic.com/legal/privacy) for retention and processing details.

## Disconnect or get help

In the Unclass school portal, open **Account → AI assistant connections → Disconnect**. This revokes the connection without deleting lessons, undoing changes, or erasing content already received by Anthropic. Reauthorize after the 30-day expiry to continue. Removing the plugin is not a substitute for revoking the connection in Unclass.

- [Connection documentation](https://unclass.app/docs/claude)
- [Support](https://unclass.app/support)
- [Terms of service](https://unclass.app/terms)

## Validate and test before release

Run `claude plugin validate . --strict` in this repository. For a Claude Code test, start `claude --plugin-dir .`, authenticate via `/mcp`, and check that the lesson-authoring skill loads. For Claude chat and Cowork, upload the plugin to a test account and connect from its Connectors tab.

Use a disposable course to verify course selection, a draft lesson with preview links, a targeted edit preserving other content, read-only refusal, and disconnection. Check that review-only prompts make no writes, images are left for the editor, and ambiguous or destructive requests require clarification. Compare the same lesson prompts with and without the skill. Do not treat manifest validation as an end-to-end test.

Before submitting, run **Validate** in Anthropic's submission portal with this repository and the repository root as the plugin path. Bump the manifest version for each release.

## License

The plugin files are released under the [Unlicense](LICENSE). This does not license the Unclass application or service, users' lesson content, or grant trademark rights.
