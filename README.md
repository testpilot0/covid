# Predicting Peptide for Vaccine Candidate Solution

This is an interim development repo for Predicting Peptide for Vaccine Candidate Solution. 
This paper provide an overview of proposed pipeline: 
https://github.com/testpilot0/covid/blob/master/Google%20AI%20Platform%20Pipelines_%20Predicting%20COVID%20Peptide%20Vaccine%20Candidates.pdf

  1. Peptide_Prediction_BigQuery_Demo.ipynb - explore peptide data and run BQML to predict peptide bindings. 


  2. AI_Pipeline_MHC_Peptide.ipynb -provide steps to deploy and run AI pipeline for peptide binding.

It allow data scientist quickly plug and explore various models  

(see module: https://github.com/jarokaz/peptide-predict/blob/master/mch_pipeline/model.py  line 87-84)

and run the entire pipeline with a single command. That allows data scientist run multiple computation experiments in parallel and explore various models for outcome prediction.

Third pipile in initial stage of development: 

  COVID_Mutated.ipynb
It is a template to explore virus mutations.

