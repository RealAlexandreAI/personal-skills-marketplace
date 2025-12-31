# /translate - Translate Documentation to Chinese

## Function
Translate documentation files from English to Chinese, ensuring terminology consistency, proper formatting, and MDX syntax correctness.

## Trigger Condition
When user inputs `/translate` without arguments or with optional file paths

## Automatic Workflow

When `/translate` is invoked, automatically execute the following steps:

### Step 1: Scan for Files Needing Translation
```bash
node dev-tools/scripts/translation-status.mjs scan
```

This will identify:
- Files that need translation updates (source changed)
- Files that are not yet translated
- Files that are up to date

### Step 2: Present Summary
Show the user:
- Total number of files needing translation
- List of outdated files
- List of untranslated files

### Step 3: Auto-translate All Files
If files need translation, automatically:
1. Create a todo list with all files to translate
2. Use the Task tool with general-purpose agent to translate each file in parallel (for efficiency)
3. Update translation status after each file
4. Mark todo items as completed

### Step 4: Final Verification
After all translations complete:
```bash
node dev-tools/scripts/translation-status.mjs scan
```

Report final statistics to the user.

## Manual Mode (Optional)

User can specify specific files to translate:
```
/translate <source_path> <target_path>
```

In this case, only translate the specified file pair.

## Behavior
1. **Automatic scan**: Check all files that need translation
2. **Batch translation**: Translate all outdated/untranslated files automatically
3. **Status tracking**: Update translation status for each file
4. **Quality assurance**: Follow terminology glossary and MDX syntax rules
5. **Final report**: Provide comprehensive translation statistics

## Translation Guidelines

### Terminology Glossary

The following terms must be preserved as-is or translated according to this glossary:

#### Technical Terms (Keep as-is)
- **API** = API
- **SDK** = SDK
- **CLI** = CLI
- **GitHub** = GitHub
- **Markdown** = Markdown
- **README** = README
- **Agent** = Agent
- **Human-in-the-loop** = Human-in-the-loop (do not translate)
- **JavaScript** = JavaScript
- **TypeScript** = TypeScript
- **Node.js** = Node.js
- **npm** = npm
- **package.json** = package.json
- **Git** = Git
- **repository** = 仓库
- **commit** = 提交
- **branch** = 分支
- **merge** = 合并
- **pull request** = 拉取请求
- **Build** = 构建
- **Integrate** = 集成
- **Doc** = 文档

#### Programming Languages and Tools
- **Python** = Python
- **pip** = pip
- **TypeScript** = TypeScript
- **JavaScript** = JavaScript
- **React** = React
- **Vite** = Vite
- **Express** = Express
- **FastAPI** = FastAPI

#### AI Agent Specific Terms
- **Agent** = Agent (never translate)
- **Human-in-the-loop** = Human-in-the-loop (never translate)
- **Tool** = 工具
- **Provider** = 提供者
- **Memory** = 记忆
- **State** = 状态
- **Streaming** = 流式
- **Interrupt** = 中断
- **Resume** = 恢复

### Translation Rules

1. **Preserve Code Blocks**: All code blocks, including comments, must be translated to Chinese, but preserve:
   - Variable names, function names, class names
   - Import paths
   - File paths
   - Configuration keys
   - API endpoints
   - Error messages that are part of code logic

2. **MDX Components**: Preserve all MDX component syntax exactly:
   - `<CodeGroup>`, `</CodeGroup>`
   - `<Card>`, `</Card>`
   - `<CardGroup>`, `</CardGroup>`
   - `<ResponseField>`, `</ResponseField>`
   - `<ParamField>`, `</ParamField>`
   - `<Tabs>`, `<Tab>`
   - `<Steps>`, `<Step>`
   - `<Note>`, `<Warning>`, `<Tip>`
   - `<AccordionGroup>`, `<Accordion>`

3. **Frontmatter**: Translate frontmatter fields:
   - `title`: Translate to Chinese
   - `description`: Translate to Chinese
   - Keep all other metadata as-is

4. **Headers**: Translate all markdown headers (H1-H6) to Chinese

5. **Links**: 
   - Translate link text to Chinese
   - Keep URLs as-is (unless they contain English paths that need translation)

6. **Code Comments**: Translate comments in code blocks to Chinese

### MDX Syntax Requirements

#### Critical Syntax Rules

1. **Tag Matching**: Ensure all opening tags have corresponding closing tags:
   - `<CodeGroup>` must have `</CodeGroup>`
   - `<Card>` must have `</Card>`
   - `<CardGroup>` must have `</CardGroup>`
   - `<ResponseField>` must have `</ResponseField>`
   - All other MDX components must be properly closed

2. **No Duplicate Blocks**: Remove duplicate code blocks or content sections

3. **Proper Indentation**: Maintain proper indentation for nested MDX components

4. **Code Block Syntax**: Ensure code blocks use proper triple backticks with language tags:
   ```markdown
   ```typescript
   // code here
   ```
   ```

5. **No Unexpected Closing Slashes**: Remove unexpected `/` characters in tags

### File Structure

Translation target directory structure:
- Source: `docs/src/frameworks/{path}`
- Target: `docs/src/zh/frameworks/{path}`
- Source: `docs/src/reference/{path}`
- Target: `docs/src/zh/reference/{path}`

## Quality Checks

