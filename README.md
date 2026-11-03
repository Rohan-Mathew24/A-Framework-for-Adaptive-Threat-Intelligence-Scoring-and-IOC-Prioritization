# A-Framework-for-Adaptive-Threat-Intelligence-Scoring-and-IOC-Prioritization
One of the major problems that SOCs have encountered is the problem of separating high confidence and operational threat indicators from stale and poor quality threat intelligence. 
# Adaptive Threat Intelligence Scoring and IOC Prioritization

## Project Overview

This project presents an end-to-end automated Cyber Threat Intelligence (CTI) framework for collecting, enriching, scoring, prioritizing, and monitoring Indicators of Compromise (IOCs).

The framework integrates multiple threat intelligence sources with an adaptive scoring mechanism to evaluate IOC risk and confidence. It incorporates operational decay, runtime event correlation, case management, Security Orchestration, Automation and Response (SOAR), and analyst feedback to continuously reassess threat intelligence.

The objective is to improve IOC prioritization, reduce reliance on manual threat intelligence processing, and provide security analysts with actionable and traceable threat information.

## Key Features

- **Multi-Source IOC Collection:** Collects threat intelligence from MISP and AlienVault OTX.
- **IOC Normalization and Deduplication:** Standardizes IOC records and removes duplicates while preserving metadata.
- **Contextual Enrichment:** Enriches indicators using available external threat intelligence services.
- **Adaptive Risk Scoring:** Calculates IOC Risk Scores using source reliability, corroboration, enrichment, freshness, and operational evidence.
- **Independent Confidence Scoring:** Evaluates the reliability of supporting intelligence separately from risk.
- **Operational Decay:** Reduces IOC relevance over time according to indicator-specific decay policies.
- **Tiered Watchlist Management:** Classifies indicators into Block, Monitor, Greylist, and Archive categories.
- **Runtime IOC Detection:** Correlates simulated network, DNS, and file events against prioritized IOC watchlists.
- **Case Management:** Creates investigation cases and observables for relevant detections.
- **SOAR Integration:** Coordinates analyst approval and simulated response actions.
- **Feedback-Driven Reassessment:** Updates IOC scores using local sightings and analyst feedback.
- **Security Dashboards:** Provides operational visibility into IOC distribution, risk, confidence, and threat intelligence activity.

## System Architecture

The framework follows an integrated processing workflow:

1. Threat intelligence collection from MISP and OTX.
2. IOC extraction, normalization, and deduplication.
3. Contextual enrichment using external intelligence services.
4. Risk and Confidence Score calculation.
5. Operational decay and IOC classification.
6. Publication of prioritized watchlists to OpenSearch.
7. Runtime event ingestion and IOC correlation.
8. Local sighting evidence updates.
9. TheHive case and observable creation.
10. StackStorm SOAR workflow and analyst approval.
11. Feedback processing and risk reassessment.
12. Updated watchlist publication and dashboard visualization.

## Scoring Methodology

The framework uses an adaptive Risk Score:

`R = clip(0,100)[S + C + E + A + L - N]`

Where:

- **S:** Source Reliability
- **C:** Corroboration
- **E:** Enrichment
- **A:** Age Freshness
- **L:** Local Sighting Evidence
- **N:** Negative Evidence

The Confidence Score is calculated independently using source, corroboration, and enrichment confidence components.

Runtime observations increase Local Sighting Evidence, while benign analyst decisions contribute Negative Evidence. Malicious analyst decisions are recorded without incrementing local sightings again.

## Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python |
| Threat Intelligence | MISP, AlienVault OTX |
| Search and SIEM | OpenSearch |
| Case Management | TheHive |
| SOAR | StackStorm |
| Containerization | Docker and Docker Compose |
| Data Processing | Python, JSON |
| Caching | Redis |
| Visualization | OpenSearch Dashboards |
| API Communication | REST APIs |

## Project Structure

The repository contains the threat intelligence processing scripts, runtime detection workers, integration modules, and supporting configuration files.

```text
Adaptive-Threat-Intelligence/
│
├── ETL/
│   ├── misp_pull_1.py
│   ├── extract_otx_iocs_2.py
│   ├── merge_iocs_3.py
│   ├── deduplicate_4.py
│   ├── enrich_ioc_5.py
│   ├── score_ioc6.py
│   ├── update_ioc_database7.py
│   ├── decay_score_8.py
│   ├── load_opensearch_9.py
│   ├── load_runtime11.py
│   ├── alert_worker12.py
│   ├── hive_13.py
│   ├── feedback_worker_14.py
│   ├── run_pipeline1.py
│   ├── runtime_logs.json
│   └── test/
│
├── StackStorm/
│   └── ti_soc/
│
├── Documentation/
│
├── Screenshots/
│
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

*The directory structure above is a suggested repository organization. Adjust it to match the actual files uploaded.*

## Installation and Configuration

### Prerequisites

- Ubuntu Linux
- Python 3
- Docker and Docker Compose
- MISP
- OpenSearch and OpenSearch Dashboards
- TheHive
- StackStorm
- Redis

### Environment Configuration

Configure the required API credentials through environment variables.

```bash
export THEHIVE_API_KEY="your_thehive_api_key"
export STACKSTORM_API_KEY="your_stackstorm_api_key"
```

Additional API credentials for threat intelligence sources should also be configured through environment variables.

**Important:** Never upload API keys, passwords, `.env` files, or sensitive operational data to a public GitHub repository.

### Running the Pipeline

Navigate to the ETL directory:

```bash
cd ETL
```

Run the initial threat intelligence processing pipeline:

```bash
python run_pipeline1.py
```

The pipeline performs IOC collection, processing, enrichment, scoring, decay, and OpenSearch publication.

Runtime processing and integration workers can then be executed according to the configured workflow:

```bash
python load_runtime11.py
python alert_worker12.py
python hive_13.py
python feedback_worker_14.py
```

StackStorm handles the SOAR workflow and analyst approval separately.

## Testing and Validation

The framework is validated through functional and integration testing, including:

- IOC extraction and normalization.
- Deduplication and enrichment.
- Risk and confidence calculations.
- Operational decay.
- Runtime IOC detection.
- Local sighting evidence updates.
- TheHive case and observable creation.
- StackStorm analyst approval.
- Simulated response actions.
- Negative evidence and feedback processing.
- OpenSearch watchlist classification and visualization.

Runtime events and response actions are simulated for controlled testing. The framework does not perform actual firewall blocking.

## Security Considerations

- API credentials must be stored outside source code.
- Sensitive configuration files and generated operational data should not be publicly committed.
- Runtime testing uses simulated events.
- Response actions are simulated and do not modify firewall rules.
- External API availability and rate limits may affect enrichment operations.

## Project Scope

This project demonstrates an integrated threat intelligence lifecycle, from IOC collection and enrichment to runtime detection, investigation, analyst validation, and adaptive risk reassessment.

It is developed as an academic cybersecurity capstone project.

## Future Enhancements

- Integration with additional threat intelligence sources.
- Evaluation using larger real-world security datasets.
- Deployment with enterprise-scale SIEM environments.
- Advanced analytical techniques for threat prioritization.
- Controlled integration with production security enforcement systems.

## Acknowledgements

This project uses open-source cybersecurity technologies and threat intelligence platforms, including MISP, OpenSearch, TheHive, and StackStorm.

## Author

**Rohan Mathew**

MSc Cybersecurity

REVA Academy for Corporate Excellence

## Disclaimer

This project is intended for academic research, cybersecurity learning, and controlled testing. Runtime events and response actions are simulated. Users are responsible for configuring integrations securely and obtaining appropriate authorization before testing in operational environments.
