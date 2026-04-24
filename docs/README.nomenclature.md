**What are skills?**

Agent Skills are self-contained folders with instructions and bundled resources that enhance AI capabilities for specialized tasks.
Based on the Agent Skills specification, each skill contains a SKILL.md file with detailed instructions that agents load on-demand.

Skills differ from other primitives by supporting bundled assets (scripts, code samples, reference data) that agents can utilize when performing specialized tasks.

Each skill is a folder containing a SKILL.md instruction file
Skills may include helper scripts, code templates, or reference data

Skills are lightweight, single-purpose tools that the main agent can invoke as part of its own workflow. 
They run within its context window and augment what it can do. Think of them like plugins or callable functions.

**What are agents?**

Agents are fully separate, independent AI agents with their own context window, toolset, and reasoning loop. They run outside of the main model and it can delegate work to them.

**What is a context window?**

The context window is the total amount of text (measured in tokens) that an AI model can "see" and reason about at one time — like its working memory.

It includes:

 - The full conversation history
 - Any files or code you've shared
 - Tool outputs and results
 - System instructions

You can see your current context usage with /context in the copilot CLI.
