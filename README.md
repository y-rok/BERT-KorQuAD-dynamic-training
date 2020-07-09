# 프로젝트 개요

- BERT를 활용하여 MRC Task 수행
    - KorQuAD 1.0, 2.0 Leaderboard에 모델 등록
    - News Q&A 모델 개발
- https://github.com/enlipleai/kor_pretrain_LM 으로 부터 소스 수정

## Example Scripts
**KorQuAD1.0**

# * Train
```shell
python3 run_qa.py \
  --dynamic_training
  --num_workers 2
  --checkpoint $MODEL_FILE \
  --config_file $CONFIG_FILE \
  --vocab_file $VOCAB_FILE \
  --train_file data/korquad/KorQuAD_v1.0_train.json \
  --max_seq_length 512 \
  --doc_stride 128 \
  --max_query_length 64 \
  --max_answer_length 30 \
  --per_gpu_train_batch_size 16 \
  --learning_rate 5e-5 \
  --num_train_epochs 4.0 \
  --adam_epsilon 1e-6 \
  --warmup_proportion 0.1
```

# * Eval
```shell
python3 eval_qa.py \
  --checkpoint $MODEL_FILE \
  --config_file $CONFIG_FILE \
  --vocab_file $VOCAB_FILE \
  --predict_file data/korquad/KorQuAD_v1.0_dev.json \
  --max_seq_length 512 \
  --doc_stride 64 \
  --max_query_length 64 \
  --max_answer_length 30 \
  --batch_size 16 \
  --n_best_size 20
```
