# TimeLens AI

> Software Engineering course project proposal — Thapar Institute of Engineering and Technology.

**Team:** Vansh Jain (1024030770) · Devyang (1024030765) · Bhavik (1024030772)
**Department:** Computer Engineering

---

## What it is

A personalized time-prediction and adaptive scheduling engine. Instead of trusting a user's self-reported task duration, TimeLens AI learns each user's actual estimation bias, productivity rhythm, and interruption patterns, then predicts realistic completion times and re-sequences the schedule automatically as things change.

## Why

Most task managers treat user-entered durations as ground truth. They ignore:

- Individual estimation bias (people consistently over/underestimate)
- Time-of-day productivity swings
- Context-switching overhead
- Behavioral changes as deadlines approach
- Cascading delays from a schedule that never adapts

## How it works

1. **Ingestion & feature extraction** — pull task category, self-reported estimate, historical bias, timestamp, workload, interruptions, and deadline proximity.
2. **Prediction** — a regression model (starting linear, moving to gradient-boosted as data grows) outputs a predicted duration + confidence interval.
3. **Re-alignment** — schedule automatically re-sequences on task creation, completion, or delay.
4. **Feedback loop** — actual completion times get logged and fed back to keep improving the model.

New users get a population-level prior that gradually shifts toward their personal model as they log more tasks.

## Data Flow

![Data flow diagram](diagrams/data-flow.png)

## User Flow

![User flow diagram](diagrams/user-flow.png)

## Stack

- API: REST, token-based auth
- ML: scikit-learn / XGBoost
- Data: relational store (tasks/history) + feature store (rolling stats)
- Frontend: dashboard for live schedule + bias visualization

## Success metrics

| Metric                    | What it measures                       |
| ------------------------- | -------------------------------------- |
| MAE                       | predicted vs. actual duration          |
| Bias reduction            | variance drop after correction         |
| Schedule adherence        | % tasks completed in projected window  |
| Rebuild latency           | time to recalculate schedule on change |
| Missed-deadline reduction | vs. baseline                           |

## Roadmap

**Phase 1** — task/auth API, baseline bias-correction engine, prediction endpoint + dashboard
**Phase 2** — circadian + interruption modeling, dynamic re-scheduling, automated feedback loop, analytics

## Risks

- **Cold start:** mitigated with population priors that decay toward the personal model
- **Overfitting on sparse data:** regularize small user datasets toward global priors
- **Privacy:** no raw task content stored — only anonymized timestamps, categories, metrics
