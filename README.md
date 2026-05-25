Tramini single-scenario pipeline (Week 1, 06:00-08:00, 2 h).

Inputs:
  tramini/Copy of MANUAL COUNT DATA.xlsx   — 7 daily count sheets (Week 1)
  C:/tprog/traf/t2test/alangilan.net.xml   — reference SUMO network

Intermediates (all under tramini/data/):
  step1_excel_extract/day_<sheet>_raw.csv          (Step 1, per-sheet dump)
  step1_excel_extract/day_<sheet>_period_0608.json (Step 1, period parse)
  step2_averaged.json                              (Step 1, 7-day avg)
  step3_normalized_vph.json                        (Step 1, normalized vph)
  step4_flow_table.csv                             (Step 1, flow audit)
  step5_collect/raw_phase_events.csv               (Step 2, per-event)
  step5_collect/collect_summary.json               (Step 2, counts)
  week1_0608_phases.csv                            (Step 2, master)
  step6_preprocess/cleaned_phases.csv              (Step 3)
  step6_preprocess/features_X.csv                  (Step 3)
  step6_preprocess/labels_y.csv                    (Step 3)
  step6_preprocess/split_summary.json              (Step 3)
  X_train.npy / X_test.npy / y_train.npy / y_test.npy / scaler_params.npz
  models/model_highway.pkl + models/model_gch.pkl  (Step 4)
  step7_train/highway_metrics.json + gch_metrics.json
  week1_0608_adaptive_log.csv                      (Step 5)
  step8_simulate/predictions_trace.csv             (Step 5)
  step9_evaluate/comparison.json                   (Step 6)

Final output: tramini/output/evaluation_summary.csv (one row, week1_0608).
The SUMO scenario folder tramini/output/week1_0608/ holds alangilan_week1_0608.rou.xml
and alangilan_week1_0608.sumocfg from Step 1; Step 2 and Step 5 both replay
this same .sumocfg, the difference being which controller drives the TLS.
