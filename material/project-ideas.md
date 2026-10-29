:
# Project Ideas

---

### Project 1: Smart Home / IoT Assistant

**Theme:** Device monitoring, room comfort, and energy awareness.

**Example Interaction:** *“The living room is cold. Check the temperature, compare it with my preferred setting, and prepare a suitable thermostat adjustment.”*

**From Assistant to Agentic Workflow:** The assistant reads and explains sensor measurements. The agent identifies the target device, inspects states and preferences, prepares a change, validates constraints, and requests confirmation before performing any action.

**Tools:** `list_devices`, `get_temperature`, `get_device_state`, `get_room_preferences`, `get_energy_usage`, `set_thermostat`, `log_action`.

**Database:** SQLite tables for `rooms`, `devices`, `sensor_readings`, `preferences`, and `action_log`. Use room-device mappings so the model can resolve room names without hallucinating hardware IDs.

**API / Integration:** A mock smart-home REST service or the [Home Assistant REST API](https://developers.home-assistant.io/docs/api/rest/). Simulated readings are sufficient; no real home hardware is required.

- **Sprint 1 (Notebook):** Implement a notebook workflow querying SQLite for device metadata and your API for live/simulated temperatures. Demonstrate *“check a room, compare values against preferences, and propose an adjustment”* in **both Groq and local Colab modes**. Display tool execution traces; do not automatically alter device state.
- **Sprint 2 (Streamlit & MCP/RAG):** Build an interactive Streamlit dashboard and chat UI. Expose device tools via MCP. Add RAG over thermostat user manuals, hardware troubleshooting guides, and energy-use policies. Answer queries combining live device data with retrieved manual instructions.
- **Sprint 3 (Agents & Fine-Tuning):** Introduce distinct monitoring, planning, and validation agent roles. Test disconnected sensors, stale readings, unknown devices, and safety limits. Fine-tune a classification model for user intent (*monitor*, *explain*, or *control*).

**Final Outcome:** A device-aware assistant that grounds recommendations in real telemetry and safely prepares authorized actions.

---

### Project 2: University Course Registration Assistant

**Theme:** Academic course discovery, eligibility checking, and timetable planning.

**Example Interaction:** *“Find eligible AI courses with available seats and no timetable conflicts.”*

**From Assistant to Agentic Workflow:** The assistant searches course descriptions. The agent checks student history, prerequisites, seat limits, and timetable clashes to assemble a feasible schedule draft.

**Tools:** `search_courses`, `get_course_details`, `check_prerequisites`, `check_available_seats`, `check_schedule_conflicts`, `prepare_registration`.

**Database:** SQLite tables for `courses`, `sections`, `prerequisites`, `completed_courses`, `student_plans`, and `registration_drafts` with synthetic student data.

**API / Integration:** A public university catalog API or a mock course-availability REST API.

- **Sprint 1 (Notebook):** Build a notebook search and eligibility pipeline. Use SQLite for the catalog/prerequisites and your API for current seat counts. Run a multi-step eligibility check in **both Groq and local Colab modes**, displaying the tool selection and validation traces.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit web interface for course exploration and plan generation. Expose course and registration tools over MCP. Add RAG over degree requirements and academic regulations to cite specific policy rules alongside recommendations.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate course-discovery, eligibility, and timetable-validation agents. Fine-tune extraction of course codes and timing constraints from conversational inputs. Test missing prerequisites, full classes, and schedule conflicts.

**Final Outcome:** A grounded academic planning assistant that verifies university constraints and drafts viable course schedules.

---

### Project 3: Inventory and Supply Assistant

**Theme:** Stock management, supplier comparisons, and purchase-order drafting.

**Example Interaction:** *“Find products below the reorder threshold and prepare a supplier order.”*

**From Assistant to Agentic Workflow:** The assistant lists stock counts. The agent calculates reorder quantities deterministically, compares supplier pricing, applies purchasing rules, and drafts an order.

**Tools:** `check_stock`, `get_stock_movements`, `calculate_reorder`, `find_supplier`, `compare_supplier_prices`, `draft_purchase_order`.

**Database:** SQLite tables for `products`, `stock_movements`, `reorder_rules`, `suppliers`, `supplier_prices`, and `purchase_order_drafts`. Calculate quantities and totals in Python rather than relying on LLM arithmetic.

**API / Integration:** A mock supplier pricing/availability REST API. (Public product databases like Open Food Facts can supply product metadata, but live stock must come from your database/mock API).

- **Sprint 1 (Notebook):** Implement a notebook pipeline combining SQLite stock checks with an external supplier API lookup. Run shortage queries in **both Groq and local Colab modes**, displaying deterministic reorder calculations and tool traces.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit procurement dashboard. Expose inventory and pricing tools through MCP. Add RAG over procurement guidelines, supplier contracts, and shipping policies to justify supplier selection.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate inventory-analysis, supplier-selection, and order-validation agents. Fine-tune product-category mapping or entity extraction from invoices. Test out-of-stock items, budget caps, and missing price data.

**Final Outcome:** An auditable inventory assistant that spots low inventory and drafts compliant purchase orders.

---

### Project 4: Coding and Debugging Assistant

**Theme:** Codebase inspection, failed-test diagnosis, and controlled patch generation.

**Example Interaction:** *“Explain this failed test and propose a patch that can be retested.”*

**From Assistant to Agentic Workflow:** The assistant explains a stack trace. The agent inspects files, retrieves test logs, drafts a targeted patch, applies it inside an isolated sandbox, runs tests, and reviews the outcome.

**Tools:** `list_files`, `read_file`, `search_code`, `get_test_results`, `run_tests`, `get_error_history`, `propose_patch`.

**Database:** SQLite tables for `repositories`, `test_runs`, `error_records`, `patch_proposals`, and `test_outcomes`.

**API / Integration:** A small local repository with an approved, sandboxed `pytest` runner, or the GitHub REST API for issue metadata. *Do not allow arbitrary, unvalidated shell commands.*

- **Sprint 1 (Notebook):** Build a notebook tool runner that searches sample files, inspects recorded test failures, and suggests fixes. Run test diagnostics in **both Groq and local Colab modes** with read-only tools and detailed execution logging.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit code-investigation UI showing diffs and test statuses. Expose code inspection tools via MCP. Add RAG over project READMEs, architecture specs, and style guidelines to ensure patches align with repository rules.
- **Sprint 3 (Agents & Fine-Tuning):** Implement code-analysis, patch-drafting, and verification agents. Test patches strictly in temporary copies. Fine-tune bug classification or patch formatting. Evaluate test regressions and security boundaries.

**Final Outcome:** A debugging assistant that investigates failures and proposes safe, verifiable code corrections.

---

### Project 5: Public Transport Assistant

**Theme:** Journey planning, timetable checks, and transfer/accessibility constraints.

**Example Interaction:** *“Find an arrival before 09:00 with no more than one transfer.”*

**From Assistant to Agentic Workflow:** The assistant lists routes. The agent validates stop IDs, queries live timetables, enforces time/transfer limits, compares alternatives, and proposes the most reliable route.

**Tools:** `find_stop`, `get_departures`, `plan_journey`, `get_service_alerts`, `compare_journeys`, `save_journey`.

**Database:** SQLite tables for `stops`, `favorite_locations`, `saved_journeys`, `journey_searches`, and `timetable_snapshots` (include timestamps).

**API / Integration:** A regional transport API (such as Digitransit) or a mock routing API. Journey durations must come directly from tool outputs.

- **Sprint 1 (Notebook):** Build an interactive notebook journey planner. Resolve stops, query routes via API, filter by constraints, and format itinerary recommendations in **both Groq and local Colab modes**.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit transit application with search filters and map/route summaries. Expose transit tools via MCP. Add RAG over ticketing rules, accessibility policies, and delay refund terms to explain journey guidelines.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate routing, constraint-checking, and alert-monitoring agents. Fine-tune departure/destination and time entity extraction. Test canceled departures, impossible schedules, and stale timetable data.

**Final Outcome:** A transport assistant that generates realistic, constraint-checked travel recommendations backed by transit data.

---

### Project 6: Personal Finance and Budget Assistant

**Theme:** Expense categorization and budget tracking using **synthetic data**.

**Example Interaction:** *“Compare this month's spending against my category budgets.”*

**From Assistant to Agentic Workflow:** The assistant lists transactions. The agent categorizes items, calculates totals with Python logic, flags over-budget categories, and prepares adjustments.

**Tools:** `get_transactions`, `categorize_expense`, `calculate_spending`, `get_budget`, `compare_budget`, `draft_budget_adjustment`.

**Database:** SQLite tables for `transactions`, `categories`, `monthly_budgets`, `merchants`, and `budget_proposals` using fictional data.

**API / Integration:** A mock banking/transaction REST API, optionally paired with a public exchange-rate API. *Do not use real banking credentials or real financial data.*

- **Sprint 1 (Notebook):** Implement a notebook budget analyzer. Run transaction aggregations and comparisons against limits in **both Groq and local Colab modes**, verifying all numbers with deterministic Python calculations.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit personal finance dashboard with budget charts and natural-language query tools. Expose financial tools via MCP. Add RAG over spending policies and budgeting guides to back up suggestions.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate transaction-categorization, spending-analysis, and budget-planning agents. Fine-tune merchant categorization. Test missing records, ambiguous transaction descriptions, and currency differences.

**Final Outcome:** A privacy-compliant financial assistant that explains spending patterns using auditable arithmetic and synthetic records.

---

### Project 7: Restaurant Ordering Assistant

**Theme:** Menu search, dietary restrictions, and order drafting.

**Example Interaction:** *“Prepare a vegetarian pickup order within a €20 budget.”*

**From Assistant to Agentic Workflow:** The assistant looks up dishes and prices. The agent verifies ingredients, dietary labels, item availability, and budget limits, then compiles a draft order for confirmation.

**Tools:** `search_menu`, `get_ingredients`, `check_availability`, `filter_dietary_options`, `calculate_total`, `draft_order`.

**Database:** SQLite tables for `menu_items`, `ingredients`, `dietary_labels`, `daily_availability`, `order_items`, and `draft_orders`.

**API / Integration:** A mock restaurant inventory and ordering REST API. Never execute live purchases. Dietary and allergen validations must depend strictly on tool data, not LLM guesses.

- **Sprint 1 (Notebook):** Create a notebook ordering assistant. Filter menu items by dietary tags, check live availability via API, and compute prices deterministically. Test the scenario in **both Groq and local Colab modes**.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit ordering application with interactive cart management. Expose ordering tools via MCP. Add RAG over allergen warnings, kitchen preparation policies, and restaurant FAQs.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate menu-discovery, allergen-validation, and order-assembly agents. Fine-tune structured order extraction (quantities, notes, pickup times). Test out-of-stock items, budget overruns, and conflicting allergen tags.

**Final Outcome:** A constraint-aware ordering assistant that creates verified order drafts without making unauthorized orders.

---

### Project 8: IT Help Desk Assistant

**Theme:** Technical issue diagnosis, troubleshooting, and ticket management.

**Example Interaction:** *“Investigate a Wi-Fi connection issue and create a support ticket if it cannot be resolved.”*

**From Assistant to Agentic Workflow:** The assistant looks up knowledge base articles. The agent classifies the problem, queries known incidents, runs diagnostic checks, proposes troubleshooting steps, and drafts a ticket upon user approval.

**Tools:** `search_known_issues`, `get_device_information`, `run_diagnostic`, `find_similar_tickets`, `create_ticket`, `check_ticket_status`.

**Database:** SQLite tables for `tickets`, `issue_categories`, `diagnostic_results`, `known_issues`, `device_types`, and `resolution_history`.

**API / Integration:** A mock help-desk service or a test GitHub Issues repository. Diagnostics must use a safe whitelist; never execute arbitrary system commands.

- **Sprint 1 (Notebook):** Create a notebook troubleshooting assistant. Perform issue lookups and mock diagnostics, displaying tool traces and triage steps in **both Groq and local Colab modes**.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit IT portal for submitting and tracking support requests. Expose diagnostic and ticketing tools via MCP. Add RAG over network setup manuals, hardware troubleshooting docs, and escalation policies.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate triage, troubleshooting, and ticket-drafting agents. Fine-tune urgency and category classification. Test duplicate ticket prevention, non-responsive diagnostics, and privilege boundaries.

**Final Outcome:** An IT support assistant that guides troubleshooting with documented procedures and generates traceable ticket drafts.

---

### Project 9: Environmental Monitoring Assistant

**Theme:** Sensor telemetry, air quality tracking, and threshold alerts.

**Example Interaction:** *“Compare station readings across the city and flag any values exceeding safety limits.”*

**From Assistant to Agentic Workflow:** The assistant reports current sensor values. The agent collects multi-station data, checks units and thresholds, verifies alert rules, and prepares an alert report.

**Tools:** `list_stations`, `get_sensor_readings`, `get_air_quality`, `compare_stations`, `check_thresholds`, `create_alert`.

**Database:** SQLite tables for `stations`, `sensor_measurements`, `threshold_rules`, `alerts`, and `measurement_history` (with timestamps and units).

**API / Integration:** The [Open-Meteo Air Quality API](https://open-meteo.com/en/docs/air-quality-api) or a mock sensor REST API. Distinguish observed data from forecasts.

- **Sprint 1 (Notebook):** Build a notebook monitoring pipeline. Pull sensor records from SQLite, fetch live air-quality metrics from your API, and evaluate threshold violations using Python checks in **both Groq and local Colab modes**.
- **Sprint 2 (Streamlit & MCP/RAG):** Build a Streamlit environmental dashboard with interactive station cards and alert feeds. Expose station and sensor tools through MCP. Add RAG over environmental standards, pollutant health guides, and municipal sensor guidelines.
- **Sprint 3 (Agents & Fine-Tuning):** Coordinate data-collection, anomaly-detection, and alert-formatting agents. Fine-tune extraction of pollutant types, target locations, and measurement units. Test sensor outages, missing fields, and threshold edge cases.

**Final Outcome:** A monitoring assistant that highlights threshold violations, provides evidence-backed explanations, and prepares clear alert reports.