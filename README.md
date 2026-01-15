# Backend & AI Engineering Challenge

## Overview

You will build an **AI-powered Brand Mention Analyzer** that processes web content to extract competitive intelligence about brand mentions, rankings, and feature evaluations.

This challenge tests skills critical to our work: designing AI extraction workflows, handling LLM limitations, data modeling for analytics, and building systems that produce actionable insights.

**Time Expectation**: 4-6 hours of focused work

**You may use AI coding assistants** (Claude, Copilot, etc.). We encourage it - we want to see how you work with these tools. Be prepared to explain your prompts, decisions, and any manual corrections you made.

---

## The Problem

You're given a dataset of 20 web articles that could appear as citations when someone searches **"what are the best marathon shoes for women over 40"**. These articles include product reviews, comparison guides, forum discussions, and expert recommendations.

Your task is to build a system that extracts and analyzes brand mentions from these articles to answer questions like:

- Which brands are mentioned most frequently across sources?
- How do brands rank against each other in recommendations?
- What specific features (cushioning, stability, price, durability) are associated with each brand?
- What's the sentiment distribution for each brand?
- Which brands appear in "top pick" or "best overall" positions vs. "also consider" positions?

---

## Input

The dataset is provided in `sample_data/articles/` as individual JSON files - 20 articles of varying types:

```
sample_data/
└── articles/
    ├── article-001.json
    ├── article-002.json
    ├── ...
    └── article-020.json
```

This structure allows you to easily create train/test splits for evaluation by selecting different subsets of files.

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

**Note**: One article (article-016) contains no brand mentions - only generic feature discussions. Your extraction should handle this gracefully.

---

## Deliverables

### 1. Extraction System

Build an LLM-based system that processes the articles and extracts structured brand mention data. Your system should:

- Extract brand mentions with relevant context
- Identify rankings and recommendations (e.g., "top pick", "#3 choice", "runner-up")
- Extract feature evaluations (e.g., "excellent cushioning", "runs narrow")
- Determine sentiment and recommendation strength
- Handle the various article types appropriately

**Key consideration**: LLMs hallucinate. Your system needs to account for this - brands mentioned in the output should actually appear in the source text.

### 2. Evaluation Mechanism

Provide a repeatable way to measure the quality of your extraction. We don't prescribe a specific framework - choose an approach that gives you (and us) confidence that the extraction is working correctly. Be prepared to explain:

- How do you know the extraction is accurate?
- What's your false positive rate (hallucinated brands)?
- How would you catch regressions if you changed the prompt?

### 3. Analytical Output

The core deliverable: produce analytical outputs that answer real competitive intelligence questions. Design whatever storage and query approach makes sense, but the system must be able to generate insights such as:

**Brand Performance Summary**

- Total mention count per brand across all sources
- Average ranking position when brands appear in ranked lists
- Share of "top pick" / "best overall" designations

**Feature Analysis**

- Which features are most associated with each brand (positive and negative)
- Feature comparison matrix across top brands

**Competitive Landscape**

- Head-to-head comparison frequency (which brands are compared against each other)
- Sentiment distribution per brand
- Source type breakdown (where does each brand get mentioned most)

**Example output format** (you may choose a different structure):

```
=== Brand Performance Report ===

Top 5 Brands by Mention Volume:
1. Brooks (34 mentions across 28 articles)
   - Avg ranking: 2.1 | Top picks: 8 | Sentiment: 78% positive
   - Top features: cushioning (12), comfort (9), durability (7)

2. ASICS (31 mentions across 25 articles)
   - Avg ranking: 2.4 | Top picks: 6 | Sentiment: 72% positive
   - Top features: gel cushioning (11), stability (8), wide options (6)

...

=== Feature Comparison Matrix ===
                 Cushioning  Stability  Lightweight  Durability  Price Value
Brooks Ghost        +++         ++          +           ++           ++
ASICS Nimbus        +++        +++          -           ++            +
Nike Pegasus         ++          +         +++           +           ++
...

=== Head-to-Head Comparisons ===
Brooks vs ASICS: 12 direct comparisons (Brooks preferred 7x)
Brooks vs Nike: 8 direct comparisons (Brooks preferred 5x)
...
```

---

## Technical Requirements

### Stack

- **TypeScript** (Node.js or Bun)
- **PostgreSQL** and/or **ClickHouse** for data storage
- **OpenAI API** (or compatible) for LLM extraction
- Any other libraries you find useful

### Architecture

We don't prescribe a specific architecture. Design your system to favor **correctness** and **maintainability**. Be prepared to explain:

- Why you structured the code the way you did
- How you'd onboard another engineer to work on this
- What you'd change if this needed to process 10,000 articles daily

### Running the System

Your submission should be runnable with minimal setup:

```bash
docker-compose up -d  # Start databases
npm install
npm run extract       # Process articles
npm run report        # Generate analytical output
```

---

## Submission

1. Create a **GitHub repository** with your solution
2. Include a **README.md** with:
   - Setup and running instructions
   - Architecture overview and key design decisions
   - Known limitations and what you'd improve with more time
3. Include a **PROMPTS.md** documenting:
   - Key prompts you used with AI assistants during development
   - Your extraction prompt(s) with rationale for design choices
   - What worked well, what required manual fixes

### How to Deliver

Choose one of the following options:

1. **GitHub Repository (preferred)**: Create a GitHub repository with your solution and give collaborator access to [@miguelff](https://github.com/miguelff)

2. **Email**: Send the complete git repository as a tar file to **miguel@useomnia.com**

---

## Interview Discussion Topics

Be prepared to discuss:

1. **Extraction Design**: Why did you structure your extraction prompt that way? How did you handle different article types?

2. **Hallucination Handling**: How confident are you that extracted brands actually appear in the source? Show us.

3. **Data Model**: Why did you choose this storage approach? How would it handle 10x or 100x more data?

4. **Evaluation Approach**: Walk us through how you validated extraction quality. What would a more comprehensive evaluation look like?

5. **Analytical Outputs**: Which insights are most valuable? What additional analysis would be useful for someone making competitive intelligence decisions?

6. **AI Assistant Usage**: Walk us through a specific part where you used Claude/Copilot. What did it get right/wrong? What did you have to fix?

7. **Trade-offs**: What did you optimize for? What did you sacrifice? What would you do differently with more time?

---

## Evaluation Criteria

| Criteria                | What We're Looking For                                                                                     |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Extraction Quality**  | Accurate brand extraction with minimal hallucinations, appropriate handling of rankings/features/sentiment |
| **Analytical Value**    | Outputs that answer real questions, well-organized and interpretable results                               |
| **Code Quality**        | Clean, maintainable code that another engineer could understand and extend                                 |
| **Evaluation Approach** | Confidence that the system works correctly, ability to detect regressions                                  |
| **Design Decisions**    | Thoughtful trade-offs, appropriate complexity for the problem                                              |

---

## Appendix: Docker Compose

```yaml
version: "3.8"
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: brand_analyzer
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: dev
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  clickhouse:
    image: clickhouse/clickhouse-server:24.1
    ports:
      - "8123:8123"
      - "9000:9000"
    volumes:
      - clickhouse_data:/var/lib/clickhouse

volumes:
  postgres_data:
  clickhouse_data:
```

---

Good luck! We're excited to see your approach.
