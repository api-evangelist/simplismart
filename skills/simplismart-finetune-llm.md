---
name: Start and monitor an LLM/VLM fine-tuning job on Simplismart
description: Submit a training job with a dataset and config to Simplismart's Training Suite, then poll its status and metrics.
api: openapi/simplismart-llm-training-openapi.yml
operations: [createTrainingJob, listTrainingJobs, getTrainingJob, getTrainingMetrics]
---

# Fine-tune an LLM/VLM

Simplismart's Training Suite fine-tunes base models on your data.

## Steps

1. **Authenticate** with `Authorization: Bearer <SIMPLISMART_API_KEY>`.
2. **Start the job** (`createTrainingJob`). POST to `/job/` with the training
   configuration, training data, and metadata. See the LLM Training Configuration
   guide for input/output features, quantization, and prompt templating.
3. **Track it.** Poll `getTrainingJob` (`GET /job/get/`) for the specific job, or
   `listTrainingJobs` (`GET /job/list/`) for all jobs in the organisation.
4. **Read metrics** (`getTrainingMetrics`, `GET /job/metrics/`) for training curves.
5. **Deploy the result.** Once training succeeds, create a deployment from the
   resulting model repo (see the Simplismart CLI/SDK deployment flow).

## Rules

- Training/compilation consumes GPU quota (default 1x H100 + 1x L40 per org);
  a job that exceeds quota fails to start — request an increase via
  support@simplismart.ai.
- Jobs are keyed by `request_id` / `org_id`; errors return `{ "detail": "..." }`.
