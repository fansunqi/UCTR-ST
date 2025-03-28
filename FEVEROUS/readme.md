#### Program Collecting

Download the template file [all_data.json](https://github.com/czyssrs/Logic2Text) , and then run

```python
python process_program.py
```

#### Data Generation

```python
git clone https://github.com/Raldir/FEVEROUS.git
python syn.py
```

For NL-Generator, we use a GPT-2 model from [here](https://github.com/czyssrs/Logic2Text). Please download the model and scripts, and put them into a directory named gpt_base. Our trained model can be downloaded from [google drive](https://drive.google.com/file/d/1YIsQWVK_h2QrkW8VzYa0GDiZ6Qtfx271/view?usp=sharing). Then run:

```python
cd gpt_base
python preprocess.py data_folder GPT_folder
```

After fine-tuning the model, you can generate synthetic claims from collected programs:

```python
sh generate.sh #you may need to set correct model paths in the file
```

#### Model Training&Inference

We use the baseline model from the official repository of [FEVEROUS](https://github.com/Raldir/FEVEROUS), so you can first clone the training scripts. Then download the FEVEROUS data using `./scripts/download_data.sh` and replace the training file with the synthetic file.

For training and inference, please run:
```python
sh train_verdict.sh
sh infer_verdict.sh
```

#### Self-Training Procedure
```
python prepare_for_eval.py  # reformat training file for inference
sh infer_verdict.sh # get pseudo_labels
python prepare_for_st.py    # reformat the pseudo-labeled training file
sh train_verdict.sh #train a student model based on the new training file
```