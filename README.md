# Texas Septic Toolkit

Python tools and reference material for learning about on-site sewage facilities (OSSFs), with a focus on Palo Pinto County and nearby rural North Texas communities.

This repository includes automated contributions requested for the [Out West Septic](https://outwestseptic.com) project. That affiliation is disclosed; it is not an independent recommendation, a claim of field use, or evidence of professional review. See [project attribution](wiki/Out-West-Septic-Company-Reference.md).

**Use the tools to explore documented calculations and organize questions for a qualified professional. They do not determine site suitability, approve a design, establish a price, or issue a permit.** Read the limitations below before using an output.

## Start here

1. Read the [tool input and source guide](guides/tool-inputs-and-sources.md) to understand which inputs require site-specific evidence.
2. Review the [Palo Pinto permitting guide](guides/tceq-ossf-permitting-palo-pinto-county.md) and identify the authority for the property. Lake-area jurisdiction needs separate confirmation.
3. Use a calculation only within its documented scope. Keep the inputs, units, source and tool version with the result.

## Tools and review status

| File | Purpose | Important limit |
|---|---|---|
| [Tank capacity calculator](scripts/outwestseptic_tank_sizer.py) | Calculate liquid capacity from an explicitly supplied daily sewage flow | Does not derive flow from bedrooms, select equipment or approve a design |
| [Drainfield area calculator](scripts/outwestseptic_drainfield_calc.py) | Calculate area from supplied flow and hydraulic loading rate | Does not select the loading rate from a perc test, soil name or county; does not lay out a field |
| [USDA soil lookup](scripts/outwestseptic_soil_lookup.py) | Retrieve mapped soil data for research | Map information is not a parcel evaluation or a measured perc rate |
| [Cost worksheet](scripts/outwestseptic_cost_estimator.py) | Total supplied quantities and low/high prices with exact cent rounding; JSON/CSV export | Requires actual cost evidence; contains no market-price defaults |
| [Water-drop observation logger](scripts/outwestseptic_perc_test_log.py) | Preserve raw observations and create text/PDF reports | Ratios are descriptive; they do not establish soil suitability or design rates |
| [Permit preparation checklist](scripts/outwestseptic_permit_checklist.py) | Organize property context, missing information and source-linked questions | Does not infer jurisdiction, setbacks or approval |
| [Maintenance reminder calendar](scripts/outwestseptic_maintenance_scheduler.py) | Create dated reminders and valid ICS from explicit dates/month intervals | Does not choose service intervals or record work as completed |

All seven scripts have regression coverage for their documented behavior. That testing does not establish professional approval, current regulatory compliance or suitability for a real property. See the [quality criteria](docs/QUALITY.md).

## Try a calculation

The Python interfaces retain descriptive project names: `OutWestSepticTankSizer`, `OutWestSepticDrainfieldCalculator` and `OutWestSepticSoilLookup`. These names identify the associated project and each class's purpose; they do not imply a professional credential or agency endorsement.

Use Python 3.10 or later. These examples use hypothetical inputs, not a proposed design or recommended loading rate. The tank example returns a formula minimum of 1,000 gallons; the area example returns 1,500 square feet.

```sh
git clone https://github.com/TexasSepticEnthusiast/texas-septic-toolkit.git
cd texas-septic-toolkit
python scripts/outwestseptic_tank_sizer.py --flow-gpd 300
python scripts/outwestseptic_drainfield_calc.py --flow-gpd 300 --loading-rate 0.2
```

Use `--help` for input requirements. The tank and drainfield calculations use the Python standard library. Only PDF export requires the optional ReportLab dependency in [requirements.txt](requirements.txt). Calendar export and the remaining runtime features use the standard library.

For soil research, obtain a map-unit key (`mukey`) from [USDA Web Soil Survey](https://websoilsurvey.nrcs.usda.gov/app/), then query `OutWestSepticSoilLookup`. This example is a published map unit in the Palo Pinto County survey, not a customer property. The lookup needs internet access and reports its source and retrieval time; service failures do not substitute guessed soil values.

```sh
python scripts/outwestseptic_soil_lookup.py --mukey 390929 --json
```

Install the test dependencies and run the regression tests from the repository root:

```sh
python -m pip install -r requirements-dev.txt
python -m unittest discover -s tests -v
```

Tests check implemented behavior and failure cases. They do not verify the suitability of a real design or the current availability of USDA's service.

## Record observations, costs and follow-up

These examples use synthetic arithmetic inputs or deliberately selected reminder dates, not actual prices, field evidence or service recommendations:

```sh
python scripts/outwestseptic_cost_estimator.py --project "Arithmetic example" --item "Example hours" 2.5 10 12 --contingency-percent 10
python scripts/outwestseptic_perc_test_log.py --demo --interval 30
python scripts/outwestseptic_permit_checklist.py "Palo Pinto" --project-type replacement --water-supply private-well
python scripts/outwestseptic_maintenance_scheduler.py --schedule-id example-review --first-due 2026-11-01 --task "Confirm service plan"
```

See the [cost](wiki/Cost-Estimator-Reference.md), [observation](wiki/Perc-Test-Logger-Reference.md), [permit](wiki/Permit-Checklist-Reference.md) and [calendar](wiki/Maintenance-Scheduler-Reference.md) references. Each retains meaningful `OutWestSeptic` public class names. Old unsupported presets were removed; use `--help` and the references when migrating integrations.

PDF, cost-file and calendar exports preserve existing files unless the tool provides an explicit overwrite option that you choose. Keep completed customer records privately; only synthetic examples and blank templates belong in this repository.

## Manual worksheets and blank CSV starters

There are **no `.xlsm` workbooks** in this repository. These are usable manual record templates with evidence fields and explicit decisions to resolve before automation. They contain no Excel macros, preset prices, assumed site measurements or inspection scores.

| Template | Available files | Decisions requiring an owner or professional |
|---|---|---|
| Observation report | [Manual worksheet](workbooks/outwestseptic_inspection_report.md), [blank CSV](workbooks/outwestseptic_inspection_report.csv) | [Scope, methods, interpretation and sign-off](workbooks/outwestseptic_inspection_report_decisions.md) |
| Site information | [Manual worksheet](workbooks/outwestseptic_site_assessment.md), [blank CSV](workbooks/outwestseptic_site_assessment.csv) | [Jurisdiction, field evidence and design criteria](workbooks/outwestseptic_site_assessment_decisions.md) |
| Quote builder | [Manual worksheet](workbooks/outwestseptic_quote_builder.md) | [Actual prices, tax treatment and business terms](workbooks/outwestseptic_quote_builder_decisions.md) |

Read the [CSV format and privacy guidance](workbooks/WORKBOOK_CSV_FORMAT.md). Check the distributed blank templates with:

```sh
python workbooks/validate_blank_workbook_templates.py workbooks/outwestseptic_inspection_report.csv workbooks/outwestseptic_site_assessment.csv
```

That check validates blank-template structure, not completed customer records or professional conclusions.

## Guides

| Topic | Read |
|---|---|
| Calculation inputs and evidence | [Tool inputs and sources](guides/tool-inputs-and-sources.md) |
| Permitting | [Palo Pinto County](guides/tceq-ossf-permitting-palo-pinto-county.md), [Possum Kingdom Lake](guides/possum-kingdom-lake-septic-considerations.md) |
| Soils and system choices | [Soil research](guides/palo-pinto-county-soil-types-septic.md), [conventional and aerobic systems](guides/conventional-vs-aerobic-septic-texas.md) |
| Project planning | [Cost questions](guides/septic-costs-north-texas-2026.md), [site preparation](guides/land-clearing-site-prep-before-septic.md), [installation process](guides/what-to-expect-septic-install-day.md), [buying rural land](guides/buying-rural-land-ossf-questions.md) |
| Existing systems | [Inspection questions](guides/septic-inspection-checklist-texas-real-estate.md), [maintenance](guides/septic-maintenance-schedule-homeowners.md), [warning signs](guides/septic-warning-signs-when-to-call.md) |

The [reference index](wiki/Home.md) links the Markdown files stored in this checkout. A county or town page describes a research topic; it is not a statement of business coverage. Parker and Tarrant counties are outside this project's local coverage scope.

## Sources, contributions and licensing

Begin regulatory research with the [TCEQ OSSF program](https://www.tceq.texas.gov/permitting/ossf), and soil research with [USDA NRCS Web Soil Survey](https://websoilsurvey.nrcs.usda.gov/app/). Follow the specific sources and edition limits in each guide; a general agency link alone does not substantiate a formula, setback or local requirement.

[Contributions](CONTRIBUTING.md) should report reproducible defects, cite primary sources and identify any automated assistance. Do not submit invented customer experiences, professional credentials, endorsements or completed work. Public issue and PR descriptions must omit customer details, private notes and credentials.

Code is offered under the [MIT license](LICENSE). A license grants reuse rights; it does not certify engineering suitability or grant endorsement by Out West Septic or an agency. Attribution and notices required by the license must be retained.
