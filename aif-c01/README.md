# AIF-C01 Practice Exam Simulator

An offline, single-file HTML practice simulator for the **AWS Certified AI Practitioner (AIF-C01)** exam.

> ⚠️ **This is NOT an official AWS exam.** This is an independent study tool built on top of the official AWS exam guide and AWS documentation. It is not affiliated with, endorsed by, or sponsored by Amazon Web Services.

---

## My Story

I built this simulator while preparing for the AIF-C01 certification, and I passed. It was one of the tools I used throughout my study process, and it genuinely helped me reinforce concepts, identify weak spots, and verify whether I had actually absorbed the material before sitting the real exam.

That said, **this simulator does not replace more comprehensive resources** like [Tutorials Dojo](https://tutorialsdojo.com/) practice exams. I used those too. The goal here is different: this tool works best as a **complementary study companion**, something you run alongside your main materials to test yourself domain by domain, practice with immediate explanations, and keep coming back to the questions you got wrong.

If you are serious about passing, use this alongside (not instead of) full-length, up-to-date practice exams from dedicated providers.

---

## Features

| Feature | Description |
|---|---|
| **Real exam mode** | 65 questions, 90-minute countdown, domain-weighted sampling (D1 20% · D2 24% · D3 28% · D4 14% · D5 14%), feedback only at the end |
| **Custom mode** | Choose how many questions, feedback style (immediate / end / none), and timer on/off |
| **By domain** | Filter by any of the 8 domains, with a secondary subdomain filter |
| **Learning mode** | Choose a topic, get immediate explanations after every answer |
| **Pause & resume** | Freeze the timer mid-exam and come back later |
| **Per-question review** | After any exam, review every question with your pick, the correct answer, and a full explanation |
| **History** | Last 10 exams saved locally. Click any to re-review |
| **Mark to redo** | Flag questions you want to revisit (see section below) |
| **Dark mode** | Toggle light/dark theme; preference is saved |
| **Offline** | One HTML file, no server, no dependencies. Open it directly in any browser |

---

## Question Bank

**298 questions** across 8 domains:

| Domain | Name | Questions |
|---|---|---|
| D1 | Fundamentals of AI and ML | 39 |
| D2 | Fundamentals of Generative AI | 34 |
| D3 | Applications of Foundation Models | 54 |
| D4 | Guidelines for Responsible AI | 27 |
| D5 | Security, Compliance, and Governance for AI Solutions | 23 |
| D6 | Generative AI Capabilities *(focus domain)* | 46 |
| D7 | Responsible AI: Core Dimensions *(focus domain)* | 42 |
| D8 | AWS AI Services *(focus domain)* | 33 |

**D1–D5** are the official exam domains and appear in Real exam and Custom modes.  
**D6–D8** are supplemental focus domains, only available in By domain and Learning modes.

**Question types included:**
- Single-choice (standard)
- Multiple response: Select TWO or THREE
- Matching: associate AWS services or concepts to their definitions

---

## How to Use

1. Download `aif-c01-simulado.html`
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari)
3. No installation, no internet connection required

That's it.

---

## Exam Modes Explained

### Real Exam (65 questions)
Simulates the real AIF-C01 format:
- 65 questions sampled by official domain weights
- 90-minute countdown with pause support
- Feedback only at the end
- Scaled score 100–1000, passing = 700

### Custom
Pick your own question count, domains included, feedback timing, and timer settings.

### By Domain
Select one of the 8 domains. If the domain has multiple subdomains (e.g., D1 has "ML fundamentals," "Managed AI services," and "Eval metrics"), a secondary filter appears automatically.

### Learning
Select a topic from any domain. Every question shows the correct answer and a full explanation immediately, with no waiting until the end. Best for targeted study sessions.

---

## Mark to Redo

During any quiz, a **"Mark to redo"** button appears on each question. Marking it flags the question for future review.

On the setup screen, a **"Marked to redo"** panel shows all flagged questions with options to:
- View the question text
- Remove individual items
- Copy the full list (to paste into Claude or another tool for analysis)
- Clear all marks at once

Marks persist across sessions via `localStorage`.

> **Planned feature (not yet implemented):** The intended behavior is that once a question is marked, it stops appearing in future exam sessions automatically, until you modify it or manually remove the mark. This automatic exclusion is **not yet implemented**. Currently, marked questions still appear in the exam pool normally; the feature only surfaces them in the review list on the setup screen.

---

## AWS MCP: How the Questions Were Generated

