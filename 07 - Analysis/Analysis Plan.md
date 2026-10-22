- [ ] Check evidence completeness, timestamps, and collection coverage.
- [ ] Separate controlled scan/test traffic from ambient traffic.
- [ ] Summarize configuration and exposure for each phase and Windows variant.
- [ ] Report network totals alongside per-endpoint counts and rates.
- [ ] Normalize traffic metrics by actual endpoint observation hours.
- [ ] Compare matched before/after mitigation measurements.
- [ ] Report repeated runs and variation where available.
- [ ] Assess time variation, host count, and configuration differences as alternative explanations.
- [ ] Link each hypothesis assessment to evidence using [[Finding Template]].
- [ ] State what remains uncertain and avoid treating missing logs as zero events.

## Interpretation rules

An open port indicates reachability, not a proven vulnerability. Suspicious traffic does not establish compromise. A blocked attempt does not establish what would have happened without blocking. Claims of successful compromise require direct supporting evidence.

Assess hypotheses as supported, unsupported, or inconclusive within the tested conditions; do not invent results.

