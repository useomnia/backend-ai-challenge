# Backend & AI Engineering Challenge

## Overview

You will build an **AI-powered Brand Mention Analyzer** that processes web content to extract competitive intelligence about brand mentions, rankings, and feature evaluations **relevant to a given search prompt**.

This challenge tests skills critical to our work: designing AI extraction workflows, handling LLM limitations, data modeling for analytics, and building systems that produce actionable insights.

**Appetite**: We're setting a maximum investment of **2 hours** for this challenge. This is intentionally tight - you won't be able to do everything well. Part of what we're evaluating is how you prioritize:

- Which requirements deliver the most value?
- What can you cut or simplify without compromising the core?
- Where do you draw the line between "good enough" and "polished"?

Document your scoping decisions. Tell us what you chose to leave out and why. This analysis is as important as the code itself.

**You may use AI coding assistants** (Claude, Copilot, etc.). We encourage it - we want to see how you work with these tools. Be prepared to explain your prompts, decisions, and any manual corrections you made.

**Need resources?** Ask us for anything you need to complete this challenge:

- API keys (OpenAI, Anthropic, etc.)
- Claude Code invitation
- Access to specific tools or services
- Clarification on requirements

We want to see your best work - don't let access to resources be a blocker.

---

## The Problem

You're given:

1. A **search prompt** representing what a user searched for
2. A collection of **web articles** that could appear as citations for that search

Your task is to build a system that extracts and analyzes **brand mentions relevant to the prompt's category**. The articles may contain:

- Brands directly relevant to the prompt (should be extracted)
- Brands mentioned incidentally but unrelated to the prompt's category (should be filtered out)
- No brand mentions at all (should be handled gracefully)

The system should answer questions like:

- Which brands are mentioned most frequently across sources?
- How do brands rank against each other in recommendations?
- What specific features are associated with each brand?
- What's the sentiment distribution for each brand?
- Which brands appear in "top pick" or "best overall" positions vs. "also consider" positions?

**Important**: Your solution should generalize. We will test it with different prompts and article sets to see if it correctly identifies the relevant brand category and filters appropriately.

---

## Input

The input is provided in the `input/` directory:

```
input/
├── prompt.txt              # The search query (e.g., "what are the best marathon shoes for women over 40")
└── articles/
    ├── article-001.json
    ├── article-002.json
    ├── ...
    └── article-023.json
```

### prompt.txt

Contains the search query that defines what brands are relevant. Your system should infer the brand category from this prompt (e.g., "running shoes" → athletic footwear brands).

### articles/

Individual JSON files - 23 articles of varying types. This structure allows you to easily create train/test splits for evaluation.

Article types include:

- **roundup** - Product lists ("10 Best Marathon Shoes for Women 2024")
- **review** - Single product reviews ("ASICS Gel-Nimbus 26 Review")
- **forum** - Discussion threads ("What shoes do you recommend for older runners?")
- **guide** - Expert advice ("How to Choose Running Shoes After 40")
- **comparison** - Head-to-head articles ("Brooks Ghost vs ASICS Gel-Nimbus: Which Is Better?")

Each article file contains:

```json
{
  "id": "article-001",
  "url": "https://example.com/...",
  "title": "Article Title",
  "source_type": "roundup|review|forum|guide|comparison",
  "content": "Full article text...",
  "published_at": "2024-01-15T10:00:00Z"
}
```

## Deliverables

### 1. Extraction System

Build an LLM-based system that processes the articles and extracts structured brand mention data. Your system should:

- Infer the relevant brand category from the prompt
- Extract brand mentions with relevant context
- Filter out brands/mentions not relevant to the prompt's category
- Normalize brand names (handle casing, abbreviations, model-only references)
- Identify rankings and recommendations (e.g., "top pick", "#3 choice", "runner-up")
- Extract feature evaluations (e.g., "excellent cushioning", "runs narrow")
- Determine sentiment and recommendation strength
- Handle the various article types appropriately

