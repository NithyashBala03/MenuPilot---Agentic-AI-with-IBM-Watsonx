# MenuPilot-Agentic-AI-with-IBM-Watsonx
Agentic Menu Intelligence built with IBM watsonx Orchestrate (Dev Day: AI Demystified Hackathon 2026)

MenuPilot converts restaurant menu exports into a **clear weekly action plan** by combining:
- **Orchestration** (intent routing and repeatable workflow)
- **Deterministic computation** (computed KPIs + flags)
- **LLM reasoning** (structured, actionable recommendations)

> Outcome: faster, data-backed decisions on pricing, profitability, and customer experience, without spending hours in spreadsheets.

---
<img width="776" height="852" alt="image" src="https://github.com/user-attachments/assets/66302fce-002f-4e4e-87b4-bf43a78e0f96" />

##  The Scenario (Problem)
Restaurants often have sales and cost data, but weekly decisions are still manual:
- High food-cost items quietly reduce margins
- Low-rated or complaint-heavy items keep getting sold
- Prep-time bottlenecks hurt service quality
- “Static reports” show numbers but do not tell teams **what to do next**

MenuPilot solves this by producing a **Top 5 Weekly Action Plan** grounded in computed KPIs, quadrant classification (Stars, Plowhorses, Puzzles, Dogs), and operational flags.

---

##  What MenuPilot Delivers
### Output A: Menu Performance Table (structured)
A standardized table per item with computed insights:
- `food_cost_pct`, `margin`
- `popularity`, `profit`, `menu_quadrant`
- `cost_warning`, `slow_prep_flag`, `low_rating_flag`, `complaints_flag`

### Output B: Weekly Action Plan (prioritized)
A ranked list of **top 5 actions** for the week:
- What to fix first
- What to price-adjust
- What to promote
- What to remove
- Expected impact and rationale

---

##  Why This Is Agentic AI
MenuPilot is not “one prompt.” It behaves like an agentic workflow:

1. **Plan / Route**  
   A Branch/Condition step routes user intent into one of 3 paths:
   - Weekly Plan
   - Quick Summary
   - Deep Diagnostics

2. **Act (Tool Step)**  
   A deterministic compute step enriches the dataset by generating derived columns:
   - Food cost %, margin
   - Quadrant classification
   - Rule flags for cost, prep-time, ratings, complaints

3. **Reason + Recommend (LLM Step)**  
   The LLM consumes the **computed** fields to generate grounded recommendations in a structured format.

---

##  Architecture
**Built in**: **IBM watsonx Orchestrate Flow Builder**

**Flow pattern**
1. **User Activity**: collect request (goal + optional focus)
2. **Branch**: route request type (weekly vs summary vs diagnostics)
3. **Compute Step**: generate KPIs and flags (code-fed)
4. **Generative Prompt**: produce Output A + Output B
5. **Output**: final response shown to user

---
<img width="1327" height="687" alt="image" src="https://github.com/user-attachments/assets/f35c432e-f440-49ff-80fb-607cfb42f9a2" />

## Dataset (Snapshot + Cleaning)

### Source
This project uses a **snapshot** taken from the **Food.com Recipes and Interactions** dataset on Kaggle (file: `RAW_interactions.csv`). The original dataset contains large-scale user–item interaction logs (e.g., user ratings and interaction metadata) intended for food/recipe analytics. 

### What we did (from raw → MenuPilot-ready)
To make the data usable for a restaurant “menu operations” scenario, we cleaned and transformed the snapshot into **two structured CSVs** that are optimized for agentic workflows:

1) **`menupilot.csv` (Detailed, item-level diagnostics)**
   - Contains item-level fields and enriched signals used for deep analysis and action planning.
   - Includes derived columns such as:
     - `food_cost_pct`, `margin`
     - `popularity`, `profit`, `menu_quadrant` (Stars/Plowhorses/Puzzles/Dogs)
     - rule flags (e.g., `cost_warning`, `slow_prep_flag`, `low_rating_flag`, `complaints_flag`)

2) **`menu_export_system.csv` (Pre-aggregated, fast summaries)**
   - A smaller, pre-aggregated version used for quick insights and ranking outputs.
   - Includes fields like:
     - `item_name`, `units_sold`, `menu_price`, `ingredient_cost`, `margin`
     - `popularity`, `profit`, `menu_quadrant`

### Why two datasets?
- **`menupilot.csv`** supports deep diagnostics and evidence-based recommendations (ratings, complaints, prep-time/flags).
- **`menu_export_system.csv`** supports quick executive summaries (quadrant counts, top/bottom rankings) without extra detail.

### Notes on responsible use
- Only cleaned, structured outputs are used in the demo flow.
- The workflow is designed to avoid exposing sensitive data and to operate on non-confidential datasets.


---

##  Demo Prompts
Use these in your flow run/test to trigger the best outputs:

weekly plan; 
focus: reduce food cost + complaints. 
Output top 5 actions with priority_rank, item_name, issue_detected, recommended_action, expected_impact.

## Limitations (Honest + Professional)
- Outputs depend on the quality of the export data.
- Thresholds (food cost %, rating cutoffs) should be tuned per restaurant concept.
- In Flow-only access environments, “agent” behavior is implemented as an orchestrated workflow rather than a standalone agent object.

---

## Future Improvements
- Connect to POS exports automatically (scheduled weekly run)
- Add price elasticity simulation (what-if pricing changes)
- Push actions into task tools (owner, due date, status)
- Add role-specific views (GM vs kitchen vs finance)
- Add change tracking month-over-month (trend detection)

---

##  Demo Video
Add your public demo link here:
- **Demo:** https://www.canva.com/design/DAHADWnZRuE/UHJtv3GLZ6nUamTA4ZGpJg/watch?utm_content=DAHADWnZRuE&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=h978a14bc29