After translation, verify:

1. ✅ All terminology matches the glossary
2. ✅ All MDX tags are properly closed
3. ✅ No duplicate content blocks
4. ✅ Code blocks have correct syntax
5. ✅ Links are properly formatted
6. ✅ Frontmatter is translated
7. ✅ All headers are translated
8. ✅ Comments in code are translated

## Common Issues to Fix

### Unclosed Tags
- **Error**: `Expected a closing tag for <CodeGroup>`
- **Fix**: Add missing `</CodeGroup>` tag

- **Error**: `Expected a closing tag for <Card>`
- **Fix**: Add missing `</Card>` tag or `</CardGroup>` tag

- **Error**: `Expected a closing tag for <ResponseField>`
- **Fix**: Add missing `</ResponseField>` tag

### Duplicate Content
- **Error**: Duplicate code blocks or sections
- **Fix**: Remove duplicate blocks, keep only one instance

### Syntax Errors
- **Error**: `Unexpected closing slash / in tag`
- **Fix**: Remove unexpected `/` characters or fix malformed tags

- **Error**: `Could not parse expression with acorn`
- **Fix**: Check for malformed JavaScript expressions in code blocks

## Translation Status Management

Before translating, check if translation is needed using the translation status manager:

```bash
node dev-tools/scripts/translation-status.mjs check <source_path> <target_path>
```

After translation, update the status:

```bash
node dev-tools/scripts/translation-status.mjs update <source_path> <target_path>
```

The status system tracks:
- Source file git hash (to detect changes)
- Target file path
- Last translation timestamp
- Translation status

### Automated Workflow with Status Management

The `/translate` command automatically handles the complete workflow:

1. **Auto-scan**: Automatically runs `node dev-tools/scripts/translation-status.mjs scan`
2. **Create Todo List**: Creates a todo list with all files needing translation
3. **Parallel Translation**: Uses Task tool to translate multiple files efficiently
4. **Auto-update Status**: Automatically updates status after each translation
5. **Final Verification**: Runs scan again to confirm all translations are complete

### Manual Workflow (for specific files)

If translating a specific file manually:

1. **Check Status**: Determine if translation is needed
   ```bash
   node dev-tools/scripts/translation-status.mjs check \
     docs/src/frameworks/quickstart.mdx \
     docs/src/zh/frameworks/quickstart.mdx
   ```

2. **Translate**: Perform translation following the guidelines

3. **Update Status**: After successful translation, update status
   ```bash
   node dev-tools/scripts/translation-status.mjs update \
     docs/src/frameworks/quickstart.mdx \
     docs/src/zh/frameworks/quickstart.mdx
   ```

### Available Commands

Scan all directories to find files needing translation:
```bash
node dev-tools/scripts/translation-status.mjs scan
```

List only outdated translations:
```bash
node dev-tools/scripts/translation-status.mjs list --outdated
```

## Usage Examples

### Auto-translate all files (recommended):
```
/translate
```
This will:
1. Scan for all files needing translation
2. Show summary of files to translate
3. Automatically translate all files in parallel
4. Update all translation statuses
5. Show final report

### Translate a specific file:
```
/translate docs/src/frameworks/quickstart.mdx docs/src/zh/frameworks/quickstart.mdx
```

### Translation Process
When `/translate` is invoked without arguments:
1. Runs `translation-status.mjs scan` to find files needing translation
2. Creates a todo list tracking all files
3. Delegates translation to Task tool (general-purpose agent) for each file
4. Each agent task will:
   - Read the source file
   - Translate following glossary rules
   - Write to target file
   - Update translation status
5. Marks each todo as completed
6. Runs final scan to verify all files are up to date

## Implementation Notes

### Automatic Mode (Default)
When user runs `/translate`:

1. **Scan Phase**:
   - Run `node dev-tools/scripts/translation-status.mjs scan`
   - Parse output to identify files needing translation
   - Create todo list with all files

2. **Translation Phase**:
   - For each file, use Task tool with general-purpose agent
   - Pass detailed prompt with:
     - Source and target file paths
     - Full terminology glossary
     - Translation rules
     - Status update command
   - Execute translations in parallel for efficiency

3. **Verification Phase**:
   - Run scan again to verify all files are up to date
   - Report final statistics

### Manual Mode (Specific Files)
When user provides file paths:

1. **Read Source File**: Read the English source file completely
2. **Parse Structure**: Identify MDX components, code blocks, and content sections
3. **Translate Content**: 
   - Translate text content to Chinese
   - Preserve technical terms per glossary
   - Maintain code syntax
4. **Fix Syntax**: Verify and fix any MDX syntax errors
5. **Write Target File**: Save translated content to target path
6. **Update Status**: Run `translation-status.mjs update`
7. **Verify**: Run MDX parser to check for errors

## Validation

After translation, validate the file by:
1. Checking tag matching (open/close tags)
2. Verifying code block syntax
3. Ensuring no duplicate content
4. Confirming terminology compliance
5. Testing MDX parsing if available

## Success Criteria

A translation is considered successful if:
- ✅ All content is translated to Chinese
- ✅ All technical terms follow the glossary
- ✅ MDX syntax is correct and parseable
- ✅ No duplicate content
- ✅ All tags are properly closed
- ✅ Code blocks are properly formatted
- ✅ Links and references work correctly