**Key consideration**: LLMs hallucinate. Your system needs to account for this - brands mentioned in the output should actually appear in the source text.

### 2. Evaluation Mechanism

Provide a repeatable way to measure the quality of your extraction. We don't prescribe a specific framework - choose an approach that gives you (and us) confidence that the extraction is working correctly. Be prepared to explain:

- How do you know the extraction is accurate?
- What's your false positive rate (hallucinated brands)?
- How do you measure relevance filtering (did it correctly ignore unrelated brands)?
- How would you catch regressions if you changed the prompt?

### 3. Analytical Output

The core deliverable: produce analytical outputs that answer real competitive intelligence questions. Design whatever storage and query approach makes sense, but the system must be able to generate insights such as:

**Brand Performance Summary**

- Total mention count per brand across all sources
- Average ranking position when brands appear in ranked lists
- Share of "top pick" / "best overall" designations

**Feature Analysis**

- Which are the leaders and laggers for each feature? (\*)
- Feature comparison matrix across top brands

Note: Do LLMs actually speak negatively about brands? If actual negative commentary was rare, how would you evaluate sentiment, and deterine a rank of brands in relation to a feature? ex. Nike better at comfort than Asics?

**Competitive Landscape**

- Head-to-head comparison frequency (which brands are compared against each other)
- Sentiment distribution per brand
- Source type breakdown (where does each brand get mentioned most)

## Technical Requirements

### Stack

- **TypeScript** (Node.js or Bun)
- **PostgreSQL** and/or **ClickHouse** for data storage
- **OpenAI API** (or compatible) for LLM extraction
- Any other libraries you find useful

### Architecture

We don't prescribe a specific architecture. Design your system to favor **correctness** and **maintainability**. Be prepared to explain:

- Why you structured the code the way you did
- What you'd change if this needed to process 10,000 articles daily
- How your system would handle a completely different prompt (e.g., "best espresso machines for home use")

### Running the System

Your submission should be runnable with minimal setup, in any system were a unix shell and docker are installed.

## Submission

1. Create a **GitHub repository** with your solution
2. Include a **README.md** with:
   - Setup and running instructions
   - Architecture overview and key design decisions
   - **Scoping decisions**: What you included, what you left out, and why
   - Known limitations and what you'd improve with more time

### How to Deliver

Choose one of the following options:

1. **GitHub Repository (preferred)**: Create a GitHub repository with your solution and give collaborator access to [@miguelff](https://github.com/miguelff)

2. **Email**: Send the complete git repository as a tar file to **miguel@useomnia.com**

---

## Interview Discussion Topics

Be prepared to discuss:

1. **Scoping Decisions**: Walk us through how you decided what to build and what to skip. How did you evaluate the cost/value of each requirement?

2. **Extraction Design**: Why did you structure your extraction prompt that way? How did you handle different article types?

3. **Hallucination Handling**: How confident are you that extracted brands actually appear in the source? Show us.

4. **Analytical Outputs**: Which insights are most valuable? What would you add with more time?

5. **AI Assistant Usage**: Walk us through a specific part where you used Claude or an alternative coding agent. What did it get right/wrong? What did you have to fix?

6. **Trade-offs**: What did you optimize for? What did you sacrifice?

---

## Evaluation Criteria

| Criteria                     | What We're Looking For                                                                                     |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Scoping & Prioritization** | Smart decisions about what to build vs. skip given the time constraint; clear reasoning                    |
| **Extraction Quality**       | Accurate brand extraction with minimal hallucinations, appropriate handling of rankings/features/sentiment |
| **Analytical Value**         | Outputs that answer real questions, well-organized and interpretable results                               |
| **Code Quality**             | Correctness, clarity, efficiency                                                                           |
| **Design Decisions**         | Thoughtful trade-offs, appropriate complexity for the problem                                              |

---

Good luck! We're excited to see your approach.
