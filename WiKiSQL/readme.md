### Data Generation 

```python
cd sql2text
```
Firstly please download the [Squall dataset](https://github.com/tzshi/squall). Then run:
```python
python collect_sql2text_from_squall.py
```
and further split the generated data into sql2text_train.json and sql2text_valid.json.
Then we can train a model to convert SQL language to natural language.
```python
sh trainer.sh
```

Then we collect templates from Squall (you can directly use the output file `filtered_commands.jsonl`):
```python
python generate_command_from_squall.py 
```

To synthetic programs from tables, run
```python
python syn.py #It uses train.db of wikisql
sh infer_syn.sh
```
Note: the train.db of wikisql is too large to upload. You can download it from [here](https://github.com/salesforce/WikiSQL/raw/master/data.tar.bz2)

Finally, we aggregate the all samples generated above to form the final data file `train.jsonl`, and put in into the `Table-Pretraining/examples/raw_dataset/wikisql_syn/data` directory.

## Model Training&Inference

```python
cd Table-Pretraining/examples
sh process.sh #process data
sh train_syn.sh
```

## Self-Training Procedure
```
sh eval_train.sh # get pseudo_labels
python utils.py    # reformat the pseudo-labeled training file
sh process.sh   #process the new training file
sh train_st.sh #train a student model based on the new training file
```