This project uses the same content-generation approach that **[Kiro](https://github.com/aws-samples/sample-kiro-study-buddy)** introduced for AWS study tools: the **AWS Documentation MCP server** (`awslabs.aws-documentation-mcp-server`).

The MCP (Model Context Protocol) server gives an AI assistant direct access to the official AWS documentation. Every question in this simulator was generated with that context, meaning the AI could verify:
- What a service actually does and when to use it
- Correct service names, feature names, and pricing models
- Best practices as documented by AWS

This approach substantially reduces hallucinations compared to asking a model to generate questions from training memory alone.

### Want to generate your own questions?

Any AI assistant that supports MCP can use this server: Claude, Cursor, Windsurf, or any other MCP-compatible tool. The steps below use **Claude Code** as an example, but the same MCP server works with other tools in their own MCP configuration.

1. **Install an MCP-compatible AI assistant** (e.g., [Claude Code](https://claude.ai/code), [Cursor](https://www.cursor.com/), [Windsurf](https://windsurf.com/))
2. **Add the AWS Documentation MCP server** to your assistant's MCP config:
   ```bash
   # Example using Claude Code CLI
   claude mcp add awslabs.aws-documentation-mcp-server \
     --scope user \
     -e FASTMCP_LOG_LEVEL=ERROR \
     -- uvx awslabs.aws-documentation-mcp-server@latest
   ```
   For other tools, refer to their MCP configuration docs and point them to `uvx awslabs.aws-documentation-mcp-server@latest`.
3. **Open the project folder** in your AI assistant
4. **Ask it to generate questions**, for example:
   > "Generate 10 new scenario-based questions for D3 (Applications of Foundation Models) covering RAG and vector stores. Use the AWS MCP to verify service behavior. Add them to the BANK array in the HTML file."

The AI will consult the live AWS docs to write and verify each question before inserting it into the file.

---

## Project Structure

```
aif-c01-simulado.html   ← the entire app (HTML + CSS + JS + question bank)
README.md               ← this file
```

Everything is self-contained in a single file. The question bank is a plain JavaScript array (`BANK`) near the top of the `<script>` section, easy to edit directly.

---

## How to Add or Edit Questions

Each question in `BANK` follows one of three formats:

**Single-choice:**
```js
{d:"d1", c:"ml-basics", q:"Question text here?",
 o:["Option A", "Option B", "Option C", "Option D"],
 a:0,  // index of the correct option
 e:"Explanation text. Why correct is correct, why others are wrong."}
```

**Multiple response (Select TWO or THREE):**
```js
{d:"d2", c:"genai-core", q:"Question text? (Select TWO.)",
 o:["Option A", "Option B", "Option C", "Option D", "Option E"],
 a:[0, 2],  // array of correct option indexes
 e:"Explanation."}
```

**Matching:**
```js
{d:"d4", c:"responsible-ai", type:"match",
 q:"Match each item to its category.",
 pairs:[
   {p:"Prompt text for pair 1", opts:["A","B","C","D"], a:0},
   {p:"Prompt text for pair 2", opts:["A","B","C","D"], a:1}
 ],
 e:"Explanation covering all pairs."}
```

**Domain keys:** `d1` through `d8`  
**Concept keys:** defined in the `CONCEPTS` object near the top of the script. Add a new key there if you create a new subdomain.

---

## Disclaimer

- This is an **unofficial** community-made study tool
- Questions are based on the [AIF-C01 Exam Guide](https://aws.amazon.com/certification/certified-ai-practitioner/) and AWS documentation
- AWS certification exams are updated periodically. Always verify against the latest official exam guide
- Passing this simulator does not guarantee passing the real exam
- AWS, Amazon SageMaker, Amazon Bedrock, and all other AWS product names are trademarks of Amazon Web Services

---

## Resources

- [AIF-C01 Exam Guide (official)](https://aws.amazon.com/certification/certified-ai-practitioner/)
- [AWS AI Practitioner Learning Path](https://aws.amazon.com/training/learn-about/machine-learning/)
- [Tutorials Dojo AIF-C01 Practice Exams](https://tutorialsdojo.com/courses/aws-certified-ai-practitioner-practice-exams/) ← highly recommended for full preparation
- [AWS Documentation MCP Server](https://github.com/awslabs/mcp/tree/main/src/aws-documentation-mcp-server)
- [Kiro Study Buddy (inspiration)](https://github.com/aws-samples/sample-kiro-study-buddy)

---

## License

MIT License.

---

**Disclaimer.** Not affiliated with, endorsed by, or associated with Amazon Web Services, Inc. AWS, Amazon Web Services, and all related marks are trademarks of Amazon.com, Inc. or its affiliates.
