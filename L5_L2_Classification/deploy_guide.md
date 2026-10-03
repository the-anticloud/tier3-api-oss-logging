# Deploy Guide — api-oss-logging
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Python 3.11, structlog, SQLite, AIOSS_FORMAT

## Prerequisites
Python 3.11+. See stack: Python 3.11, structlog, SQLite, AIOSS_FORMAT. AIOSS_FORMAT required. PAX 27B weights for AI-assisted features.

## AIOSS Integration
```bash
aioss init --module api-oss-logging --output ./api_oss_logging.aioss
aioss append --chain ./api_oss_logging.aioss --payload ./output.bin --module api-oss-logging
aioss verify --chain ./api_oss_logging.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="api-oss-logging",
    aioss_chain="./api_oss_logging.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./api_oss_logging.aioss --verbose
python -m api_oss_logging.tests.smoke
```
